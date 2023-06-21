---
html: account_lines.html
parent: account-methods.html
blurb: アカウントのトラストラインに関する情報を返します。
labels:
  - トークン
---

# account_lines
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountLines.cpp "Source")

`account_lines`メソッドは、アカウントのトラストラインに関する情報（XRP以外のあらゆる通貨の残高と資産など）を返します。 取得された情報はすべて、特定バージョンのレジャーに関連付けられています。

## 要求フォーマット

要求フォーマットの例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "command": "account_lines",
  "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
}
```

*JSON-RPC*

```json
{
    "method": "account_lines",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
        }
    ]
}
```

*Commandline*

```sh
#Syntax: account_lines <account> [<peer>] [<ledger_index>|<ledger_hash>]
rippled account_lines r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_lines)

要求には以下のパラメーターを指定できます。

| `Field`        | 型            | 説明                                                                                                                 |
|:-------------- |:------------ |:------------------------------------------------------------------------------------------------------------------ |
| `account`      | 文字列          | アカウントの一意のIDであり、通常はアカウントの\[アドレス\]\[\]です。                                                                            |
| `ledger_hash`  | 文字列          | _（省略可）_ 使用するレジャーバージョンの20バイトの16進文字列。 （\[レジャーの指定\]\[\]を参照してください）                                                     |
| `ledger_index` | 文字列または符号なし整数 | _（省略可）_ 使用するレジャーの\[レジャーインデックス\]\[\]、またはレジャーを自動的に選択するためのショートカット文字列。 （\[レジャーの指定\]\[\]を参照してください）                      |
| `peer`         | 文字列          | _（省略可）_ 2番目のアカウントの\[アドレス\]\[\]。 指定されている場合は、2つのアカウントを結ぶトラストラインだけが出力されます。                                            |
| `limit`        | 整数           | （省略可、デフォルト値は可変）取得するトラストラインの数を制限します。 サーバーはこの値を受け入れる必要はありません。 10以上400以下の範囲で値を指定する必要があります。 \[新規: rippled 0.26.4\]\[\] |
| `marker`       | \[マーカー\]\[\] | _（省略可）_ 以前にページネーションされた応答の値。 その応答を停止した箇所からデータの取得を再開します。 \[新規: rippled 0.26.4\]\[\]                                  |

以下のパラメーターは廃止予定であり、今後予告なしに削除される可能性があります。 `ledger`および`peer_index`。

## 応答フォーマット

