---
html: tx.html
parent: transaction-methods.html
blurb: 1つのトランザクションに関する情報を取得します。
labels:
  - トランザクション送信
  - 支払い
---

# tx
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Tx.cpp "Source")

`tx`メソッドは1つのトランザクションに関する情報を取得します。

## 要求フォーマット

要求フォーマットの例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
 "id": 1,
 "command": "tx",
 "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7",
 "binary": false
}
```
*JSON-RPC*

```json
{
   "method": "tx",
   "params": [
       {
           "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7",
           "binary": false
       }
   ]
}
```
*コマンドライン*

```sh
#Syntax: tx transaction [binary]
rippled tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7 false
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#tx)

要求には以下のパラメーターが含まれます。

| `Field`       | 型      | 説明                                                                                                                                                                                                                                                                                                                                  |
|:------------- |:------ |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `transaction` | 文字列    | トランザクションの256ビットハッシュ（16進数）。                                                                                                                                                                                                                                                                                                          |
| `binary`      | ブール値   | _(Optional)_ If `true`, return transaction data and metadata as binary [serialized](serialization.html) to hexadecimal strings. If `false`, return transaction data and metadata as JSON. The default is `false`.                                                                                                                   |
| `min_ledger`  | Number | _(Optional)_ Use this with `max_ledger` to specify a range of up to 1000 \[ledger indexes\]\[ledger index\], starting with this ledger (inclusive). If the server [cannot find the transaction](#not-found-response), it confirms whether it was able to search all the ledgers in this range. \[New in: rippled 1.5.0\]\[\]        |
| `max_ledger`  | Number | _(Optional)_ Use this with `min_ledger` to specify a range of up to 1000 \[ledger indexes\]\[ledger index\], ending with this ledger (inclusive). If the server [cannot find the transaction](#not-found-response), it confirms whether it was able to search all the ledgers in the requested range. \[New in: rippled 1.5.0\]\[\] |

**Caution:** This command may successfully find the transaction even if it is included in a ledger _outside_ the range of `min_ledger` to `max_ledger`.

## 応答フォーマット

処理が成功した応答の例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
   "id": 1,
   "result": {
       "Account": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
       "Amount": {
           "currency": "USD",
           "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
           "value": "1"
       },
       "Destination": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
       "Fee": "10",
       "Flags": 0,
       "Paths": [
           [
               {
                   "account": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                   "currency": "USD",
                   "issuer": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                   "type": 49,
                   "type_hex": "0000000000000031"
               }
           ],
           [
               {
                   "account": "rD1jovjQeEpvaDwn9wKaYokkXXrqo4D23x",
                   "currency": "USD",
                   "issuer": "rD1jovjQeEpvaDwn9wKaYokkXXrqo4D23x",
                   "type": 49,
                   "type_hex": "0000000000000031"
               },
               {
                   "account": "rB5TihdPbKgMrkFqrqUC3yLdE8hhv4BdeY",
                   "currency": "USD",
                   "issuer": "rB5TihdPbKgMrkFqrqUC3yLdE8hhv4BdeY",
                   "type": 49,
                   "type_hex": "0000000000000031"
               },
               {
                   "account": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                   "currency": "USD",
                   "issuer": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                   "type": 49,
                   "type_hex": "0000000000000031"
               }
           ]
       ],
       "SendMax": {
           "currency": "USD",
           "issuer": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
           "value": "1.01"
       },
       "Sequence": 88,
       "SigningPubKey": "02EAE5DAB54DD8E1C49641D848D5B97D1B29149106174322EDF98A1B2CCE5D7F8E",
       "TransactionType": "Payment",
       "TxnSignature": "30440220791B6A3E036ECEFFE99E8D4957564E8C84D1548C8C3E80A87ED1AA646ECCFB16022037C5CAC97E34E3021EBB426479F2ACF3ACA75DB91DCC48D1BCFB4CF547CFEAA0",
       "hash": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7",
       "inLedger": 348734,
       "ledger_index": 348734,
       "meta": {
           "AffectedNodes": [
               {
                   "ModifiedNode": {
                       "FinalFields": {
                           "Account": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                           "Balance": "59328999119",
                           "Flags": 0,
                           "OwnerCount": 11,
                           "Sequence": 89
                       },
                       "LedgerEntryType": "AccountRoot",
                       "LedgerIndex": "E0D7BDE68B468FF0B8D948FD865576517DA987569833A05374ADB9A72E870A06",
                       "PreviousFields": {
                           "Balance": "59328999129",
                           "Sequence": 88
                       },
                       "PreviousTxnID": "C26AA6B4F7C3B9F55E17CD0D11F12032A1C7AD2757229FFD277C9447A8815E6E",
                       "PreviousTxnLgrSeq": 348700
                   }
               },
               {
                   "ModifiedNode": {
                       "FinalFields": {
                           "Balance": {
                               "currency": "USD",
                               "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                               "value": "-1"
                           },
                           "Flags": 131072,
                           "HighLimit": {
                               "currency": "USD",
                               "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                               "value": "100"
                           },
                           "HighNode": "0000000000000000",
                           "LowLimit": {
                               "currency": "USD",
                               "issuer": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                               "value": "0"
                           },
                           "LowNode": "0000000000000000"
                       },
                       "LedgerEntryType": "RippleState",
                       "LedgerIndex": "EA4BF03B4700123CDFFB6EB09DC1D6E28D5CEB7F680FB00FC24BC1C3BB2DB959",
                       "PreviousFields": {
                           "Balance": {
                               "currency": "USD",
                               "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                               "value": "0"
                           }
                       },
                       "PreviousTxnID": "53354D84BAE8FDFC3F4DA879D984D24B929E7FEB9100D2AD9EFCD2E126BCCDC8",
                       "PreviousTxnLgrSeq": 343570
                   }
               }
           ],
           "TransactionIndex": 0,
           "TransactionResult": "tesSUCCESS"
       },
       "validated": true
   },
   "status": "success",
   "type": "response"
}
```

*JSON-RPC*

```json
{
    "result": {
        "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
        "Fee": "12",
        "Flags": 0,
        "LastLedgerSequence": 56865248,
        "OfferSequence": 5037708,
        "Sequence": 5037710,
        "SigningPubKey": "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
        "TakerGets": "15000000000",
        "TakerPays": {
            "currency": "CNY",
            "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
            "value": "20160.75"
        },
        "TransactionType": "OfferCreate",
        "TxnSignature": "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
        "date": 648248020,
        "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
        "inLedger": 56865245,
        "ledger_index": 56865245,
        "meta": {
            "AffectedNodes": [
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "ExchangeRate": "4F04C66806CF7400",
                            "Flags": 0,
                            "RootIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                            "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                            "TakerPaysCurrency": "000000000000000000000000434E590000000000",
                            "TakerPaysIssuer": "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "Balance": "10404767991",
                            "Flags": 0,
                            "OwnerCount": 3,
                            "Sequence": 5037711
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
                        "PreviousFields": {
                            "Balance": "10404768003",
                            "Sequence": 5037710
                        },
                        "PreviousTxnID": "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
                        "PreviousTxnLgrSeq": 56865244
                    }
                },
                {
                    "DeletedNode": {
                        "FinalFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "BookNode": "0000000000000000",
                            "Flags": 0,
                            "OwnerNode": "0000000000000000",
                            "PreviousTxnID": "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
                            "PreviousTxnLgrSeq": 56865244,
                            "Sequence": 5037708,
                            "TakerGets": "15000000000",
                            "TakerPays": {
                                "currency": "CNY",
                                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                                "value": "20160.75"
                            }
                        },
                        "LedgerEntryType": "Offer",
                        "LedgerIndex": "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Flags": 0,
                            "IndexNext": "0000000000000000",
                            "IndexPrevious": "0000000000000000",
                            "Owner": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "RootIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                    }
                },
                {
                    "CreatedNode": {
                        "LedgerEntryType": "Offer",
                        "LedgerIndex": "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
                        "NewFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "Sequence": 5037710,
                            "TakerGets": "15000000000",
                            "TakerPays": {
                                "currency": "CNY",
                                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                                "value": "20160.75"
                            }
                        }
                    }
                }
            ],
            "TransactionIndex": 0,
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
      "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
      "Fee" : "12",
      "Flags" : 0,
      "LastLedgerSequence" : 56865248,
      "OfferSequence" : 5037708,
      "Sequence" : 5037710,
      "SigningPubKey" : "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
      "TakerGets" : "15000000000",
      "TakerPays" : {
         "currency" : "CNY",
         "issuer" : "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
         "value" : "20160.75"
      },
      "TransactionType" : "OfferCreate",
      "TxnSignature" : "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
      "date" : 648248020,
      "hash" : "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
      "inLedger" : 56865245,
      "ledger_index" : 56865245,
      "meta" : {
         "AffectedNodes" : [
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "ExchangeRate" : "4F04C66806CF7400",
                     "Flags" : 0,
                     "RootIndex" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                     "TakerGetsCurrency" : "0000000000000000000000000000000000000000",
                     "TakerGetsIssuer" : "0000000000000000000000000000000000000000",
                     "TakerPaysCurrency" : "000000000000000000000000434E590000000000",
                     "TakerPaysIssuer" : "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "Balance" : "10404767991",
                     "Flags" : 0,
                     "OwnerCount" : 3,
                     "Sequence" : 5037711
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
                  "PreviousFields" : {
                     "Balance" : "10404768003",
                     "Sequence" : 5037710
                  },
                  "PreviousTxnID" : "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
                  "PreviousTxnLgrSeq" : 56865244
               }
            },
            {
               "DeletedNode" : {
                  "FinalFields" : {
                     "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "BookDirectory" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                     "BookNode" : "0000000000000000",
                     "Flags" : 0,
                     "OwnerNode" : "0000000000000000",
                     "PreviousTxnID" : "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
                     "PreviousTxnLgrSeq" : 56865244,
                     "Sequence" : 5037708,
                     "TakerGets" : "15000000000",
                     "TakerPays" : {
                        "currency" : "CNY",
                        "issuer" : "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                        "value" : "20160.75"
                     }
                  },
                  "LedgerEntryType" : "Offer",
                  "LedgerIndex" : "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Flags" : 0,
                     "IndexNext" : "0000000000000000",
                     "IndexPrevious" : "0000000000000000",
                     "Owner" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "RootIndex" : "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
               }
            },
            {
               "CreatedNode" : {
                  "LedgerEntryType" : "Offer",
                  "LedgerIndex" : "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
                  "NewFields" : {
                     "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "BookDirectory" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                     "Sequence" : 5037710,
                     "TakerGets" : "15000000000",
                     "TakerPays" : {
                        "currency" : "CNY",
                        "issuer" : "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                        "value" : "20160.75"
                     }
                  }
               }
            }
         ],
         "TransactionIndex" : 0,
         "TransactionResult" : "tesSUCCESS"
      },
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

