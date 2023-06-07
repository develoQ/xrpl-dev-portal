---
html: xrp.html
parent: introduction.html
blurb: 送金のためのデジタルアセットである、XRPの使い方と特性について学びましょう。
labels:
  - XRP
---
# XRP

**XRP**は、XRPレジャーのネイティブ暗号資産です。XRPレジャーのすべての[アカウント](accounts.html)間で相互にXRPを送金できます。アカウントは、最小限度額のXRPを[準備金](reserves.html)として保有する必要があります。XRPレジャーアドレス間にてXRPの直接送金が可能で、ゲートウェイや流動性プロバイダーを必要としません。このため、XRPは便利なブリッジ通貨となりました。

XRPレジャーの高度な機能の一部（[Escrow](escrow.html)や[Payment Channel](use-payment-channels.html)など）は、XRPでのみ使えます。オーダーブックの[オートブリッジング](autobridging.html)は、XRPを使用して、2つの発行済み通貨のオーダーブックをXRPオーダーブックにマージして、合成された一つのオーダーブックを作成することで、分散型取引所の流動性を高めます。（たとえば、オートブリッジングによりUSD:XRPオーダーとXRP:EURオーダーがマッチングされ、USD:EURオーダーブックとなります。）

XRPはまた、ネットワークのスパムの防御対策としても機能します。すべてのXRPレジャーアドレスには、XRPレジャーの維持管理コストを支払うために少額のXRPが必要です。[トランザクションコスト](transaction-cost.html)と[準備金](reserves.html)は、XRP建ての中立的な手数料であり、いかなる当事者にも支払われません。レジャーのデータフォーマットで、XRPは[AccountRootオブジェクト](accountroot.html)に保管されます。

XRPの特性のいくつかは、XRPレジャーの性質とその[コンセンサスプロセス](consensus.html)に由来しています。XRPレジャーはマイニングを必要とせず、コンセンサスプロセスは不変性のために複数のconfirmationsを必要としないため、XRPレジャーはビットコインや他のトップクラスの暗号通貨よりも高速で効率的にトランザクションを処理することができます。

## XRPの特性

一番最初のレジャーにて1000億XRPが発行され、これ以上新しいXRPは作成できません。XRPは、[トランザクションコスト](transaction-cost.html)によって消却されるか、またはキーの所有者がいないアドレスに送金すると失われることがあります。このため、XRPは本質的にはやや[デフレ通貨](https://ja.wikipedia.org/wiki/デフレーション)です。XRPがなくなることを心配する必要はありません。現時点の消却のペースでは、すべてのXRPが消却されるまでに約7万年かかります。またXRPの総供給量の変化に伴い、XRPの[価格と手数料が調整される可能性があります](fee-voting.html)。

技術的には、XRPは0.000001XRPの単位まで正確に計算され、その単位は「Drop」と呼ばれます。[`rippled`API](http-websocket-apis.html)では、XRPの量は常にXRPのdrop単位で指定する必要があります。たとえば1XRPは`1000000`dropと表されます。詳細については、[通貨フォーマットのリファレンス](currency-formats.html)を参照してください。

## 歴史

### XRPの販売

XRPレジャーは、2011年から2012年初頭にかけて、Jed McCaleb、Arthur Britto、David Schwartzによって構築されました。2012年9月、JedとArthurはChris LarsenとともにRipple（当時はOpenCoin Inc.という会社）を設立し、Ripple社がXRPレジャー上で開発を行う代わりに800億XRPをRipple社に寄付することを決定しました。2017年、同社は[550億XRPをエスクローへ預託](https://ripple.com/insights/ripple-escrows-55-billion-xrp-for-supply-predictability/)し、一般的な供給量の[増加が予測可能](https://ripple.com/insights/ripple-to-place-55-billion-xrp-in-escrow-to-ensure-certainty-into-total-xrp-supply/)になることを保証しました。 当面の間。Ripple社の[XRPマーケットパフォーマンスサイト](https://ripple.com/xrp/market-performance/)では、同社が現在利用可能でエスクローにロックされているXRPの量を報告しています。

### 命名

元々、XRPレジャーは、その技術が支払いを[複数のホップや通貨を波及(ripple)する](rippling.html)ことを可能にすることから「リップル」と呼ばれていました。台帳に組み込まれたネイティブアセットには、「ripple credits」または「ripples」という用語と、ISO 4217規格の非国家通貨を表す接頭語「X」から、クリエイターが選んだティッカーシンボル「XRP」が採用されました。3人の開発者がXRPを寄付した会社は「Ripple Labs」として登録されました。技術や会社の類似名称との混同を避けるため、あらゆる文脈で資産を指す名称として「XRP」が使われるようになり、最終的には彼らの会社の名称を「Ripple」と短縮しました。2018年5月、それまで会社(Ripple)とデジタル資産(XRP)の両方に使われていたトリスケリオンのロゴと区別するために、[コミュニティは、XRPを表す新しい「X」記号を選定](https://twitter.com/xrpsymbol/status/1006925937571713025)しました。

| XRPの"X"ロゴ                           | Ripple社のトリスケリオン                     |
|:--------------------------------------|:------------------------------------------|
| !["X"ロゴ](assets/img/xrp-x-logo.png) | ![トリスケリオン](img/ripple-triskelion.png) |


XRPの最小で不可分の単位は、RippleのフォーラムメンバーであるThePiachuの提案により「drop」と名付けられました。初期の代替用語は、Jed McCalebにちなんで「jed」でした。

## 関連項目

- [XRPの送信(対話型チュートリアル))](send-xrp.html)
- [取引所へのXRPの上場](list-xrp-as-an-exchange.html)
- [rippled APIにおける通貨フォーマット](currency-formats.html)
