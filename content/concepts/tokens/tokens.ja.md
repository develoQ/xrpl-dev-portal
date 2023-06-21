---
parent: concepts.html
blurb: XRP Ledger上でデジタルな価値を表すトークンを作成することができます。
labels:
  - トークン
---

# トークン

XRP以外のすべての資産は、XRP Ledgerでは **トークン** として扱うことができます。 通常のトークンは、アカウント間の[トラストライン](trust-lines-and-issuing.html) と呼ばれる関係で管理されます。 すべてのアカウントは、トークンを保有することを許可する他のアカウントにあトークンを発行できますが、トークンを必要としないアカウントに一方的にトークンを配付することはできません。 トークンは、台帳の外に存在する資産に裏付けられた「ステーブルコイン」、XRP Ledger上で独自に作成された純粋なデジタルトークン、コミュニティクレジットなど、様々な種類の価値を表すことが出来ます。

**注記:** XRP Ledger上のトークンは、過去に「IOUs」（[I-owe-you](https://en.wikipedia.org/wiki/IOU)の略）および「発行済み通貨」とも呼ばれてきました。 しかし、これらの呼称は、XRP Ledgerのトークンが表すことのできるデジタル資産の全範囲をカバーしていないため、望ましくないとされています。 <!-- STYLE_OVERRIDE: ious -->

Standard tokens are fungible: meaning, all units of that token are interchangeable and indistinguishable. XRP Ledgerでのネイティブ対応の詳細については、[非代替トークン(NFT)](non-fungible-tokens.html)を参照してください。

トークンは[複数通貨間の支払い](cross-currency-payments.html)に使用でき、[分散型取引所(DEX)](decentralized-exchange.html)で取引することができます。

トラストラインの残高は、どちら側から見るかによって、プラスまたはマイナスで表されます。 マイナスの残高を持つ側は「発行者」と呼ばれ、そのトークンに関するいくつかの機能を設定することができます。 発行者ではない別のアカウントにトークンを送ると、それらのトークンは発行者、場合によっては同じ通貨コードを使用している他のアカウントに「ripple」します。 これは便利な場合もありますが、想定外の挙動を引き起こす可能性もあります。 トラストラインに[No Ripple flag](rippling.html)を使用すると、トラストラインがripplingしないように設定することができます。


## Stablecoins

XRP Ledger におけるトークンの代表的なモデルとして、発行者が XRP Ledgerの外部に価値ある資産を保有し、その価値を表すトークンをLedger上で発行するというものがあります。 このタイプの発行者は、そのサービスを通じてXRP Ledgerに通貨を送受信できることから、 _ゲートウェイ_ と呼ばれることもあります。 If the assets that back a token use the same amounts and denomination as the tokens in the ledger, that token can be considered a "stablecoin" because—in theory—the exchange rate between that token and its off-ledger representation should be stable at 1:1.

ステーブルコインの発行者は、XRP Ledgerの外側において、トークンを実際の通貨や資産と交換するための _入金_ と _出金_ のサービスを提供する必要があります。

実際には、XRP Ledger はただのシステムであり、その外側にいかなるルールも適用することはできません。 そのため、XRP Ledger上のステーブルコインは、その発行者を信頼し、その発行者が要求に応じてトークンを現物資産へ交換することができなければ、そのステーブルコインの価値が維持されないと考えるべきでしょう。 ユーザは、誰がトークンを発行しているのか、信頼できるのか、合法的なのか、支払能力があるのか、という点について十分に注意をしなければなりません。 信頼できない場合は、そのトークンを保有するべきではないでしょう。

ゲートウェイの運営方法については、[XRP Ledger Gatewayの開設](become-an-xrp-ledger-gateway.html)をご覧ください。


## コミュニティクレジット

XRP Ledgerのもう一つの利用方法として、「コミュニティクレジット」という、知人同士がXRP Ledgerを利用して、誰が誰にいくら借金があるのかを把握する仕組みがあります。 この借金を自動的かつアトミックに活用し、[rippling](rippling.html)を通じて支払いを決済できるのが、XRP Ledgerの優れた機能です。

例えば、AsheeshがMarcusに20ドル、MarcusがBharathに50ドルの借金がある場合、BharathはAsheeshのMarcusに対する借金を帳消しにする代わりに、その分のMarcusに対する借金を帳消しすることによってAsheeshに20ドルを「支払う」ことができる。 逆もまた可能である。 AsheeshはMarcusを通してBharathに支払うことで、それぞれの負債を減らすことができるのです。 XRP Ledgerは、このように複雑な連鎖的な取引を、中間にいるアカウントが何もせずとも、単一の取引で決済することができるのです。

このタイプの使用法については、[paths](paths.html)を参照してください。 <!--{# TODO: It would be nice to be able to link to a page with more illustrative examples of community credit. #}-->


## その他のトークン

XRP Ledgerで発行されるトークンには、その他にも使用例があります。 例えば、セカンダリアドレスに一定数量の通貨を発行し、発行者に「キーを渡す」ことで、「ICO（Initial Coin Offering）」を行うことができます。

**警告:** ICOは米国では[証券と見なされ、規制対象となる](https://www.sec.gov/oiea/investor-alerts-and-bulletins/ib_coinofferings)可能性があります。 <!-- SPELLING_IGNORE: ico, icos -->

金融サービスビジネスを始める前に、関連規制を調査されることを強くお勧めします。


## トークンの特性

XRP Ledgerにおけるトークンは、[XRPと異なる性質](currency-formats.html#comparison)を持ちます。 トークンは常に _トラストライン内_ に存在し、トークンのすべての移動はトラストラインに沿って行われます。 You cannot cause someone else's account to hold more of a token than the _limit_ configured on their trust line. (自分のトラストラインを制限以上に増やすことは _可能_ です。 例えば、[分散型取引所](decentralized-exchange.html)でさらに購入したり、すでにプラスの残高がある状態で上限値を下げたりすることができます。

トークンは、精度が15桁の10進数（基数10）と指数を用いて、非常に大きな値（最大9999999999999999×10<sup>80</sup>）から、非常に小さな値（最小1.0×10<sup>-81</sup>まで）を表現することができます。

必要なトラストラインが設定されていれば、誰でも\[Paymentトランザクション\]\[\]を送信することでトークンを発行することができます。 トークンを発行者に送り返せば、トークンを「burn」することができます。 また、発行者の設定により、[複数通貨間の支払い](cross-currency-payments.html)やトレードでトークンをさらに生み出せるケースもあります。

発行者は、ユーザがトークンを送金する際に自動で差し引かれる「送金手数料」(transfer-fees.html)を設定することができます。 発行者は、自分のトークンを含む取引レートの[ティックサイズ](ticksize.html)を定義することもできます。 発行者と一般アカウントのどちらも、トラストラインを[凍結](freezes.html)することができ、トラストライン内のトークンの使用方法を制限することができます。 ( XRPにはこのいずれも適用されません。 )

トークン発行の技術的な手順については、[代替可能トークンの発行](issue-a-fungible-token.html) を参照してください。


## See Also

- **コンセプト:**
    - [XRP](xrp.html)
    - [Cross-Currency Payments](cross-currency-payments.html)
    - [Decentralized Exchange](decentralized-exchange.html)
- **チュートリアル:**
    - [代替可能トークンの発行](issue-a-fungible-token.html)
    - [XRP Ledgerゲートウェイの開設](become-an-xrp-ledger-gateway.html)
    - [トランザクションの結果の確認](look-up-transaction-results.html)
    - [専門化した支払いタイプの使用](use-specialized-payment-types.html)
- **リファレンス:**
    - \[Paymentトランザクション\]\[\]
    - \[TrustSetトランザクション\]\[\]
    - [RippleStateオブジェクト](ripplestate.html)
    - \[account_linesメソッド\]\[\]
    - \[account_currenciesメソッド\]\[\]
    - \[gateway_balancesメソッド\]\[\]

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
