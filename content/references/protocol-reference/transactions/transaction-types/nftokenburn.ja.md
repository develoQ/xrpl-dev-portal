---
html: nftokenburn.html
parent: transaction-types.html
blurb: TokenBurnを使用して、NFTを永久に破棄します。
labels:
  - 非代替性トークン, NFT
---

# NFTokenBurn
[[Source]](https://github.com/XRPLF/rippled/blob/master/src/ripple/app/tx/impl/NFTokenBurn.cpp "Source")

NFTokenBurnトランザクションは、`NFToken`オブジェクトを保持している`NFTokenPage`内から削除し、トークンをレジャーから削除（ _バーン_ ）することになります。

このトランザクションの送信者は、`NFToken`の所有者でなければなりません。 `NFToken`が`lsfBurnable`フラグを有効にしている場合、代わりに発行者または発行者の`NFTokenMinter`によって許可されているアカウントの場合があります。

この操作に成功すると、対応する`NFToken`が削除されます。 この操作によって`NFToken`を保持している`NFTokenPage`が空になるか、統合されて`NFTokenPage`が削除されると、所有者準備金が1つ減ります。

_(\[NonFungibleTokensV1_1 amendment\]\[\]が必要です)_


## {{currentpage.name}} JSONの例

```json
{
  "TransactionType": "NFTokenBurn",
  "Account": "rNCFjv8Ek5oDrNiMJ3pw6eLLFtMjZLJnf2",
  "Owner": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
  "Fee": "10",
  "NFTokenID": "000B013A95F14B0044F78A264E41713C64B5F89242540EE208C3098E00000D65"
}
```

[Query example transaction. >](websocket-api-tool.html?server=wss%3A%2F%2Fs1.ripple.com%2F&req=%7B%22id%22%3A%22example_NFTokenBurn%22%2C%22command%22%3A%22tx%22%2C%22transaction%22%3A%227B9EFDFDC801C58F2B61B89AA2751634F49CE2A93923671FF0F4F099C7EE17FF%22%2C%22binary%22%3Afalse%7D)

{% include '_snippets/tx-fields-intro.ja.md' %}

| フィールド       | JSONの型 | \[内部の型\]\[\] | 説明                                                                                                                                           |
|:----------- |:------ |:------------ |:-------------------------------------------------------------------------------------------------------------------------------------------- |
| `NFTokenID` | 文字列    | Hash256      | このトランザクションによって削除される`NFToken`を指定します。                                                                                                          |
| `Owner`     | 文字列    | AccountID    | _(省略可)_ Burnする`NFToken`の所有者。 所有者がこのトランザクションを送信するアカウントと異なる場合にのみ使用されます。 発行者または許可された発行者は、`lsfBurnable`フラグが有効なNFTをBurnするために、このフィールドを使用することができます。 |


## エラーケース

すべてのトランザクションで発生する可能性のあるエラーに加えて、{{currentpage.name}}トランザクションでは、次の[トランザクション結果コード](transaction-results.html)が発生する可能性があります。

| エラーコード             | 説明                                        |
|:------------------ |:----------------------------------------- |
| `temDISABLED`      | \[NonFungibleTokensV1の修正\]\[\]は有効ではありません。 |
| `tecNO_ENTRY`      | 指定された `TokenID` が見つかりませんでした。              |
| `tecNO_PERMISSION` | このアカウントにはトークンをBurnする権限がありません。             |

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
