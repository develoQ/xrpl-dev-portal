---
html: book_offers.html
parent: path-and-order-book-methods.html
blurb: Get info about offers to exchange two currencies.
labels:
  - Decentralized Exchange
  - Cross-Currency
---

# book_offers
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/BookOffers.cpp "Source")

`book_offers`メソッドは、[オーダーブック](http://www.investopedia.com/terms/o/order-book.asp)と呼ばれる、2つの通貨間のオファーのリストを取得します。 The response omits [unfunded Offers](offers.html#lifecycle-of-an-offer) and reports how much of each remaining Offer's total is currently funded.

## 要求フォーマット
要求フォーマットの例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 4,
  "command": "book_offers",
  "taker": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "taker_gets": {
    "currency": "XRP"
  },
  "taker_pays": {
    "currency": "USD",
    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
  },
  "limit": 10
}
```

*JSON-RPC*

```json
{
    "method": "book_offers",
    "params": [
        {
            "taker": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "taker_gets": {
                "currency": "XRP"
            },
            "taker_pays": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            },
            "limit": 10
        }
    ]
}
```

*コマンドライン*

```sh
#Syntax: book_offers taker_pays taker_gets [taker [ledger [limit] ] ]
rippled book_offers 'USD/rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B' 'EUR/rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#book_offers)

要求には以下のパラメーターが含まれます。