処理が成功した応答の例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "status": "success",
    "type": "response",
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "lines": [
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "ASP",
                "limit": "0",
                "limit_peer": "10",
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "XAU",
                "limit": "0",
                "limit_peer": "0",
                "no_ripple": true,
                "no_ripple_peer": true,
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                "balance": "3.497605752725159",
                "currency": "USD",
                "limit": "5",
                "limit_peer": "0",
                "no_ripple": true,
                "quality_in": 0,
                "quality_out": 0
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
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "lines": [
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "ASP",
                "limit": "0",
                "limit_peer": "10",
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "XAU",
                "limit": "0",
                "limit_peer": "0",
                "no_ripple": true,
                "no_ripple_peer": true,
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "rs9M85karFkCRjvc6KMWn8Coigm9cbcgcx",
                "balance": "0",
                "currency": "015841551A748AD2C1F76FF6ECB0CCCD00000000",
                "limit": "10.01037626125837",
                "limit_peer": "0",
                "no_ripple": true,
                "quality_in": 0,
                "quality_out": 0
            }
        ],
        "status": "success"
    }
}
```

*Commandline*
```json
{
   "result" : {
      "account" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "ledger_current_index" : 56867265,
      "lines" : [
         {
            "account" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "balance" : "0",
            "currency" : "ASP",
            "limit" : "0",
            "limit_peer" : "10",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "balance" : "0",
            "currency" : "XAU",
            "limit" : "0",
            "limit_peer" : "0",
            "no_ripple" : true,
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
            "balance" : "5",
            "currency" : "USD",
            "limit" : "5",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rHpXfibHgSb64n8kK9QWDpdbfqSpYbM9a4",
            "balance" : "481.992867407479",
            "currency" : "MXN",
            "limit" : "1000",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
            "balance" : "0.793598266778297",
            "currency" : "EUR",
            "limit" : "1",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
            "balance" : "0",
            "currency" : "CNY",
            "limit" : "3",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
            "balance" : "1.336889190631542",
            "currency" : "DYM",
            "limit" : "3",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "0.3488146605801446",
            "currency" : "CHF",
            "limit" : "0",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "0",
            "currency" : "BTC",
            "limit" : "3",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "11.68225001668339",
            "currency" : "USD",
            "limit" : "5000",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rpgKWEmNqSDAGFhy5WDnsyPqfQxbWxKeVd",
            "balance" : "-0.00111",
            "currency" : "BTC",
            "limit" : "0",
            "limit_peer" : "10",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rBJ3YjwXi2MGbg7GVLuTXUWQ8DjL7tDXh4",
            "balance" : "-0.0008744482690504699",
            "currency" : "BTC",
            "limit" : "0",
            "limit_peer" : "10",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
            "balance" : "0",
            "currency" : "USD",
            "limit" : "1",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
            "balance" : "9.07619790068559",
            "currency" : "CNY",
            "limit" : "100",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "7.292695098901099",
            "currency" : "JPY",
            "limit" : "0",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "balance" : "0",
            "currency" : "AUX",
            "limit" : "0",
            "limit_peer" : "0",
            "no_ripple" : true,
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
            "balance" : "0.0004557360418801623",
            "currency" : "USD",
            "limit" : "1",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "12.41688780720394",
            "currency" : "EUR",
            "limit" : "100",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rfF3PNkwkq1DygW2wum2HK3RGfgkJjdPVD",
            "balance" : "35",
            "currency" : "USD",
            "limit" : "500",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rwUVoVMSURqNyvocPCcvLu3ygJzZyw8qwp",
            "balance" : "-5",
            "currency" : "JOE",
            "limit" : "0",
            "limit_peer" : "50",
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rE6R3DWF9fBD7CyiQciePF9SqK58Ubp8o2",
            "balance" : "0",
            "currency" : "USD",
            "limit" : "0",
            "limit_peer" : "100",
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rE6R3DWF9fBD7CyiQciePF9SqK58Ubp8o2",
            "balance" : "0",
            "currency" : "JOE",
            "limit" : "0",
            "limit_peer" : "100",
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rs9M85karFkCRjvc6KMWn8Coigm9cbcgcx",
            "balance" : "0",
            "currency" : "015841551A748AD2C1F76FF6ECB0CCCD00000000",
            "limit" : "10.01037626125837",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rEhDDUUNxpXgEHVJtC2cjXAgyx5VCFxdMF",
            "balance" : "0",
            "currency" : "USD",
            "limit" : "0",
            "limit_peer" : "1",
            "quality_in" : 0,
            "quality_out" : 0
         }
      ],
      "status" : "success",
      "validated" : false
   }
}
```

<!-- MULTICODE_BLOCK_END -->

この応答は\[標準フォーマット\]\[\]に従っており、正常に完了した場合には、アカウントのアドレスとトラストラインオブジェクトの配列が含まれています。 具体的には、結果オブジェクトには以下のフィールドが含まれます。

| `Field`                | 型                       | 説明                                                                                                                      |
|:---------------------- |:----------------------- |:----------------------------------------------------------------------------------------------------------------------- |
| `account`              | 文字列                     | この要求に対応するアカウントの一意の\[アドレス\]\[\]。 トラストラインのための「パースペクティブアカウント」です。                                                           |
| `lines`                | 配列                      | トラストラインオブジェクトからなる配列。 以下で説明します。 トラストラインの数が多い場合は、一度に`limit`の数までのトラストラインが返されます。                                            |
| `ledger_current_index` | 整数 - \[レジャーインデックス\]\[\] | _（`ledger_hash`または`ledger_index`が指定されている場合は省略）_ この情報の取得時に使用した、現行のオープンレジャーのレジャーインデックス。 \[新規: rippled 0.26.4-sp1\]\[\]    |
| `ledger_index`         | 整数 - \[レジャーインデックス\]\[\] | _（`ledger_current_index`が代わりに指定されている場合は省略）_ このデータの取得時に使用されたレジャーバージョンのレジャーインデックス。 \[新規: rippled 0.26.4-sp1\]\[\]         |
| `ledger_hash`          | 文字列 - \[ハッシュ\]\[\]      | _（省略される場合があります）_ このデータの取得時に使用されたレジャーバージョンの識別用ハッシュ。 \[新規: rippled 0.26.4-sp1\]\[\]                                       |
| `marker`               | \[マーカー\]\[\]            | 応答がページネーションされていることを示す、サーバーが定義した値。 この値を次のコールに渡して、このコールで終わった箇所から再開します。 この後に追加のページがない場合は省略されます。 \[新規: rippled 0.26.4\]\[\] |

各トラストラインオブジェクトには以下のフィールドの組み合わせが含まれています。

| `Field`           | 型      | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:----------------- |:------ |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `account`         | 文字列    | The unique \[Address\]\[\] of the counterparty to this trust line.                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `balance`         | 文字列    | 現在このラインに対して保留されている残高（数値）の表示。 残高がプラスの場合はパースペクティブアカウントがその額を保有しており、マイナスの場合はパースペクティブアカウントがその額を借用しています。                                                                                                                                                                                                                                                                                                                                                                                                      |
| `currency`        | 文字列    | このトラストラインが保有できる通貨を示す\[通貨コード\]\[\]。                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `limit`           | 文字列    | このアカウントがピアアカウントからの借用を希望する特定の通貨の上限額。                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `limit_peer`      | 文字列    | 相手側アカウントがパースペクティブアカウントからの借用を希望する特定の通貨の上限額。                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `quality_in`      | 符号なし整数 | このアカウントが、このトラストラインの出金時残高を評価する際のレート（この数値対10億単位の比率）。 （たとえば5億の場合は0.5:1の比率を表します。 ）特殊なケースとして、0は1:1の比率として扱われます。                                                                                                                                                                                                                                                                                                                                                                                               |
| `quality_out`     | 符号なし整数 | このアカウントが、このトラストラインの入金時残高を評価する際のレート（この数値対10億単位の比率）。 （たとえば5億の場合は0.5:1の比率を表します。 ）特殊なケースとして、0は1:1の比率として扱われます。                                                                                                                                                                                                                                                                                                                                                                                               |
| `no_ripple`       | ブール値   | （省略される場合があります）ピアアカウントでこのラインに対し[NoRippleフラグ](rippling.html)が有効な場合は`true`。 If present and `false`, this account has disabled the No Ripple flag, but, because the account also has the Default Ripple flag disabled, that is not considered [the default state](ripplestate.html#contributing-to-the-owner-reserve). If omitted, the account has the No Ripple flag disabled for this trust line and Default Ripple enabled. \[Updated in: rippled 1.7.0\]\[\]                                            |
| `no_ripple_peer`  | ブール値   | _(May be omitted)_ If `true`, the peer account has enabled the [No Ripple flag](rippling.html) for this trust line. If present and `false`, this account has disabled the No Ripple flag, but, because the account also has the Default Ripple flag disabled, that is not considered [the default state](ripplestate.html#contributing-to-the-owner-reserve). If omitted, the account has the No Ripple flag disabled for this trust line and Default Ripple enabled. \[Updated in: rippled 1.7.0\]\[\] |
| `authorized`      | ブール値   | （省略される場合があります）このアカウントが[このトラストラインを承認した](authorized-trust-lines.html)場合は、`true`。 省略されている場合は、`false`と同じです。                                                                                                                                                                                                                                                                                                                                                                                                 |
| `peer_authorized` | ブール値   | （省略される場合があります）ピアアカウントが[このトラストラインを承認した](authorized-trust-lines.html)場合は`true`。 省略されている場合は、`false`と同じです。                                                                                                                                                                                                                                                                                                                                                                                                  |
| `freeze`          | ブール値   | （省略される場合があります）このアカウントがこのトラストラインを[凍結](freezes.html)した場合は`true`。 省略されている場合は、`false`と同じです。                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `freeze_peer`     | ブール値   | （省略される場合があります）ピアアカウントがこのトラストラインを[凍結](freezes.html)した場合は、`true`。 省略されている場合は、`false`と同じです。                                                                                                                                                                                                                                                                                                                                                                                                                |

## 考えられるエラー

* いずれかの\[汎用エラータイプ\]\[\]。
* `invalidParams` - 1つ以上のフィールドの指定が正しくないか、1つ以上の必須フィールドが指定されていません。
* `actNotFound` - 要求の`account`フィールドに指定されている\[アドレス\]\[\]が、レジャーのアカウントに対応していません。
* `lgrNotFound` - `ledger_hash`または`ledger_index`で指定したレジャーが存在しないか、存在してはいるもののサーバーが保有していません。
* `actMalformed` - 指定されている`marker`フィールドが受け入れられない場合。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
