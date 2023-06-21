---
html: cross-currency-payments.html
parent: payment-types.html
blurb: 複数通貨間の支払いでは、パスとオーダーブックを通じて変換するのとは異なる通貨を自動的にに送金します。
labels:
  - Cross-Currency
  - 支払い
---

# Cross-Currency Payments

XRP Ledgerでは、1つ以上の発行済み通貨、XRP、またはその両方を交換して、複数通貨間で支払いを送金できます。 [XRPによる直接支払](use-simple-xrp-payments.html)と同様に、このような支払いでは\[Paymentトランザクションタイプ\]\[Payment\]が使用されます。 XRP Ledgerでの複数通貨間の支払いは完全に非可分です。 つまり、支払いを全額実行するか、またはまったく実行しないかのいずれかになります。

デフォルトでは、複数通貨間の支払いでは宛先に一定額が送金され、支払元が変動コストを負担します。 複数通貨間の支払いが、[Partial Payments](partial-payments.html)で行われ、一定の送金限度内の変動額が宛先に送金される場合もあります。


## 前提条件

- 定義上、複数通貨間支払いには2種類以上の通貨が関係します。 つまり、関係する通貨のうち、少なくとも1種類以上がXRP以外の発行済み通貨である必要があります。
- 送金元と受取人の間に1つ以上の[パス](paths.html)が確立しており、すべてのパスの総流動性が、支払いを促進するのに十分である必要があります。 複数通貨間の支払いの場合、これは一般に通貨取引[オファー](offers.html)を消費することを意味します。


## オートブリッジング

2種類の発行済み通貨を自動的に交換する複数通貨間の支払いでは、XRPの使用により支払いコストを抑えられる場合には自動的にXRPが使用されます。 たとえば、USDからMXNに送金する支払いの場合、USDからXRP、XRPからMXNへの交換にかかるコストが、USDからMXNへの直接交換にかかるコストよりも低い場合には、前者の交換が自動的に実行されます。 Larger trades can use a combination of direct (USD-MXN) and auto-bridged (USD-XRP-MXN) conversions.

詳細は、[オートブリッジング](autobridging.html)を参照してください。


## See Also

- **Concepts:**
    - [Tokens](tokens.html)
    - [Decentralized Exchange](decentralized-exchange.html)
    - [Paths](paths.html)
- **References:**
    - \[Payment transaction type\]\[Payment transaction\]
    - \[path_find method\]\[\]
    - \[ripple_path_find method\]\[\]
    - [Interpreting Metadata of Cross-Currency Payments](look-up-transaction-results.html#token-payments)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
