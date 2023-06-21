---
html: account_tx.html
parent: account-methods.html
blurb: 指定したアカウントに関連するトランザクションのリストを取得します。
labels:
  - Payments
  - Accounts
---

# account_tx
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountTx.cpp "Source")

`account_tx`メソッドは、指定したアカウントに関連するトランザクションのリストを取得します。

## 要求フォーマット

要求フォーマットの例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 2,
  "command": "account_tx",
  "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "ledger_index_min": -1,
  "ledger_index_max": -1,
  "binary": false,
  "limit": 2,
  "forward": false
}
```

*JSON-RPC*

```json
{
    "method": "account_tx",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "binary": false,
            "forward": false,
            "ledger_index_max": -1,
            "ledger_index_min": -1,
            "limit": 2
        }
    ]
}
```

*コマンドライン*

```sh
#Syntax account_tx account ledger_index_min ledger_index_max [offset] [limit] [binary] [count] [forward]
rippled -- account_tx r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59 -1 -1 2 5 1 0 1
rippled -- account_tx rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w -1 -1 2 0 binary descending
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_tx)

要求には以下のパラメーターが含まれます。

| `Field`            | 型            | 説明                                                                                                                                                                          |
|:------------------ |:------------ |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`          | 文字列          | アカウントの一意のIDであり、最も一般的にはアカウントのアドレスが使用されます。                                                                                                                                    |
| `ledger_index_min` | 整数           | _（省略可能）_ 含めるトランザクションのレジャーのうち最古のものを指定するのに使用します。 `-1`の値は、使用可能な検証済みレジャーのうち最古のバージョンを使用するよう、サーバーに指示します。                                                                          |
| `ledger_index_max` | 整数           | _（省略可能）_ 含めるトランザクションのレジャーのうち最新のものを指定するのに使用します。 `-1`の値は、使用可能な検証済みレジャーのうち最新のバージョンを使用するよう、サーバーに指示します。                                                                          |
| `ledger_hash`      | 文字列          | _（省略可能）_ 単一のレジャーからのみトランザクションを検索するのに使用します。 （\[レジャーの指定\]\[\]を参照してください）                                                                                                        |
| `ledger_index`     | 文字列または符号なし整数 | _（省略可能）_ 単一のレジャーからのみトランザクションを検索するのに使用します。 （\[レジャーの指定\]\[\]を参照してください）                                                                                                        |
| `binary`           | ブール値         | _（省略可能）_ デフォルトは`false`です。 `true`に設定すると、JSONの代わりに16進文字列でトランザクションが返されます。                                                                                                      |
| `forward`          | ブール値         | _（省略可能）_ デフォルトは`false`です。 `true`に設定すると、最も古いレジャーを先頭としてインデックスが付けられた値が返されます。 そうしない場合、最新のレジャーを先頭として結果にインデックスが付けられます。 （結果を示した各ページの中身は順序よく整理されていない場合がありますが、ページ全体としては順序付けされています。 ） |
| `limit`            | 整数           | _（省略可能）_ デフォルトは変化します。 取得するトランザクションの数を制限します。 サーバーはこの値を受け入れる必要はありません。                                                                                                         |
| `marker`           | \[マーカー\]\[\] | 以前にページネーションされた応答の値。 その応答を停止した箇所からデータの取得を再開します。 サーバーが使用できるレジャーの範囲に変更があっても、この値は変わりません。                                                                                        |

次の各フィールドは省略可能とされていますが、要求内で**1つ以上は使用する必要があります**: `ledger_index`、`ledger_hash`、`ledger_index_min`、または`ledger_index_max`。

次のフィールドは廃止されました： `offset`、`count`、`descending`、`ledger_max`、`ledger_min`。 \[削除: rippled 1.7.0\]\[\]


### 照会されたデータの繰り返し

他のページネーションメソッドと同様に、`marker`フィールドを使用すると複数ページのデータが返されます。

複数の要求の合間に、`"ledger_index_min": -1`と`"ledger_index_max": -1`が変更されて、以前とは異なるレジャーバージョンを参照する場合があります。 `marker`フィールドを使用すると、要求からのレジャー範囲に変更がある場合でも、マーカーが示すポイントが要求で指定されたレジャーの範囲外でない限り、安全にページネーションできます。

## 応答フォーマット

処理が成功した応答の例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 2,
    "status": "success",
    "type": "response",
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "ledger_index_max": 6542489,
        "ledger_index_min": 32570,
        "limit": 2,
        "transactions": [
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                    "Balance": "9999999980",
                                    "Flags": 0,
                                    "OwnerCount": 2,
                                    "Sequence": 3
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
                                "PreviousFields": {
                                    "Balance": "9999999990",
                                    "OwnerCount": 1,
                                    "Sequence": 2
                                },
                                "PreviousTxnID": "389720F6FD8A144F171708F9ECB334D704CBCFEFBCDA152D931AC34FB5F9E32B",
                                "PreviousTxnLgrSeq": 95405
                            }
                        },
                        {
                            "CreatedNode": {
                                "LedgerEntryType": "RippleState",
                                "LedgerIndex": "718C6D58DD3BBAAAEBFE48B8FBE3C32C9F6F2EBC395233BA95D0057078EE07DB",
                                "NewFields": {
                                    "Balance": {
                                        "currency": "USD",
                                        "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                                        "value": "0"
                                    },
                                    "Flags": 131072,
                                    "HighLimit": {
                                        "currency": "USD",
                                        "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                        "value": "100"
                                    },
                                    "LowLimit": {
                                        "currency": "USD",
                                        "issuer": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                                        "value": "0"
                                    }
                                }
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Flags": 0,
                                    "Owner": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                                    "RootIndex": "77F65EFF930ED7E93C6CC839C421E394D6B1B6A47CEA8A140D63EC9C712F46F5"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "77F65EFF930ED7E93C6CC839C421E394D6B1B6A47CEA8A140D63EC9C712F46F5"
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                                    "Balance": "78991384535796",
                                    "Flags": 0,
                                    "OwnerCount": 3,
                                    "Sequence": 188
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "B33FDD5CF3445E1A7F2BE9B06336BEBD73A5E3EE885D3EF93F7E3E2992E46F1A",
                                "PreviousTxnID": "E9E1988A0F061679E5D14DE77DB0163CE0BBDC00F29E396FFD1DA0366E7D8904",
                                "PreviousTxnLgrSeq": 195455
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "ExchangeRate": "4E11C37937E08000",
                                    "Flags": 0,
                                    "RootIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93",
                                    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                                    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                                    "TakerPaysCurrency": "0000000000000000000000004254430000000000",
                                    "TakerPaysIssuer": "5E7B112523F68D2F5E879DB4EAC51C6698A69304"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93"
                            }
                        }
                    ],
                    "TransactionIndex": 0,
                    "TransactionResult": "tesSUCCESS"
                },
                "tx": {
                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "Fee": "10",
                    "Flags": 0,
                    "LimitAmount": {
                        "currency": "USD",
                        "issuer": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                        "value": "100"
                    },
                    "Sequence": 2,
                    "SigningPubKey": "02BC8C02199949B15C005B997E7C8594574E9B02BA2D0628902E0532989976CF9D",
                    "TransactionType": "TrustSet",
                    "TxnSignature": "304402200EF81EC32E0DFA9BE376B20AFCA11765ED9FEA04CA8B77C7178DAA699F7F5AFF02202DA484DBD66521AC317D84F7717EC4614E2F5DB743E313E8B48440499CC0DBA4",
                    "date": 413620090,
                    "hash": "002AA492496A1543DBD3680BF8CF21B6D6A078CE4A01D2C1A4B63778033792CE",
                    "inLedger": 195480,
                    "ledger_index": 195480
                },
                "validated": true
            },
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                    "Balance": "9999999970",
                                    "Flags": 0,
                                    "OwnerCount": 3,
                                    "Sequence": 4
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
                                "PreviousFields": {
                                    "Balance": "9999999980",
                                    "OwnerCount": 2,
                                    "Sequence": 3
                                },
                                "PreviousTxnID": "002AA492496A1543DBD3680BF8CF21B6D6A078CE4A01D2C1A4B63778033792CE",
                                "PreviousTxnLgrSeq": 195480
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Flags": 0,
                                    "Owner": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                                    "RootIndex": "A39F044D860C5B5846AA7E0FAAD44DC8897F0A62B2F628AA073B21B3EC146010"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "A39F044D860C5B5846AA7E0FAAD44DC8897F0A62B2F628AA073B21B3EC146010"
                            }
                        },
                        {
                            "ModifiedNode": {
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "E0D7BDE68B468FF0B8D948FD865576517DA987569833A05374ADB9A72E870A06",
                                "PreviousTxnID": "0222B59280D165D40C464EA75AAD08A4D152C46A38C0625DEECF6EE87FC5B9E1",
                                "PreviousTxnLgrSeq": 343555
                            }
                        },
                        {
                            "CreatedNode": {
                                "LedgerEntryType": "RippleState",
                                "LedgerIndex": "EA4BF03B4700123CDFFB6EB09DC1D6E28D5CEB7F680FB00FC24BC1C3BB2DB959",
                                "NewFields": {
                                    "Balance": {
                                        "currency": "USD",
                                        "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                                        "value": "0"
                                    },
                                    "Flags": 131072,
                                    "HighLimit": {
                                        "currency": "USD",
                                        "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                        "value": "100"
                                    },
                                    "LowLimit": {
                                        "currency": "USD",
                                        "issuer": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                                        "value": "0"
                                    }
                                }
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "ExchangeRate": "4E11C37937E08000",
                                    "Flags": 0,
                                    "RootIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93",
                                    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                                    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                                    "TakerPaysCurrency": "0000000000000000000000004254430000000000",
                                    "TakerPaysIssuer": "5E7B112523F68D2F5E879DB4EAC51C6698A69304"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93"
                            }
                        }
                    ],
                    "TransactionIndex": 0,
                    "TransactionResult": "tesSUCCESS"
                },
                "tx": {
                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "Fee": "10",
                    "Flags": 0,
                    "LimitAmount": {
                        "currency": "USD",
                        "issuer": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                        "value": "100"
                    },
                    "Sequence": 3,
                    "SigningPubKey": "02BC8C02199949B15C005B997E7C8594574E9B02BA2D0628902E0532989976CF9D",
                    "TransactionType": "TrustSet",
                    "TxnSignature": "3044022058A89552068D1A274EE72BA71363E33E54E6608BC28A84DEC6EE530FC2B5C979022029F4D1EA1237A1F717C5F5EC526E6CFB6DF54C30BADD25EDDE7D2FDBC8F17E34",
                    "date": 416347560,
                    "hash": "53354D84BAE8FDFC3F4DA879D984D24B929E7FEB9100D2AD9EFCD2E126BCCDC8",
                    "inLedger": 343570,
                    "ledger_index": 343570
                },
                "validated": true
            }
        ],
        "validated": true
    }
}
```

