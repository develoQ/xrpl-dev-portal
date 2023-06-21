---
html: escrowcancel.html
parent: transaction-types.html
blurb: Escrowに留保されているXRPを送金元に返金します。
labels:
  - Escrow
---

# EscrowCancel

[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/app/tx/impl/Escrow.cpp "Source")

_\[Escrow Amendment\]\[\]が必要です。_

Escrowに留保されているXRPを送金元に返金します。

## {{currentpage.name}} JSONの例

```json
{
   "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
   "TransactionType": "EscrowCancel",
   "Owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
   "OfferSequence": 7,
}
```

[Query example transaction. >](websocket-api-tool.html?server=wss%3A%2F%2Fxrplcluster.com%2F&req=%7B%22id%22%3A%22example_EscrowCancel%22%2C%22command%22%3A%22tx%22%2C%22transaction%22%3A%22B24B9D7843F99AED7FB8A3929151D0CCF656459AE40178B77C9D44CED64E839B%22%2C%22binary%22%3Afalse%7D)

{% include '_snippets/tx-fields-intro.ja.md' %}
<!--{# fix md highlighting_ #}-->


| フィールド           | JSONの型 | \[内部の型\]\[\] | 説明                                                            |
|:--------------- |:------ |:------------ |:------------------------------------------------------------- |
| `Owner`         | 文字列    | AccountID    | Address of the source account that funded the escrow payment. |
| `OfferSequence` | 数値     | UInt32       | 取り消すEscrowを作成した\[EscrowCreateトランザクション\]\[\]のトランザクションシーケンス。    |

EscrowCancelトランザクションはどのアカウントからでも送信できます。

* 対応する\[EscrowCreateトランザクション\]\[\]で`CancelAfter`時刻が指定されていない場合、EscrowCancelトランザクションは失敗します。
* 指定されていても、`CancelAfter`時刻が最後に閉鎖されたレジャーの閉鎖時刻よりも後である場合は、EscrowCancelトランザクションが失敗します。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
