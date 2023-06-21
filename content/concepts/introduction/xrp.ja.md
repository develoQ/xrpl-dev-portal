---
html: xrp.html
parent: introduction.html
blurb: 送金のためのデジタルアセットである、XRPの使い方と特性について学びましょう。
labels:
  - XRP
---

# XRP

**XRP**は、XRP Ledgerのネイティブ暗号資産です。 XRP Ledgerのすべての[アカウント](accounts.html)間で相互にXRPを送金できます。 アカウントは、最小限度額のXRPを[準備金](reserves.html)として保有する必要があります。 XRP Ledgerアドレス間にてXRPの直接送金が可能で、ゲートウェイや流動性プロバイダーを必要としません。 このため、XRPは便利なブリッジ通貨となりました。

XRP Ledgerの高度機能の一部（[Escrow](escrow.html)や[Payment Channel](use-payment-channels.html)など）は、XRPでのみ使えます。 オーダーブックの[オートブリッジング](autobridging.html)は、XRPを使用して、2つの発行済み通貨のオーダーブックをXRPオーダーブックにマージして、合成された一つのオーダーブックを作成することで、分散型取引所の流動性を高めます。 （たとえば、オートブリッジングによりUSD:XRPオーダーとXRP:EURオーダーがマッチングされ、USD:EURオーダーブックとなります。 ）

XRPはまた、ネットワークのスパムの防御対策としても機能します。 すべてのXRP Ledgerアドレスには、XRP Ledger維持管理コストを支払うために少額のXRPが必要です。 [トランザクションコスト](transaction-cost.html)と[準備金](reserves.html)は、XRP建ての中立的な手数料であり、どの当事者にも支払われません。 レジャーのデータフォーマットで、XRPは[AccountRootオブジェクト](accountroot.html)に保管されます。

Some of the desirable properties of XRP come from the nature of the XRP Ledger and its [consensus process](consensus.html). The XRP Ledger does not require mining and the consensus process does not require multiple confirmations for immutability, which makes the XRP Ledger faster and more efficient at processing transactions than Bitcoin and other top cryptocurrencies.


## XRPの特性

一番最初のレジャーにて1000億XRPが発行され、これ以上新しいXRPは作成できません。 XRPは、[トランザクションコスト](transaction-cost.html)によって消却されるか、またはキーの所有者がいないアドレスに送金すると失われることがあります。 このため、XRPは本質的にはやや[デフレ通貨](https://en.wikipedia.org/wiki/Deflation)です。 現時点の消却のペースでは、すべてのXRPが消却されるまでに約7万年かかります。 またXRPの総供給量の変化に伴い、XRPの[価格と手数料が調整される可能性があります](fee-voting.html)。

技術的には、XRPは0.000001 XRPの単位まで正確に計算され、「Drop」と呼ばれます。 [`rippled`API](http-websocket-apis.html)では、XRPの量は常にXRPのdrop単位で指定する必要があります。 たとえば1 XRPは`1000000` dropと表されます。 詳細については、[通貨フォーマットのリファレンス](currency-formats.html)を参照してください。

## History

### XRP Sales

The XRP Ledger was built over 2011 – early 2012 by Jed McCaleb, Arthur Britto and David Schwartz. In September 2012, Jed and Arthur, along with Chris Larsen formed Ripple (the company, called OpenCoin Inc. at the time) and decided to gift 80 billion XRP to Ripple in exchange for Ripple developing on the XRP Ledger. Since then, the company has regularly sold XRP, used it to strengthen XRP markets and improve network liquidity, and incentivized development of the greater ecosystem. In 2017, the company [placed 55 billion XRP in escrow](https://ripple.com/insights/ripple-escrows-55-billion-xrp-for-supply-predictability/) to ensure that the amount entering the general supply [grows predictably](https://ripple.com/insights/ripple-to-place-55-billion-xrp-in-escrow-to-ensure-certainty-into-total-xrp-supply/) for the foreseeable future. Ripple's [XRP Market Performance site](https://ripple.com/xrp/market-performance/) reports how much XRP the company has available and locked in escrow at present. <!-- SPELLING_IGNORE: mccaleb, britto, opencoin -->

### Naming

Originally, the XRP Ledger was called "Ripple" for the way the technology allowed payments [to ripple through multiple hops and currencies](rippling.html). For the native asset built into the ledger, the creators chose the ticker symbol "XRP" from the term "ripple credits" or "ripples" and the X prefix for non-national currencies in the ISO 4217 standard. The company registered itself as "Ripple Labs". The name "XRP" came to be used to refer to the asset in all contexts, to avoid confusion with the similar names for the technology and company, and eventually the company shortened its own name to "Ripple". In May 2018, [the community selected a new "X" symbol](https://twitter.com/xrpsymbol/status/1006925937571713025) to represent XRP to differentiate it from the triskelion logo that had previously been used for both the company and the digital asset.

| XRPのユースケース、メリット、最新情報についての詳細は、[XRPポータル](https://ripple.com/xrp/)を参照してください。 | Ripple triskelion                        |
|:------------------------------------------------------------------------- |:---------------------------------------- |
| !["X" logo](assets/img/xrp-x-logo.png)                                    | ![Triskelion](img/ripple-triskelion.png) |


The smallest, indivisible unit of XRP was named a "drop" at the suggestion of Ripple forum member ThePiachu. An early alternative term was a "jed", after Jed McCaleb. <!-- SPELLING_IGNORE: thepiachu, mccaleb -->

## See Also

- [Send XRP (Interactive Tutorial)](send-xrp.html)
- [List XRP In Your Exchange](list-xrp-as-an-exchange.html)
- [Currency Formatting in rippled APIs](currency-formats.html#)