この応答は\[標準フォーマット\]\[\]に従っており、正常に完了した場合は結果に[Transactionオブジェクト](transaction-formats.html)フィールドと以下の追加のフィールドが含まれています。

| `Field`        | 型                                | 説明                                                                                                                                                                                                                                                                                                                                 |
|:-------------- |:-------------------------------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `date`         | Number                           | A [number of seconds](basic-data-types.html#specifying-time) since January 1, 2000 (00:00 UTC) indicating the [close time](ledgers.html#ledger-close-times) of the ledger in which the transaction was applied. This value does not have a precise relationship with physical time, and is dependent on the close time resolution. |
| `hash`         | 文字列                              | トランザクションのSHA-512ハッシュ                                                                                                                                                                                                                                                                                                               |
| `inLedger`     | Number                           | （廃止予定）`ledger_index`のエイリアス。                                                                                                                                                                                                                                                                                                        |
| `ledger_index` | Number                           | The \[ledger index\]\[\] of the ledger that includes this transaction.                                                                                                                                                                                                                                                             |
| `meta`         | Object (JSON) or String (binary) | [Transaction metadata](transaction-metadata.html), which describes the results of the transaction.                                                                                                                                                                                                                                 |
| `validated`    | オブジェクト                           | 省略されている場合またはfalseに設定されている場合は、このデータは最終データではありません。                                                                                                                                                                                                                                                                                   |
| （各種）           | （各種）                             | [Transactionオブジェクト](transaction-formats.html)のその他のフィールド                                                                                                                                                                                                                                                                            |

**Note:** `rippled` 1.7.0 has a known issue where the `meta` field contains JSON even if the request asked for binary. ([#3791](https://github.com/ripple/rippled/pull/3791))

### Not Found Response

（省略可、デフォルトではfalseです）trueの場合、トランザクションデータとメタデータがJSONではなく16進文字列として返されます。

- The transaction has not been included in any ledger version, and has not been executed.
- The transaction was included in a ledger version that the server does not have available.

This means that a `txnNotFound` on its own is not enough to know the [final outcome of a transaction](finality-of-results.html).

To further narrow down the possibilities, you can provide a range of ledgers to search using the `min_ledger` and `max_ledger` fields in the request. If you provide **both** of those fields, the `txnNotFound` response includes the following field:

| `Field`        | Type | Description                                                                                                                                                                                                                                                                                                                                                                     |
|:-------------- |:---- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `searched_all` | ブール値 | _(Omitted unless the request provided `min_ledger` and `max_ledger`)_ If `true`, the server was able to search all of the specified ledger versions, and the transaction was in none of them. If `false`, the server did not have all of the specified ledger versions available, so it is not sure if one of them might contain the transaction. \[New in: rippled 1.5.0\]\[\] |

An example of a `txnNotFound` response that fully searched a requested range of ledgers:

<!-- MULTICODE_BLOCK_START -->

_WebSocket_

```json
{
  "error": "txnNotFound",
  "error_code": 29,
  "error_message": "Transaction not found.",
  "id": 1,
  "request": {
    "binary": false,
    "command": "tx",
    "id": 1,
    "max_ledger": 54368673,
    "min_ledger": 54368573,
    "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7"
  },
  "searched_all": true,
  "status": "error",
  "type": "response"
}
```

_JSON-RPC_

```json
200 OK

{
  "result": {
    "error": "txnNotFound",
    "error_code": 29,
    "error_message": "Transaction not found.",
    "request": {
      "binary": false,
      "command": "tx",
      "max_ledger": 54368673,
      "min_ledger": 54368573,
      "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7"
    },
    "searched_all": true,
    "status": "error"
  }
}
```

<!-- MULTICODE_BLOCK_END -->

## 考えられるエラー

* \[汎用エラータイプ\]\[\]のすべて。
* `invalidParams` - 1つ以上のフィールドの指定が正しくないか、1つ以上の必須フィールドが指定されていません。
* `txnNotFound` - トランザクションが存在しないか、または`rippled`で使用できない古いレジャーバージョンのトランザクションです。
* `excessiveLgrRange` - The `min_ledger` and `max_ledger` fields of the request are more than 1000 apart.
* `invalidLgrRange` - The specified `min_ledger` is larger than the `max_ledger`, or one of those parameters is not a valid ledger index.


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
