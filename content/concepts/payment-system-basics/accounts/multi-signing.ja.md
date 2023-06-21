---
html: multi-signing.html
parent: accounts.html
blurb: マルチ署名を使用することで、トランザクション送信時のセキュリティが強化されます。
labels:
  - スマートコントラクト
  - セキュリティ
---

# マルチ署名

マルチ署名は、複数のシークレットキーを組み合わせて使用してXRP Ledgerの[トランザクションを承認する](transaction-basics.html#トランザクションの承認)手法です。 アドレスで有効な承認手法（マルチ署名、[マスターキーペア](cryptographic-keys.html#マスターキーペア)、[レギュラーキーペア](cryptographic-keys.html#レギュラーキーペア)など）を自由に組み合わせて使用できます。 （唯一の要件は、 _少なくとも1つの_ 手法を有効にする必要があることです。 ）

マルチ署名には次のメリットがあります。

* 複数のデバイスからのキーを要求できます。 これにより、不正使用者があなたの代わりにトランザクションを送信するには複数のマシンを悪用しなければならなくなります。
* 複数のユーザー間で1つのアドレスの管理を共有できます。 この場合、各ユーザーが、そのアドレスからトランザクションを送信する際に必要な複数のキーのいずれか1つだけを所有します。
* あなたのアドレスからトランザクションを送信できる権限を、複数ユーザーのグループに委任できます。 委任を受けた各ユーザーは、あなたが通常の方法で署名できない場合にあなたのアドレスを制御できます。
* ... and more.

## 署名者リスト

マルチ署名を使用するには、あなたの代理として署名できるアドレスのリストを作成する必要があります。

\[SignerListSetトランザクション\]\[\]は、あなたのアドレスからのトランザクションを承認できるアドレスを定義します。 SignerListには最大8個のアドレスを指定できます。 The list cannot include your address and there can be no duplicate entries. SignerListのquorum値とweight値を使用して、必要な署名の数と組み合わせを制御できます。

_(Updated by the \[ExpandedSignerList amendment\]\[\].)_

### Signer Weight

You assign a weight to each signer in the list. The weight represents the authority of the signer relative to other signers on the list. The higher the value, the more authority. Individual weight values cannot exceed 2<sup>16</sup>-1.

### Quorum

The quorum value of a list is the minimum weight total required to authorize a transaction. The quorum must be greater than 0 but less than or equal to the sum of the weight values in the signer list: meaning, it must be possible to achieve a quorum with the given signer weights.

### Wallet Locator
<!-- STYLE_OVERRIDE: wallet -->

You can also add up to 256 bits of arbitrary data to each signer's entry in the list. This data is not required or used by the network, but can be used by smart contracts or other applications to identify or confirm other data about the signers.

_(Added by the \[ExpandedSignerList amendment\]\[\].)_


### Examples Using Signer Weight and Quorum

The weights and quorum allow you to set an appropriate level of oversight for each transaction, based on the relative trust and authority relegated to responsible participants who manage the account.

For a shared account use case, you might create a list with a quorum of 1, then give all participants a weight of 1. A single approval from any one of them is all that is required to approve a transaction.

For a very important account, you might set the quorum to 3, with 3 participants that have a weight of 1. All of the participants must agree and approve each transaction.

Another account might also have a quorum of 3. You assign your CEO a weight of 3, 3 Vice Presidents a weight of 2 each, and 3 Directors a weight of 1 each. To approve a transaction for this account requires the approval of all 3 Directors (total weight of 3), 1 Vice President and 1 Director (total weight of 3), 2 Vice Presidents (total weight of 4), or the CEO (total weight of 3). <!-- STYLE_OVERRIDE: vice -->

In each of the previous three use cases, you would disable the master key without configuring a regular key, so that multi-signing is the only way of [authorizing transactions](transaction-basics.html#authorizing-transactions).

There might be a scenario where you create a multi-signing list as a "backup plan." The account owner normally uses a regular key for their transactions (not a multi-signing key). For safety, the owner adds a signer list containing 3 friends, each with a weight of 1, and a quorum of 3. If the account owner were to lose the private key, they can ask their friends to multi-sign a transaction to replace the regular key.


## マルチ署名済みトランザクションの送信

マルチ署名済みトランザクションを正常に送信するには、以下のすべての条件を満たす必要があります。

* The address sending the transaction (specified in the `Account` field) must have a [`SignerList` object in the ledger](signerlist.html). For instructions on how to do this, see [Set Up Multi-Signing](set-up-multi-signing.html).
* トランザクションに`SigningPubKey`フィールドを空の文字列として含める必要があります。
* トランザクションに、署名の配列が指定されている[`Signers`フィールド](transaction-common-fields.html#signersフィールド)を含める必要があります。
* `Signers`配列に含まれている署名は、SignerListで定義されている署名と一致している必要があります。
* 指定された署名で、これらの署名者に関連付けられている`weight`の合計が、SignerListの`quorum`以上である必要があります。
* [トランザクションコスト](transaction-cost.html)（`Fee`フィールドで指定）は、通常のトランザクションコストの（N+1）倍以上である必要があります。 このNは、指定される署名の数です。
* トランザクションのすべてのフィールドは、署名収集前に定義する必要があります。 フィールドの[自動入力](transaction-common-fields.html#自動入力可能なフィールド)は実行できません。
* `Signers` 配列がバイナリ形式で指定される場合、この配列は署名者アドレスの数値に基づいて、低い値から順にソートされている必要があります。 （JSONとして提出される場合は、\[submit_multisignedメソッド\]\[\] がこの処理を自動的に実行します。 ）

## See Also

- **Tutorials:**
    - [Set Up Multi-Signing](set-up-multi-signing.html)
    - [トランザクションを送信するアドレス（`Account`に指定されるアドレス）は、[レジャーに`SignerList`](signerlist.html)を所有する必要があります。](send-a-multi-signed-transaction.html)
- **Concepts:**
    - [Cryptographic Keys](cryptographic-keys.html)
    - [Special Transaction Cost for Multi-signed transactions](transaction-cost.html#special-transaction-costs)
- **References:**
    - \[SignerListSet transaction\]\[\]
    - [SignerList object](signerlist.html)
    - \[sign_for method\]\[\]
    - \[submit_multisigned method\]\[\]

{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