*JSON-RPC*

```json
200 OK
{
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "ledger_index_max": 8696227,
        "ledger_index_min": 32570,
        "limit": 2,
        "status": "success",
        "transactions": [
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                    "Balance": "9999999980",
                                    "Flags": 0,
                                    "OwnerCount": 2,
                                    "Sequence": 3
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
                                "PreviousFields": {
                                    "Balance": "9999999990",
                                    "OwnerCount": 1,
                                    "Sequence": 2
                                },
                                "PreviousTxnID": "389720F6FD8A144F171708F9ECB334D704CBCFEFBCDA152D931AC34FB5F9E32B",
                                "PreviousTxnLgrSeq": 95405
                            }
                        },
                        {
                            "CreatedNode": {
                                "LedgerEntryType": "RippleState",
                                "LedgerIndex": "718C6D58DD3BBAAAEBFE48B8FBE3C32C9F6F2EBC395233BA95D0057078EE07DB",
                                "NewFields": {
                                    "Balance": {
                                        "currency": "USD",
                                        "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                                        "value": "0"
                                    },
                                    "Flags": 131072,
                                    "HighLimit": {
                                        "currency": "USD",
                                        "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                        "value": "100"
                                    },
                                    "LowLimit": {
                                        "currency": "USD",
                                        "issuer": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                                        "value": "0"
                                    }
                                }
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Flags": 0,
                                    "Owner": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                                    "RootIndex": "77F65EFF930ED7E93C6CC839C421E394D6B1B6A47CEA8A140D63EC9C712F46F5"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "77F65EFF930ED7E93C6CC839C421E394D6B1B6A47CEA8A140D63EC9C712F46F5"
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                                    "Balance": "78991384535796",
                                    "Flags": 0,
                                    "OwnerCount": 3,
                                    "Sequence": 188
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "B33FDD5CF3445E1A7F2BE9B06336BEBD73A5E3EE885D3EF93F7E3E2992E46F1A",
                                "PreviousTxnID": "E9E1988A0F061679E5D14DE77DB0163CE0BBDC00F29E396FFD1DA0366E7D8904",
                                "PreviousTxnLgrSeq": 195455
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "ExchangeRate": "4E11C37937E08000",
                                    "Flags": 0,
                                    "RootIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93",
                                    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                                    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                                    "TakerPaysCurrency": "0000000000000000000000004254430000000000",
                                    "TakerPaysIssuer": "5E7B112523F68D2F5E879DB4EAC51C6698A69304"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93"
                            }
                        }
                    ],
                    "TransactionIndex": 0,
                    "TransactionResult": "tesSUCCESS"
                },
                "tx": {
                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "Fee": "10",
                    "Flags": 0,
                    "LimitAmount": {
                        "currency": "USD",
                        "issuer": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
                        "value": "100"
                    },
                    "Sequence": 2,
                    "SigningPubKey": "02BC8C02199949B15C005B997E7C8594574E9B02BA2D0628902E0532989976CF9D",
                    "TransactionType": "TrustSet",
                    "TxnSignature": "304402200EF81EC32E0DFA9BE376B20AFCA11765ED9FEA04CA8B77C7178DAA699F7F5AFF02202DA484DBD66521AC317D84F7717EC4614E2F5DB743E313E8B48440499CC0DBA4",
                    "date": 413620090,
                    "hash": "002AA492496A1543DBD3680BF8CF21B6D6A078CE4A01D2C1A4B63778033792CE",
                    "inLedger": 195480,
                    "ledger_index": 195480
                },
                "validated": true
            },
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                    "Balance": "9999999970",
                                    "Flags": 0,
                                    "OwnerCount": 3,
                                    "Sequence": 4
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
                                "PreviousFields": {
                                    "Balance": "9999999980",
                                    "OwnerCount": 2,
                                    "Sequence": 3
                                },
                                "PreviousTxnID": "002AA492496A1543DBD3680BF8CF21B6D6A078CE4A01D2C1A4B63778033792CE",
                                "PreviousTxnLgrSeq": 195480
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Flags": 0,
                                    "Owner": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                                    "RootIndex": "A39F044D860C5B5846AA7E0FAAD44DC8897F0A62B2F628AA073B21B3EC146010"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "A39F044D860C5B5846AA7E0FAAD44DC8897F0A62B2F628AA073B21B3EC146010"
                            }
                        },
                        {
                            "ModifiedNode": {
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "E0D7BDE68B468FF0B8D948FD865576517DA987569833A05374ADB9A72E870A06",
                                "PreviousTxnID": "0222B59280D165D40C464EA75AAD08A4D152C46A38C0625DEECF6EE87FC5B9E1",
                                "PreviousTxnLgrSeq": 343555
                            }
                        },
                        {
                            "CreatedNode": {
                                "LedgerEntryType": "RippleState",
                                "LedgerIndex": "EA4BF03B4700123CDFFB6EB09DC1D6E28D5CEB7F680FB00FC24BC1C3BB2DB959",
                                "NewFields": {
                                    "Balance": {
                                        "currency": "USD",
                                        "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                                        "value": "0"
                                    },
                                    "Flags": 131072,
                                    "HighLimit": {
                                        "currency": "USD",
                                        "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                                        "value": "100"
                                    },
                                    "LowLimit": {
                                        "currency": "USD",
                                        "issuer": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                                        "value": "0"
                                    }
                                }
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "ExchangeRate": "4E11C37937E08000",
                                    "Flags": 0,
                                    "RootIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93",
                                    "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                                    "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                                    "TakerPaysCurrency": "0000000000000000000000004254430000000000",
                                    "TakerPaysIssuer": "5E7B112523F68D2F5E879DB4EAC51C6698A69304"
                                },
                                "LedgerEntryType": "DirectoryNode",
                                "LedgerIndex": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93"
                            }
                        }
                    ],
                    "TransactionIndex": 0,
                    "TransactionResult": "tesSUCCESS"
                },
                "tx": {
                    "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "Fee": "10",
                    "Flags": 0,
                    "LimitAmount": {
                        "currency": "USD",
                        "issuer": "r3PDtZSa5LiYp1Ysn1vMuMzB59RzV3W9QH",
                        "value": "100"
                    },
                    "Sequence": 3,
                    "SigningPubKey": "02BC8C02199949B15C005B997E7C8594574E9B02BA2D0628902E0532989976CF9D",
                    "TransactionType": "TrustSet",
                    "TxnSignature": "3044022058A89552068D1A274EE72BA71363E33E54E6608BC28A84DEC6EE530FC2B5C979022029F4D1EA1237A1F717C5F5EC526E6CFB6DF54C30BADD25EDDE7D2FDBC8F17E34",
                    "date": 416347560,
                    "hash": "53354D84BAE8FDFC3F4DA879D984D24B929E7FEB9100D2AD9EFCD2E126BCCDC8",
                    "inLedger": 343570,
                    "ledger_index": 343570
                },
                "validated": true
            }
        ],
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
      "account" : "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
      "ledger_index_max" : 57112094,
      "ledger_index_min" : 57105464,
      "limit" : 2,
      "marker" : {
         "ledger" : 57112074,
         "seq" : 9
      },
      "status" : "success",
      "transactions" : [
         {
            "ledger_index" : 57112090,
            "meta" : "201C0000002EF8E51100612503677617551E0297F38EF4FED7004E074D246B4EA3E550D9AE0F61BE40E08D3432091D52CE56140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574E624000AB96E624000037771BFD270E1E7220002000024000AB96F2D0000000062400003776C784A418114D2E44C9FAF7BE9C536219800A6E698E4C7D2C911E1E1E311006156F7D315E0E992B1F1AC66B309C9D68961AA327FE770101B74D4C975F8C5DEC96AE8240367761A624000000005478807811403C95DC0C7CE402E8044A5F13304108013CE9963E1E1F1031000",
            "tx_blob" : "120000228000000024000AB96E201B036776306140000000054788076840000000000000287321020A46D8D02AC780C59853ACA309EAA92E7D8E02DD72A0B6AC315A7D18A6C3276A74463044022054811EEF61ACCFA1B5FC6BB05D2FA49CF5174062740370328382E6EA557C0E6A0220480584D487638C333A87CA37100354BD36209E355E8DB9FE79791A56E24C1F268114D2E44C9FAF7BE9C536219800A6E698E4C7D2C911831403C95DC0C7CE402E8044A5F13304108013CE9963",
            "validated" : true
         },
         {
            "ledger_index" : 57112087,
            "meta" : "201C00000026F8E5110061250367760A556B80EE9A9AD3FC40F471F29DCB80C678375137CE36220718902EF1EDCD375E7156140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574E66240000376DEB77118E1E7220002000024000AB96E2D00000000624000037771BFD2708114D2E44C9FAF7BE9C536219800A6E698E4C7D2C911E1E1E511006125036776155591DA498D40AFD90670555F3D719883B48D224B4E4E906C634DEFA21163E8197756CC20FEBEA6D2AF969EC46F2BD92684D9FBABC3F238E841B5E056FE4EBF4379A9E62400071DA26240000001C0D849F8E1E722000200002400071DA32D0000000062400000012DCFE87881146914CB622B8E41E150DE431F48DA244A69809366E1E1F1031000",
            "tx_blob" : "12000022800000002400071DA22E00000001201B0367762D61400000009308615868400000000000002873210381575032E254BF4D699C3D8D6EFDB63B3A71F97475C6F6885BC7DAEEE55D9A0174473045022100E592BCCFD85CCE0B39075EFC66D6BCA594EBB451F12AD5AD9EE533A267F1381B02203635AB46AC110848FC44E797BD19D77A19E10A0F463AA5540B1C62E5D48C81F081146914CB622B8E41E150DE431F48DA244A698093668314D2E44C9FAF7BE9C536219800A6E698E4C7D2C911",
            "validated" : true
         }
      ],
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

この応答は\[標準フォーマット\]\[\]に従っており、正常に完了した場合は結果に次のフィールドが含まれます。

| `Field`            | 型                       | 説明                                                                                         |
|:------------------ |:----------------------- |:------------------------------------------------------------------------------------------ |
| `account`          | 文字列                     | 関連するアカウントを識別する一意の\[アドレス\]\[\]。                                                             |
| `ledger_index_min` | 整数 - \[レジャーインデックス\]\[\] | トランザクションで実際に検索された最古のレジャーのレジャーインデックス。                                                       |
| `ledger_index_max` | 整数 - \[レジャーインデックス\]\[\] | トランザクションで実際に検索された最新のレジャーのレジャーインデックス。                                                       |
| `limit`            | 整数                      | 要求で使用される`limit`値。 （サーバーによって強制される実際の制限値とは異なる場合があります。                                        |
| `marker`           | \[マーカー\]\[\]            | 応答がページネーションされていることを示す、サーバーが定義した値。 この値を次のコールに渡して、このコールで終わった箇所から再開します。                       |
| `transactions`     | 配列                      | 以降で説明する、要求の基準を満たすトランザクションの配列。                                                              |
| `validated`        | ブール値                    | このフィールドが含まれていて`true`に設定されている場合、この応答内の情報は検証済みのレジャーバージョンから取得されています。 そうでない場合、情報は変更されることがあります。 |

**注記:** サーバーから返される`ledger_index_min`および`ledger_index_max`の値が要求で指定したものとは異なる場合があります。

各トランザクションのオブジェクトには以下のフィールドが含まれます。 含まれるフィールドは、JSONまたは16進文字列（`"binary":true`）フォーマットのどちらで要求されたかによって異なります。

| `Field`        | 型                        | 説明                                                                                 |
|:-------------- |:------------------------ |:---------------------------------------------------------------------------------- |
| `ledger_index` | 整数                       | このトランザクションを含むレジャーバージョンの\[レジャーインデックス\]\[\]。                                         |
| `meta`         | オブジェクト（JSON）または文字列（バイナリ） | `binary`がTrueの場合、これは16進文字列のトランザクションメタデータです。 そうでない場合、トランザクションメタデータはJSONフォーマットになります。 |
| `tx`           | オブジェクト                   | （JSONモードのみ）トランザクションを定義するJSONオブジェクト。                                                |
| `tx_blob`      | 文字列                      | （バイナリモードのみ）トランザクションを表す一意のハッシュ化された文字列。                                              |
| `validated`    | ブール値                     | トランザクションが検証済みのレジャーに含まれるかどうか。 検証済みのレジャーに含まれていないトランザクションはすべて、変更される場合があります。           |

## 考えられるエラー

* いずれかの\[汎用エラータイプ\]\[\]。
* `invalidParams` - 1つ以上のフィールドの指定が正しくないか、1つ以上の必須フィールドが指定されていません。
* `actMalformed` - 要求の`account`フィールドに指定した\[アドレス\]\[\]が、正しいフォーマットではありません。
* `lgrIdxMalformed` - `ledger_index_min`または`ledger_index_max`で指定したレジャーが存在しないか、存在してはいるもののサーバーが保有していません。
* `lgrIdxsInvalid` - 要求で`ledger_index_min`の前にある`ledger_index_max`を指定したか、[ネットワークと同期](troubleshoot-the-rippled-server.html)されていないためにサーバーに検証済みレジャーの範囲が存在しません。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
