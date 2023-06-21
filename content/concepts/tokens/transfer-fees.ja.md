---
html: transfer-fees.html
parent: tokens.html
blurb: 通貨発行者は、自己の発行済み通貨の送金に手数料を課すことができます。
labels:
  - 手数料
  - トークン
---

# 送金手数料

[Token](tokens.html) issuers can charge a _transfer fee_ that applies when users transfer those tokens among themselves. The sender of the transfer is debited an extra percentage based on the transfer fee, while the recipient of the transfer is credited the intended amount. 差額が送金手数料です。

For standard tokens, the tokens paid in the transfer fee are burned, and no longer tracked in the XRP Ledger. If the token is backed by off-ledger assets, this reduces the amount of those assets the issuer has to hold in reserve to meet its obligations in the XRP Ledger. Transfer fees are usually not appropriate for tokens that aren't backed with outside assets.

Non-fungible tokens can also have transfer fees, but they work differently. For details, see [Non-Fungible Tokens](non-fungible-tokens.html).

発行アカウントとの _直接_ の送金と入金には送金手数料は適用されませんが、[運用アドレス][]から別のユーザーへの送金には送金手数料が適用されます。

XRPにはイシュアーがいないため、送金手数料が発生することはありません。

## Example

In this example, ACME Bank issues a EUR stablecoin on the XRP Ledger. たとえばACME BankがACMEイシュアンスの送金手数料を1%に設定するとします。 支払いの受取人が2 EUR.ACMEを受領するには、送金元が2.02 EUR.ACMEを送金する必要があります。 このトランザクションの後、XRP LedgerのACMEの債務残高は0.02€減少します。 つまり、ACMEはそのXRP Ledgerイシュアンスの担保となるアカウントで当該の額を保有する必要がありません。

次の図は、XRP LedgerによるAliceからCharlieへの2 EUR.ACMEの支払い（送金手数料1%）を示します。

Aliceが2,02€を送金し、Charlieが2,00€を受領し、XRP LedgerでのACMEの負債が0,02€減少します

In accounting terms, Alice's, ACME's, and Charlie's balance sheets may have changed like this:

img/e2g-with_transferrate.png



## ペイメントパスでの送金手数料

<!--{# TODO: Update this for OwnerPaysFee amendment when that gets added #}-->

送金手数料は、各送金においてイシュアンスが発行アカウントを通じて当事者間を移動するたびに適用されます。 さらに複雑なトランザクションでは、手数料が複数回適用されます。 送金手数料は、送金の終わりの時点から逆方向に適用されるので、最終的には支払いの送金者がすべての手数料をカバーするのに十分な額を送金する必要があります。 例:

{{ include_svg("img/transfer-fees-in-paths.svg", "Diagram of cross-currency payment with transfer fees") }}

このシナリオでは、ACMEが発行したEURをSalazar（送金元）が保有しており、WayGateが発行した100 USDをRosa（受取人）に送金したいと思っています。 FXMakerはオーダーブックで最も良いレート（1 USD.WayGate = 0.9 EUR.ACME）のオファーを提供する通貨取引業者です。 もし手数料がなければ、Salazarは90 EURを送金すればRosaに100 USDを送金することができます。 しかしながら、ACMEで1%の送金手数料が発生し、WayGateで0.2%の送金手数料が発生します。 つまり、次のようになります。

* Rosaが100 USD.WayGateを受領するには、FXMakerから100.20 USD.WayGateを送金する必要があります。
* 100.20 USD.WayGateを送金する場合のFXMakerの現在の買値は90.18 EUR.ACMEです。
* FXMakerが90.18 EUR.ACMEを受領するには、Salazarが91.0818 EUR.ACMEを送金する必要があります。

<!-- SPELLING_IGNORE: waygate, fxmaker -->

# 技術詳細

送金手数料は[発行アドレス][]の設定により表されます。 送金手数料には、0%未満の値と100%を超える値は指定できず、0.0000001%の位までで切り捨てられます。 TransferRate設定は同一アカウントにより発行されるすべての通貨に適用されます。 通貨によって異なる送金手数料のパーセンテージを適用するには、通貨ごとに異なる[発行アドレス][]を使用します。

`rippled`のJSON-RPC APIおよびWebSocket APIでは、送金手数料は`TransferRate`フィールドに10進数で指定され、この数字は受取人が同一通貨を10億単位受領できるよう送金する必要のある額を表します。 `TransferRate`が`1005000000`の場合、送金手数料0.5%に相当します。 デフォルトでは`TransferRate`は手数料なしに設定されています。 `TransferRate`には、`1000000000`（手数料「0%」）未満の値と`2000000000`（手数料「100%」）を上回る値は指定できません。 値`0`は手数料なしの特殊なケースであり、`1000000000`に相当します。

金融機関は、[発行アドレス][]から\[AccountSetトランザクション\]\[\]を送信して、イシュアンスの`TransferRate`を変更することができます。

アカウントの`TransferRate`を確認するには、\[account_infoメソッド\]\[\]を使用します。 `TransferRate`が省略されている場合は、手数料はありません。

**注記:** `rippled` v0.80.0で導入され2017-11-14に有効となった[fix1201 Amendment](amendments.html)により、最大送金手数料は実効限度である約329%（32ビット整数の最大サイズに基づく）から100%に引き下げられました。 The ledger may still contain accounts with a transfer fee setting higher than 100% because transfer fees that were already set continue to apply at their stated rate.

## Client Library Support

Some [client libraries](client-libraries.html) have convenience functions for getting and setting `TransferRate` functions.

**JavaScript:** Use `xrpl.percentToTransferRate()` to convert a percentage transfer fee from a string to the corresponding `TransferRate` value.

## See Also

- **コンセプト:**
    - [手数料（曖昧さの回避）](fees.html)
    - [トランザクションコスト](transaction-cost.html)
    - [パス](paths.html)
- **チュートリアル:**
    - [XRP Ledgerゲートウェイの開設](become-an-xrp-ledger-gateway.html)
- **リファレンス:**
    - \[account_linesメソッド\]\[\]
    - \[account_infoメソッド\]\[\]
    - \[AccountSetトランザクション\]\[\]
    - [AccountRootのフラグ](accountroot.html#accountroot-flags)


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}

[運用アドレス]: issuing-and-operational-addresses.html
[発行アドレス]: issuing-and-operational-addresses.html
