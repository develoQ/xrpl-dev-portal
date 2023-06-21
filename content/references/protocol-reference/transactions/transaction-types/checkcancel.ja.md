---
html: checkcancel.html
parent: transaction-types.html
blurb: Cancel a check.
labels:
  - Checks
---

# CheckCancel
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/app/tx/impl/CancelCheck.cpp "Source")

_（\[Checks Amendment\]\[\]が必要です）_

未清算のCheckを取り消し、送金を行わずにレジャーから削除します。 Checkの送金元または送金先は、いつでもこのトランザクションタイプを使用してCheckを取り消すことができます。 有効期限切れのCheckはすべてのアドレスが取り消すことができます。

## {{currentpage.name}} JSONの例

```json
{
   "Account": "rUn84CUYbNjRoTQ6mSW7BVJPSVJNLb1QLo",
   "TransactionType": "CheckCancel",
   "CheckID": "49647F0D748DC3FE26BDACBC57F251AADEFFF391403EC9BF87C97F67E9977FB0",
   "Fee": "12"
}
```

[Query example transaction. >](websocket-api-tool.html?server=wss%3A%2F%2Fs1.ripple.com%2F&req=%7B%22id%22%3A%22example_CheckCancel%22%2C%22command%22%3A%22tx%22%2C%22transaction%22%3A%22D3328000315C6DCEC1426E4E549288E3672752385D86A40D56856DBD10382953%22%2C%22binary%22%3Afalse%7D)

{% include '_snippets/tx-fields-intro.ja.md' %}
<!--{# fix md highlighting_ #}-->

| フィールド     | JSONの型 | \[内部の型\]\[\] | 説明                                                 |
|:--------- |:------ |:------------ |:-------------------------------------------------- |
| `CheckID` | 文字列    | Hash256      | 取り消す[Checkレジャーオブジェクト](check.html)のID（64文字の16進文字列）。 |

## エラーケース

- `CheckID`により識別されるオブジェクトが存在していないか、またはCheckではない場合、トランザクションは結果コード`tecNO_ENTRY`で失敗します。
- Checkが有効期限切れではなく、CheckCancelトランザクションの送信者がCheckの送金元または送金先ではない場合、トランザクションは結果コード`tecNO_PERMISSION`で失敗します。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
