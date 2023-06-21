---
html: offercancel.html
parent: transaction-types.html
blurb: Withdraw a currency-exchange order.
labels:
  - Decentralized Exchange
---

# OfferCancel

[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/app/tx/impl/CancelOffer.cpp "Source")

OfferCancelトランザクションは、XRP LedgerからOfferオブジェクトを削除します。

## {{currentpage.name}}のJSONの例

```json
{
   "TransactionType":"OfferCancel",
   "Account":"ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
   "Fee":"12",
   "Flags":0,
   "LastLedgerSequence":7108629,
   "OfferSequence":6,
   "Sequence":7
}
```

[Query example transaction. >](websocket-api-tool.html?server=wss%3A%2F%2Fxrplcluster.com%2F&req=%7B%22id%22%3A%22example_OfferCancel%22%2C%22command%22%3A%22tx%22%2C%22transaction%22%3A%22E7697D162A606FCC138C5732BF0D2A4AED49386DC59235FC3E218650AAC19744%22%2C%22binary%22%3Afalse%7D)

{% include '_snippets/tx-fields-intro.ja.md' %}

| フィールド           | JSONの型 | \[内部の型\]\[\] | 説明                                                                                                                      |
|:--------------- |:------ |:------------ |:----------------------------------------------------------------------------------------------------------------------- |
| `OfferSequence` | 数値     | UInt32       | 前のOfferCreateトランザクションのシーケンス番号。 指定されている場合は、レジャーでそのトランザクションにより作成されたOfferオブジェクトがすべて取り消されます。 指定されたオファーが存在しない場合はエラーと見なされません。 |

*ヒント:* 古いオファーを削除して新しいオファーに置き換えるには、OfferCancelとOfferCreateを使用する代わりに、`OfferSequence`パラメーターを指定した\[OfferCreateトランザクション\]\[\]を使用できます。

OfferCancelメソッドは、一致するシーケンス番号が見つからない場合でも[tesSUCCESS](tes-success.html)を返します。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
