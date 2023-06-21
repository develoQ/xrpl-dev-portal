---
html: issuing-and-operational-addresses.html
parent: tokens.html
blurb: XRP Ledgerで自動的にトランザクションを送信するビジネスは、リスクを最小限に抑えるために目的ごとに別のアドレスを設定することをおすすめします。
labels:
  - トークン
  - セキュリティ
---

# 発行アドレスと運用アドレス

{% include '_snippets/issuing-and-operational-addresses-intro.ja.md' %}
<!--{#_ #}-->

## 資金のライフサイクル

When a token issuer follows this separation of roles, funds tend to flow in specific directions, as in the following diagram:

図: 発行アドレスからスタンバイアドレス、運用アドレス、顧客アドレスおよびパートナーアドレスに移動し、最後に発行アドレスに戻る資金フロー

The issuing address creates tokens by sending payments to standby addresses. These tokens have negative value from the perspective of the issuing address, since they (often) represent obligations. The same tokens have positive value from other perspectives, including from the perspective of a standby address.

The standby addresses, which are operated by actual humans, send those tokens to operational addresses. This step allows the issuing address to be used as little as possible after this point, while having at least some tokens available on standby.

Operational addresses, which are operated by automated systems, send payments to other counterparties, such as liquidity providers, partners, and other customers. Those counterparties may send funds freely among themselves multiple times.

As always, token payments must "ripple through" the issuer across trust lines.

Eventually, someone sends tokens back to the issuer. This destroys those tokens, reducing the issuer's obligations in the XRP Ledger. If the token is a stablecoin, this is the first step of redeeming the tokens for the corresponding off-ledger assets.


## 発行アドレス

発行アドレスは、金庫に似ています。 パートナーアドレス、顧客アドレス、運用アドレスは、発行アドレスとの間で会計上の関係（トラストライン）を作成しますが、発行アドレスから送信されるトランザクションは可能な限り少ない数に抑えられます。 人間のオペレーターが定期的に、発行アドレスからトランザクションを作成、署名し、スタンバイアドレスまたは運用アドレスの残高を補充します。 このようなトランザクションの署名に使用されるシークレットキーには、インターネットに接続されたどのコンピューターからもアクセスできないことが極めて重要です。

金庫とは異なり、発行アドレスは顧客またはパートナーからのペイメントを直接受領できます。 XRP Ledgerのトランザクションはすべて公開されているため、自動システムは発行アドレスからのペイメントを監視する際にシークレットキーを必要としません。

### 発行アドレスの漏えい

If a malicious actor learns the secret key behind a institution's issuing address, that actor can create new tokens and send them to users or trade them in the decentralized exchange. This can make a stablecoin issuer insolvent. It can become difficult for the financial institution to distinguish legitimately-obtained tokens and redeem them fairly. 金融機関の発行アドレスが乗っ取られた場合には、金融機関が新たに発行アドレスを作成する必要があり、また古い発行アドレスと会計上の関係を有するすべてのユーザーは新しいアドレスで、アカウントとの関係を新たに作成する必要があります。

### 複数の発行アドレス

金融機関はXRP Ledgerで1つの発行アドレスから複数の通貨を発行できます。 ただし、いくつかの設定（[送金手数料](transfer-fees.html)のパーセンテージや[Global Freeze](freezes.html)の状況など）は、1つのアドレスから発行されるすべての通貨に同様に適用されます。 金融機関が通貨ごとに設定を変えて柔軟に管理したい場合、金融機関は通貨ごとに異なる発行アドレスを使用する必要があります。


## 運用アドレス

運用アドレスはレジに似ています。 イシュアンスを顧客とパートナーに送信して、金融機関に代わってペイメントを行います。 トランザクションに自動的に署名するには、運用アドレスのシークレットキーをインターネットに接続されたサーバーに保管する必要があります。 （シークレットキーは暗号化して保管できますが、サーバーがトランザクションに署名する際にシークレットキーを暗号化解除する必要があります。 ）顧客とパートナーは、運用アドレスとの会計上の関係を作成すべきではありません。

各運用アドレスではイシュアンスの残高が限られています。 運用アドレスの残高が少なくなると、金融機関は残高を補充するため、発行アドレスまたはスタンバイアドレスから送金します。

### 運用アドレスの漏えい

不正使用者が運用アドレスのシークレットキーを入手した場合に金融機関が失う可能性のある通貨額は、最大でも運用アドレスが保有している額までです。 金融機関は、顧客やパートナーからのアクションなしに、新しい運用アドレスに切り替えることができます。


## スタンバイアドレス

金融機関がリスクと利便性のバランスを保つためのもう1つの手段として、発行アドレスと運用アドレスの中間ステップとして「スタンバイアドレス」を利用することができます。 The institution can fund additional XRP Ledger addresses as standby addresses, whose keys are not available to always-online servers, but are entrusted to different trusted users.

When an operational address is running low on funds (either tokens or XRP), a trusted user can use a standby address to refill the operational address's balance. When a standby addresses run low on funds, the institution can use the issuing address to send more funds to a standby address in a single transaction, and the standby addresses can distribute those funds among themselves if necessary. これにより発行アドレスのセキュリティが強化され、発行アドレスが実行する合計トランザクション数が減少し、1つの自動化システムの管理下に過剰な資金が残ることがなくなります。

運用アドレスと同様に、スタンバイアドレスは顧客やパートナーではなく発行アドレスとの間に会計上の関係を確立する必要があります。 運用アドレスに適用される注意事項はすべてスタンバイアドレスにも適用されます。

### Standby Address Compromise

スタンバイアドレスの漏えいが発生した場合、運用アドレスが漏えいした場合と同様の影響が及びます。 不正使用者がスタンバイアドレスに保有される残高を盗むことが可能となり、金融機関は顧客やパートナーからのアクションなしに新しいスタンバイアドレスに切り替えることができます。


## See Also

- **コンセプト:**
    - [アカウント](accounts.html)
    - [暗号鍵](cryptographic-keys.html)
- **チュートリアル:**
    - [XRP Ledgerゲートウェイの開設](become-an-xrp-ledger-gateway.html)
    - [レギュラーキーペアの割り当て](assign-a-regular-key-pair.html)
    - [レギュラーキーペアの変更または削除](change-or-remove-a-regular-key-pair.html)
- **リファレンス:**
    - \[account_infoメソッド\]\[\]
    - \[SetRegularKeyトランザクション\]\[\]
    - [AccountRootオブジェクト](accountroot.html)


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
