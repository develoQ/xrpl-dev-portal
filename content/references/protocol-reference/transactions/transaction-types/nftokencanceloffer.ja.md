---
html: nftokencanceloffer.html
parent: transaction-types.html
blurb: NFTokenの売買のための既存のトークンへのオファーをキャンセルする。
labels:
  - NFT, 非代替性トークン
---

# NFTokenCancelOffer
[[Source]](https://github.com/XRPLF/rippled/blob/master/src/ripple/app/tx/impl/NFTokenCancelOffer.cpp "Source")

`NFTokenCancelOffer`トランザクションは、`NFTokenCreateOffer`を使用して作成した既存のトークンへのオファーをキャンセルするために使用できます。

_(\[NonFungibleTokensV1_1 amendment\]\[\]が必要です)_

## {{currentpage.name}} JSONの例

```json
{
    "TransactionType": "NFTokenCancelOffer",
    "Account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    "NFTokenOffers": [
      "9C92E061381C1EF37A8CDE0E8FC35188BFC30B1883825042A64309AC09F4C36D"
    ]
}
```

[Query example transaction. >](websocket-api-tool.html?server=wss%3A%2F%2Fs1.ripple.com%2F&req=%7B%22id%22%3A%22example_NFTokenCancelOffer%22%2C%22command%22%3A%22tx%22%2C%22transaction%22%3A%229FF6366C19F762AE3479DC01390CDE17F1055EFF0C52A28B8ACF0CC11AEF0CC5%22%2C%22binary%22%3Afalse%7D)

## アクセス権

`NFTokenOffer` オブジェクトで表される既存のオファーは、以下の方法でキャンセルすることができます。

* `NFTokenOffer`を最初に作成したアカウント
* `NFTokenOffer`の`Destination`フィールドのアカウント（存在する場合）
* `NFTokenOffer`が有効期限を指定しており、`NFTokenCancelOffer`が含まれる親レジャーのクローズ時刻が有効期限より大きい場合、任意のアカウントに設定することができます

`NFTokenOffers`フィールドのIDの1つ以上が、レジャーに現在存在するオブジェクトを参照していない場合でも、トランザクションは成功します（たとえば、それらのオファーはすでに削除されている可能性があります）。 IDの1つが存在するオブジェクトを指していても、[NFTokenOffer](nftokenoffer.html)オブジェクトでない場合は、エラーでトランザクションが失敗します。

{% include '_snippets/tx-fields-intro.ja.md' %}

| フィールド           | JSONの型 | \[内部の型\]\[\] | 説明                                                                                                                                                                                                                  |
|:--------------- |:------ |:------------ |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NFTokenOffers` | 配列     | VECTOR256    | キャンセルする`NFTokenOffer`オブジェクトのIDの配列(`NFToken`オブジェクトのIDではなく、`NFTokenOffer`オブジェクトのID)です。 各エントリーは[NFTokenOffer](nftokenoffer.html)オブジェクトの異なる[オブジェクトID](ledger-object-ids.html)である必要があり、配列に重複した項目がある場合、トランザクションはエラーとなります。 |

The transaction can succeed even if one or more of the IDs in the `NFTokenOffers` field do not refer to objects that currently exist in the ledger. (For example, those token offers might already have been deleted.) The transaction fails with an error if one of the IDs points to an object that does exist, but is not a [NFTokenOffer](nftokenoffer.html) object.

注意すべき点は、誤って`nft_offer_index`ではなく`nft_id`を指定してしまった場合、`tesSUCCESS`応答を受け取る可能性があることです。 適切にフォーマットされたID値が見つからない場合、システムは`NFTokenOffer`が既に削除されたと判断するからです。

IDが[NFTokenOffer](nftokenoffer.html)オブジェクトでないオブジェクトを指している場合、トランザクションはエラーで失敗します。

## エラーケース

すべてのトランザクションで発生する可能性のあるエラーに加えて、{{currentpage.name}}トランザクションでは、次の[トランザクション結果コード](transaction-results.html)が発生する可能性があります。

| エラーコード             | 説明                                                                                                                                                        |
|:------------------ |:--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `temDISABLED`      | \[NonFungibleTokensV1の修正\]\[\]は有効ではありません。                                                                                                                 |
| `temMALFORMED`     | トランザクションが有効なフォーマットではありませんでした。 たとえば、`NFTokenOffers`配列が空であるか、一度にキャンセルできるオファーの最大数を超える数を含んでいた場合です。                                                            |
| `tecNO_PERMISSION` | `NFTokenOffers`フィールドのIDのうち少なくとも1つが、キャンセルできないオブジェクトを参照しています。 例えば、このトランザクションの送信者がオファーの所有者や`Destination`ではない場合や、オブジェクトが`NFTokenOffer`タイプのオブジェクトではなかった場合などです。 |

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