| `Field`        | 型            | Required? | 説明                                                                                                                                                                                                                  |
|:-------------- |:------------ |:--------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `taker_gets`   | オブジェクト       | Yes       | The asset the account taking the Offer would receive, as a [currency without an amount](currency-formats.html#specifying-without-amounts).                                                                          |
| `taker_pays`   | オブジェクト       | Yes       | The asset the account taking the Offer would pay, as a [currency without an amount](currency-formats.html#specifying-without-amounts).                                                                              |
| `ledger_hash`  | \[Hash\]\[\] | No        | _（省略可）_ 使用するレジャーバージョンの20バイトの16進文字列。 （\[レジャーの指定\]\[\]を参照してください）                                                                                                                                                      |
| `ledger_index` | \[マーカー\]\[\] | No        | _（省略可）_ 使用するレジャーの\[レジャーインデックス\]\[\]、またはレジャーを自動的に選択するためのショートカット文字列。 （\[レジャーの指定\]\[\]を参照してください）                                                                                                                       |
| `limit`        | Number       | No        | The maximum number of Offers to return. The response may include fewer results.                                                                                                                                     |
| `taker`        | 文字列          | No        | The \[Address\]\[\] of an account to use as a perspective. The response includes this account's Offers even if they are unfunded. (You can use this to see what Offers are above or below yours in the order book.) |


## 応答フォーマット

処理が成功した応答の例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 11,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 7035305,
    "offers": [
      {
        "Account": "rM3X3QSr8icjTGpaF52dozhbT2BZSXJQYM",
        "BookDirectory": "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D55055E4C405218EB",
        "BookNode": "0000000000000000",
        "Flags": 0,
        "LedgerEntryType": "Offer",
        "OwnerNode": "0000000000000AE0",
        "PreviousTxnID": "6956221794397C25A53647182E5C78A439766D600724074C99D78982E37599F1",
        "PreviousTxnLgrSeq": 7022646,
        "Sequence": 264542,
        "TakerGets": {
          "currency": "EUR",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "17.90363633316433"
        },
        "TakerPays": {
          "currency": "USD",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "27.05340557506234"
        },
        "index": "96A9104BF3137131FF8310B9174F3B37170E2144C813CA2A1695DF2C5677E811",
        "quality": "1.511056473200875"
      },
      {
        "Account": "rhsxKNyN99q6vyYCTHNTC1TqWCeHr7PNgp",
        "BookDirectory": "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D5505DCAA8FE12000",
        "BookNode": "0000000000000000",
        "Flags": 131072,
        "LedgerEntryType": "Offer",
        "OwnerNode": "0000000000000001",
        "PreviousTxnID": "8AD748CD489F7FF34FCD4FB73F77F1901E27A6EFA52CCBB0CCDAAB934E5E754D",
        "PreviousTxnLgrSeq": 7007546,
        "Sequence": 265,
        "TakerGets": {
          "currency": "EUR",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "2.542743233917848"
        },
        "TakerPays": {
          "currency": "USD",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "4.19552633596446"
        },
        "index": "7001797678E886E22D6DE11AF90DF1E08F4ADC21D763FAFB36AF66894D695235",
        "quality": "1.65"
      }
    ]
  }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "ledger_current_index": 8696243,
        "offers": [],
        "status": "success",
        "validated": false
    }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_current_index" : 56867201,
      "offers" : [
         {
            "Account" : "rnixnrMHHvR7ejMpJMRCWkaNrq3qREwMDu",
            "BookDirectory" : "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D56038D7EA4C68000",
            "BookNode" : "0000000000000000",
            "Flags" : 131072,
            "LedgerEntryType" : "Offer",
            "OwnerNode" : "0000000000000000",
            "PreviousTxnID" : "E43ADD1BD4AC2049E0D9DE6BC279B7FD95A99C8DE2C4694A4A7623F6D9AAAE29",
            "PreviousTxnLgrSeq" : 47926685,
            "Sequence" : 219,
            "TakerGets" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "2.459108753792364"
            },
            "TakerPays" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "24.59108753792364"
            },
            "index" : "3087B4828C6B5D8595EA325D69C0F396C57452893647799493A38F2C164990AB",
            "owner_funds" : "2.872409153061363",
            "quality" : "10"
         },
         {
            "Account" : "rKwjWCKBaASEvtHCxtvReNd2i9n8DxSihk",
            "BookDirectory" : "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D56038D7EA4C68000",
            "BookNode" : "0000000000000000",
            "Flags" : 131072,
            "LedgerEntryType" : "Offer",
            "OwnerNode" : "0000000000000000",
            "PreviousTxnID" : "B63B2ECD124FE6B02BC2998929517266BD221A02FEE51DDE4992C1BCB7E86CD3",
            "PreviousTxnLgrSeq" : 43166305,
            "Sequence" : 19,
            "TakerGets" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "3.52"
            },
            "TakerPays" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "35.2"
            },
            "index" : "89865F2C70D1140796D9D249AC2ED765AE2D007A52DEC6D6D64CCB1A77A6EB7F",
            "owner_funds" : "3.523192614770459",
            "quality" : "10",
            "taker_gets_funded" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "3.516160294182094"
            },
            "taker_pays_funded" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "35.16160294182094"
            }
         }
      ],
      "status" : "success",
      "validated" : false
   }
}
```

<!-- MULTICODE_BLOCK_END -->

この応答は\[標準フォーマット\]\[\]に従っており、正常に完了した場合は結果に次のフィールドが含まれます。

| `Field`                | 型                       | 説明                                                                                        |
|:---------------------- |:----------------------- |:----------------------------------------------------------------------------------------- |
| `ledger_current_index` | 数値 - \[レジャーインデックス\]\[\] | _（`ledger_current_index`が指定されている場合は省略）_ この情報の取得時に使用した、現在処理中のレジャーバージョンの\[レジャーインデックス\]\[\]。 |
| `ledger_index`         | 数値 - \[レジャーインデックス\]\[\] | _（`ledger_current_index`が指定されている場合は省略可）_ 要求に従って、このデータの取得時に使用されたレジャーバージョンのレジャーインデックス。      |
| `ledger_hash`          | \[Hash\]\[\]            | _（省略される場合があります）_ 要求に従って、このデータの取得時に使用されたレジャーバージョンの識別用ハッシュ。                                 |
| `offers`               | 配列                      | Offerオブジェクトの配列。 各オブジェクトには[Offer オブジェクト](offer.html)のフィールドが含まれています。                        |

`offers`配列の要素には、Offerの標準フィールドの他に以下のフィールドが含まれます。

| `Field`             | 型                  | 説明                                                                                                                                                                                                  |
|:------------------- |:------------------ |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `owner_funds`       | 文字列                | Amount of the `TakerGets` currency the side placing the offer has available to be traded. （XRPはdrop単位で表されます。 その他のすべての通貨は10進数値として表されます。 ）1人のトレーダーの複数のオファーが同一のブックに含まれている場合、このフィールドは最高順位のオファーにのみ含まれます。 |
| `taker_gets_funded` | 文字列 - \[ハッシュ\]\[\] | （部分的に資金供給されているオファーのみに含まれます）オファーの資金供給ステータスが指定されている場合に、受取人が受領できる最大通貨額。                                                                                                                                |
| `taker_pays_funded` | \[マーカー\]\[\]       | （部分的に資金供給されているオファーのみに含まれます）オファーの資金供給ステータスが指定されている場合に、受取人が支払う最大通貨額。                                                                                                                                  |
| `quality`           | 文字列                | 為替レート（`taker_pays`を`taker_gets`で割った比率）。 公正を期すため、同じクオリティのオファーは先入れ先出しで自動的に受諾されます。 （つまり、複数の人々が通貨を同じレートで取引するオファーを出した場合、最も古いオファーが最初に受諾されます。 ）                                                            |

## 考えられるエラー

* いずれかの\[汎用エラータイプ\]\[\]。
* `invalidParams` - 1つ以上のフィールドの指定が正しくないか、1つ以上の必須フィールドが指定されていません。
* `lgrNotFound` - `ledger_hash`または`ledger_index`で指定したレジャーが存在しないか、存在してはいるもののサーバーが保有していません。
* `srcCurMalformed` - 要求の`taker_pays`フィールドのフォーマットが適切ではありません。
* `dstAmtMalformed` - 要求の`taker_gets`フィールドのフォーマットが適切ではありません。
* `srcIsrMalformed` - 要求の`taker_pays`フィールドの`issuer`フィールドが無効です。
* `dstIsrMalformed` - 要求の`taker_gets`フィールドの`issuer`フィールドが無効です。
* `badMarket` - 必要なオーダーブックが存在していません（ある通貨をその通貨自体と交換するオファーなど）。


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
