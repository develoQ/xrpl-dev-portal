---
html: amendments.html
parent: consensus-network.html
blurb: Amendmentはトランザクション処理の新しい機能やその他の変更を指します。 バリデータはコンセンサスを通して連携し、XRP Ledgerにこれらのアップグレードを順序正しく適用します。
labels:
  - Blockchain
---

# Amendment

The amendment system uses the consensus process to approve any changes that affect transaction processing on the XRP Ledger. Fully-functional, transaction process changes are introduced as amendments; validators then vote on these changes. If an amendment receives more than 80% support for two weeks, the amendment passes and the change applies permanently to all subsequent ledger versions. Disabling a passed amendment requires a new amendment to do so.

**Note:** Bug fixes that change transaction processes also require amendments.

<!-- TODO: Move this to an amendment tutorial.
Every amendment has a unique identifying hex value and a short name. The short name is for readability only; servers can use different names to describe the same amendement ID, and the names aren't guaranteed to be unique. The amendment ID should be the SHA-512Half hash of the amendment's short name.
-->

## Amendmentプロセス

Every 256th ledger is called a **flag** ledger. The flag ledger doesn't have special contents, but the amendment process happens around it.

1. **Flag Ledger -1:** When `rippled` validators send validation messages, they also submit their amendment votes.
2. **Flag Ledger:** Servers interpret the votes from trusted validators.
3. **注意:** `rippled` 0.80.0より前のバージョンでは、サーバーがAmendment blockedである場合でも`amendment_blocked`フィールドは含まれません。
    * `tfGotMajority`フラグは、Amendmentのサポートが、信頼できるバリデータの80%以上に増加したことを意味します。
    * `tfLostMajority`フラグは、Amendmentのサポートが、信頼できるバリデータの80%未満に減少したことを意味します。
    * No flag means the amendment is enabled.

    Amendmentを適用するには通常、**80%のサポートが2週間にわたって**必要になります。 In these cases, an `EnableAmendment` pseudo-transactions is added for both scenarios, but the amendment is ultimately enabled.

4. **Flag Ledger +2:** Enabled amendments apply to transactions on this ledger onwards.


## Amendmentについて

`rippled`の各バージョンは、既知のAmendmentのリストとそれらのAmendmentを実装するためのコードでコンパイルされています。 Operators of `rippled` validators configure their servers to vote on each amendment and can change it at any time. If the operator doesn't choose a vote, the server uses a default vote defined by the source code.

**Note:** The default vote can change between software releases. \[Updated in: rippled 1.8.1\]\[\]

Amendments must maintain two weeks of support from more than 80% of trusted validators to be enabled. Amendmentが信頼済みのバリデータの少なくとも80%のサポートを取り戻した場合は、そこから2週間の期間が再スタートします。 Amendments can gain and lose a majority any number of times before it becomes permanently enabled.

Amendments that have had their source code removed without being enabled are considered **Vetoed** by the network.


## Amendment blocked
<a id="amendment-blocked"></a>

Amendment blockedは、XRP Ledgerに依存するアプリケーションを保護するためのセキュリティー機能です。 Amendmentが有効になっている場合の`rippled`の動作を確認するには、そのAmendmentが実稼働ネットワークで有効になる前に、`rippled`の構成ファイルを実行して強制的に機能を有効にします。 Rather than guess and misinterpret ledger data, these servers become **amendment blocked** and can't:

* Determine the validity of a ledger.
* トランザクションを送信または処理できない
* Participate in the consensus process.
* Vote on future amendments.

`rippled`サーバーが賛成票または反対票を投じるように設定されているAmendmentは、そのサーバーがAmendment blockedの状態になるかどうかに影響を与えません。 `rippled`サーバーは、可能な限り、ネットワークの他の部分によって有効なった一連のAmendmentに従います。 This means you can also become amendment blocked if you connect your server to a parallel network with different amendments enabled. For example, the XRP Ledger Devnet typically has experimental amendments enabled. If you are using the latest production release, your server likely won't have the code for those experimental amendments.

最新バージョンよりも古いバージョンにアップグレードして`rippled`サーバーのブロックを解除できるかどうかを判断するには、サーバーをブロックしている機能を調べ、そのブロックしている機能をサポートしている`rippled`バージョンを調べます。


## Amendment投票

When amendments are enabled, the source code for pre-amendment behaviors remain in `rippled`. While there are use-cases for keeping old code, such as reconstructing ledger outcomes for verification, tracking amendments and legacy code adds complexity over time.

The [XRP Ledger Standard 11d](https://github.com/XRPLF/XRPL-Standards/discussions/19) defines a process for retiring old amendments and associated pre-amendment code. After an amendment has been enabled on the Mainnet for two years, it can be retired. Retiring an amendment makes it part of the core protocol unconditionally; it's no longer tracked or treated as an amendment, and all pre-amendment code is removed.


## See Also

- **Concepts:**
    - [Introduction to Consensus](intro-to-consensus.html)
- **Tutorials:**
    - [Run rippled as a Validator](run-rippled-as-a-validator.html)
    - [Amendment投票の設定](configure-amendment-voting.html)
    - [Contribute Code to the XRP Ledger](contribute-code.html)


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
