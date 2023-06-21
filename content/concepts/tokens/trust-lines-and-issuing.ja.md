---
html: trust-lines-and-issuing.html
parent: tokens.html
blurb: Learn about the properties and rationale of trust lines.
labels:
  - トークン
---

# Trust Lines and Issuing

トラストラインとは、XRP Ledgerにおける[トークン](tokens.html)を保持するための仕組みを指します。 トラストラインは、XRP Ledgerのルールである「不要なトークンを他者に保有させることはできない」という原則を強制するものです。 この制限は、XRP Ledgerのユースケースである[コミュニティクレジット](tokens.html#コミュニティクレジット)などを実現するために不可欠なものです。

それぞれの「トラストライン」は、以下のような _双方向_ の関係から成り立っています。

- トラストラインが接続する **2つの[アカウント](accounts.html)** の識別子
- 一方のアカウントから見てプラス、他方のアカウントから見てマイナスとなる、単一の共有された**残高**
    - 残高がマイナスのアカウントは、一般的にトークンの「発行者」とみなされます。 ただし、[API](http-websocket-apis.html)では、`issuer`という名称はどちらを指すこともあるようです。
- 様々な **設定** とメタデータ。 2つのアカウントの _それぞれ_ は、トラストライン上の設定を制御することができます。
    - 最も重要なことは、各サイドがトラストラインに **限度額** を設定できることです。 これはデフォルトでは0です。 各アカウントの残高は(トラストラインから見て)そのアカウントの上限を超えることはできません。 ただし、[アカウント自身の操作](#限度額以上を保有する)を除きます。

各トラストラインは、特定の\[通貨コード\]\[\]に固有です。 2つのアカウント間に作成できる各種通貨コードのトラストラインの数に制限はありませんが、どの通貨コードについても、作成できるトラストラインは1方向に1つだけです。


## 作成

アカウントはいずれも、ゼロでない上限と独自の設定を持つ\[TrustSetトランザクション\]\[\]を送信することによって、他のアカウントに対して一方的にトークンを「トラスト」することができます。 これによって、残高ゼロのトラストラインが作成され、相手側の設定がデフォルトとして設定されます。

トラストラインは、[分散型取引所](decentralized-exchange.html)でトークンを購入するときなど、いくつかのトランザクションによって暗黙的に作成されることがあります。 この場合、トラストラインはデフォルト設定をそのまま使用します。


## 限度額以上を保有する

トラストラインの限度額よりも _大きい_ 残高を保有できるケースは次の3つがあります。

1. [トレード](decentralized-exchange.html)によって、限度額以上のトークンを取得した場合
2. When you decrease the limit on a trust line that has a positive balance.
3. When you acquire more of that token by [cashing a Check](checks.html). [チェックの現金化](checks.html)によって、トークンを限度額以上取得する場合 (_\[CheckCashMakesTrustLine amendment\]\[\]が必要です。 _)


## トラストラインの設定

In addition to the shared balance, each account has its own settings on the trust line, which consist of the following:

- **Limit**: 0から[トークンの上限量](currency-formats.html)の範囲内の数字です。 支払いや他のアカウントの操作によって、(このアカウントから見た)トラストラインの残高が限度額を超えることはできません。 デフォルトは`0`です。
- **Authorized**: [Authorized Trust Lines](authorized-trust-lines.html)と併用し、このアカウントが発行するトークンを相手側に保持させることを許可するための値(true/false)です。 デフォルトは`false`です。 一度`true`に設定すると、元に戻すことはできません。
- **No Ripple**: トークンがこのトラストラインを通過して[ripple](rippling.html)するかどうかを設定するための値(true/false)です。 デフォルトはアカウントの"Default Ripple"設定に依存します。 新しいアカウントでは"Default Ripple"はoffで、つまり`true`がNo Rippleのデフォルト値となります。 Usually, issuers should allow rippling and non-issuers should disable rippling unless they are using trust lines for community credit.
- **Freeze**: このトラストラインに[個別の凍結](freezes.html#individual-freeze)が適用されているかどうかを示す値(true/false)です。 デフォルトは`false です。
- **Quality In** および **Quality Out**: この設定により、このトラストライン上の他のアカウントで発行されたトークンを額面より少なく（または多く）評価することができます。 たとえば、ステーブルコインの発行者が、オフレッジャーにある同等の資産に対してトークンの引き出しに3%の手数料を課している場合、この設定を使用して、それらのトークンを額面の97%で評価することが可能です。 デフォルトは`0`で、額面価格を表しています。


## 準備金と削除

トラストラインは台帳のスペースを使用するため、[トラストラインはあなたのアカウントが準備金として保持しなければならないXRPを増加させます](reserves.html)。 Either or both accounts in the trust line may be charged the reserve for the trust line, depending on the status of the trust line: if any of your settings are not the default, or if you hold a positive balance, it counts as one item toward your owner reserve.

Generally, this means that the account that created the trust line is responsible for the reserve and the issuer is not. <!-- STYLE_OVERRIDE: is responsible for -->

Trust lines are automatically deleted if both sides' settings are in the default state and the balance is 0. This means that, to delete a trust line, you need to:

1. 設定した内容をデフォルトに戻すために、\[TrustSetトランザクション\]\[\]を送信する。
2. トラストラインにあるプラスの残高をすべて処分します。 これは[支払い](cross-currency-payments.html)によって通貨を送るか、[分散型取引所](decentralized-exchange.html)で通貨を売却することで可能です。

残高がマイナス（あなたが発行者）の場合や、相手側の設定が初期状態でない場合、トラストラインを完全に削除させることはできませんが、同様の手順で所有者準備金にカウントされないようにすることが可能です。

**Authorized** の設定は、一度オンにするとオフにできないため、トラストラインの初期状態にはカウントされません。

### Free Trust Lines
[[Source]](https://github.com/ripple/rippled/blob/72377e7bf25c4eaee5174186d2db3c6b4210946f/src/ripple/app/tx/impl/SetTrust.cpp#L148-L168)

Since trust lines are a powerful feature of the XRP Ledger, there is a special feature to make an account's first two trust lines "free".

アカウントが新しいトラストラインを作成する際、台帳の中で新しいトラストラインを含む最大2つのオブジェクトを所有している場合、アカウントの所有者準備金は通常の量ではなく、0として扱われます。 これにより、アカウントが台帳内のオブジェクトを所有するために必要な準備金の増加分を満たすだけのXRPを保有していない場合でも、取引を成功させることができます。

アカウントが台帳に3つ以上のオブジェクトを所有している場合、所有者準備金が全額適用されます。


## See Also

- **コンセプト:**
    - [Decentralized Exchange](decentralized-exchange.html)
    - [Rippling](rippling.html)
- **チュートリアル:**
    - [XRP Ledgerゲートウェイの開設](become-an-xrp-ledger-gateway.html)
- **リファレンス:**
    - \[account_linesメソッド\]\[\] - 指定されたアカウントに関連付けられたトラストラインを確認
    - \[gateway_balancesメソッド\]\[\] - 発行者の発行残高を確認
    - [RippleStateオブジェクト](ripplestate.html) - 台帳の状態データのうち、トラストラインのデータ形式
    - \[TrustSetトランザクション\]\[\] - トラストラインを作成・変更するトランザクション

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
