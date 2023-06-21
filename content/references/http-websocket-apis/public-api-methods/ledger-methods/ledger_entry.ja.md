---
html: ledger_entry.html
parent: ledger-methods.html
blurb: Get one element from a ledger version.
labels:
  - Blockchain
  - データ保持
---

# ledger_entry
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/LedgerEntry.cpp "Source")

`ledger_entry`メソッドは、XRP Ledgerの1つのレジャーオブジェクトを生フォーマットで返します。 取得可能な各種オブジェクトについては、\[レジャーフォーマット\]\[\]を参照してください。

## 要求フォーマット

このメソッドでは各種データを取得できます。 You can select which type of item to retrieve by passing the appropriate parameters, comprised of the general and type-specific fields listed below, and following the standard [request formatting](request-formatting.html). (For example, a WebSocket request always has the `command` field and optionally an `id` field, and a JSON-RPC request uses the `method` and `params` fields.)

{% include '_snippets/no-cli-syntax.md' %}

### General Fields

| Field          | 型            | 説明                                                                                                                         |
|:-------------- |:------------ |:-------------------------------------------------------------------------------------------------------------------------- |
| `binary`       | ブール値         | _（省略可）_ trueの場合、要求したレジャーオブジェクトの内容が16進文字列として返されます。 それ以外の場合はデータがJSONフォーマットで返されます。 デフォルトは`false`です。 \[更新: rippled 1.2.0\]\[\] |
| `ledger_hash`  | ブロックチェーン     | _（省略可）_ 使用するレジャーバージョンの20バイトの16進文字列。 （\[レジャーの指定\]\[\]を参照してください）                                                             |
| `ledger_index` | 文字列または符号なし整数 | _（省略可）_ 使用するレジャーの\[レジャーインデックス\]\[\]、またはレジャーを自動的に選択するためのショートカット文字列。 （\[レジャーの指定\]\[\]を参照してください）                              |

`generator`パラメーターと`ledger`パラメーターは廃止予定であり、今後予告なしに削除される可能性があります。

In addition to the general fields above, you must specify *exactly 1* of the following fields to indicate what type of object to retrieve, along with its sub-fields as appropriate. The valid fields are:

- [`index`](#get-ledger-object-by-id)
- [`account_root`](#get-accountroot-object)
- [`amm`](#get-amm-object) :not_enabled:
- [`directory`](#get-directorynode-object)
- [`offer`](#get-offer-object)
- [`ripple_state`](#get-ripplestate-object)
- [`check`](#get-check-object)
- [`escrow`](#get-escrow-object)
- [`payment_channel`](#get-paychannel-object)
- [`deposit_preauth`](#get-depositpreauth-object)
- [`ticket`](#get-ticket-object)
- [`nft_page`](#get-nft-page)

**Caution:** If you specify more than 1 of these type-specific fields in a request, the server retrieves results for only 1 of them. It is not defined which one the server chooses, so you should avoid doing this.


### Get Ledger Object by ID

`index` - 一意のIDを指定して任意のタイプのレジャーオブジェクトを取得します。

| Field   | 型   | 説明                                                                                                                                      |
|:------- |:--- |:--------------------------------------------------------------------------------------------------------------------------------------- |
| `index` | 文字列 | The [object ID](ledger-object-ids.html) of a single object to retrieve from the ledger, as a 64-character (256-bit) hexadecimal string. |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 3,
  "command": "ledger_entry",
  "type": "account_root",
  "account_root": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "ledger_entry",
    "params": [
        {
            "account_root": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "ledger_index": "validated",
            "type": "account_root"
        }
    ]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "index": "7DB0788C020F02780A673DC74757F23823FA3014C1866E72CC4CD8B226CD6EF4", "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-by-object-id)

> **Tip:** You can use this type of request to get any singleton object, if it exists in the ledger data, because its ID is always the same. For example:
> 
> - `offer` - 通貨取引オファーを定義する[Offerオブジェクト](offer.html)を取得します。
> - [`FeeSettings`](feesettings.html) - `4BC50C9B0D8515D3EAAE1E74B29A95804346C491EE1A95BF25E4AAB854A6A651`
> - [Recent History `LedgerHashes`](ledgerhashes.html) - `B4979A36CDC7F3D3D5C31A4EAE2AC7D7209DDA877588B9AFC66799692AB0D66B`
> - [`NegativeUNL`](negativeunl.html) - `2E8A59AA9D3B5B186B0B9E0F62E6C02587CA74A4D778938E957B6357D364B244`



### Get AccountRoot Object

`account_root` - [AccountRootオブジェクト](accountroot.html)を取得します。 これは\[account_infoメソッド\]\[\]とほぼ同等です。

| Field          | Type               | Description                                                                    |
|:-------------- |:------------------ |:------------------------------------------------------------------------------ |
| `account_root` | 文字列 - \[アドレス\]\[\] | The classic address of the [AccountRoot object](accountroot.html) to retrieve. |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_accountroot",
  "command": "ledger_entry",
  "account_root": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "ledger_entry",
    "params": [
        {
            "account_root": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "ledger_index": "validated"
        }
    ]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "account_root": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59", "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-accountroot)



### XRP Ledgerの1つのレジャーオブジェクトを生フォーマットで返します。

_(Automated Market Maker (AMM) functionality is part of the proposed [XLS-30d](https://github.com/XRPLF/XRPL-Standards/discussions/78) extension :not_enabled: to the XRP Ledger protocol. You can use these features on AMM test networks, but there isn't an official amendment and they aren't available on the production Mainnet.)_

Retrieve an Automated Market-Maker (AMM) object from the ledger. This is similar to \[amm_info method\]\[\], but the `ledger_entry` version returns only the ledger entry as stored.

| Field        | Type         | Description                                                                                                                                                    |
|:------------ |:------------ |:-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amm`        | オブジェクトまたは文字列 | _（省略可）_ 取得する[Escrowオブジェクト](escrow-object.html)を指定します。 文字列の場合はEscrowの[オブジェクトID](ledger-object-ids.html)（16進数）である必要があります。 オブジェクトの場合は`owner`および`seq`サブフィールドが必要です。 |
| `amm.asset`  | オブジェクト       | One of the two assets in this AMM's pool, as a [currency object without an amount](currency-formats.html#specifying-without-amounts).                          |
| `amm.asset2` | オブジェクト       | The other of the two assets in this AMM's pool, as a [currency object without an amount](currency-formats.html#specifying-without-amounts).                    |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 3,
  "command": "ledger_entry",
  "amm": {
    "asset": {
      "currency": "XRP"
    },
    "asset2": {
      "currency" : "TST",
      "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
    }
  }
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "ledger_entry",
    "params": [
        {
          "amm": {
            "asset": {
              "currency": "XRP"
            },
            "asset2": {
              "currency" : "TST",
              "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
            }
          },
          "ledger_index": "validated"
        }
    ]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "amm": { "asset": { "currency": "XRP" }, "asset2": { "currency" : "TST", "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd" } }, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html?server=wss%3A%2F%2Famm.devnet.rippletest.net%3A51233%2F#ledger_entry-amm)



### Get DirectoryNode Object

Retrieve a [DirectoryNode](directorynode.html), which contains a list of other ledger objects. Can be provided as string (object ID of the Directory) or as an object.

| Field                 | Type         | Description                                                                                                                                                                                         |
|:--------------------- |:------------ |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `directory`           | オブジェクトまたは文字列 | _（省略可）_ 取得する[DepositPreauthオブジェクト](depositpreauth-object.html)を指定します。 文字列の場合はDepositPreauthオブジェクトの[オブジェクトID](ledger-object-ids.html)（16進数）である必要があります。 オブジェクトの場合は`owner`および`authorized`サブフィールドが必要です。 |
| `directory.sub_index` | 符号なし整数       | _（省略可）_ 指定されている場合は、[DirectoryNode](directorynode.html)の後のページにジャンプします。                                                                                                                               |
| `directory.dir_root`  | 文字列          | このメソッドで認識されるすべてのパラメーターのリストを次に示します。                                                                                                                                                                  |
| `directory.owner`     | 文字列          | _（`directory`がオブジェクトとして指定されており、`directory.dir_root`が指定されていない場合に必須）_ このディレクトリーに関連付けられているアカウントの一意のアドレス。                                                                                               |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 3,
  "command": "ledger_entry",
  "directory": {
    "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "sub_index": 0
  },
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "ledger_entry",
    "params": [
        {
            "directory": {
              "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
              "sub_index": 0
            },
            "ledger_index": "validated"
        }
    ]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "directory": { "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "sub_index": 0 }, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-directorynode)



### 要求フォーマットの例:

Retrieve an [Offer object](offer.html), which defines an offer to exchange currency. Can be provided as string (unique index of the Offer) or as an object.

| Field           | Type               | Description                                                                                                                                                 |
|:--------------- |:------------------ |:----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `offer`         | オブジェクトまたは文字列       | _（省略可）_ 取得する[Offer オブジェクト](offer.html)を指定します。 文字列の場合はOfferの[一意のインデックス](ledgers.html#ツリーの形式)として解釈されます。 オブジェクトの場合は、オファーを一意に識別するためサブフィールド`account`と`seq`が必要です。 |
| `offer.account` | 文字列 - \[アドレス\]\[\] | _（`offer`が指定されている場合に必須）_ オファーを出したアカウント。                                                                                                                     |
| `offer.seq`     | 符号なし整数             | _（`offer`が指定されている場合に必須）_ Offerオブジェクトを作成したトランザクションのシーケンス番号。                                                                                                  |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_offer",
  "command": "ledger_entry",
  "offer": {
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "seq": 359
  },
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [
    {
      "offer": {
        "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "seq": 359
      },
      "ledger_index": "validated"
    }
  ]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "offer": { "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "seq": 359}, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-offer)



### Get RippleState Object

`ripple_state` - 2つのアカウント間の（XRP以外の）通貨の残高を追跡する[RippleStateオブジェクト](ripplestate.html)を取得します。

| Field                   | Type   | Description                                                                                                           |
|:----------------------- |:------ |:--------------------------------------------------------------------------------------------------------------------- |
| `ripple_state`          | オブジェクト | _（省略可）_ 取得するRippleState（トラストライン）オブジェクトを指定するオブジェクト。 取得するRippleStateエントリを一意に指定するため、`accounts`および`currency`サブフィールドが必要です。 |
| `ripple_state.accounts` | 配列     | _（省略可）_ 取得する[AccountRootオブジェクト](accountroot.html)を指定します。                                                              |
| `ripple_state.currency` | 文字列    | _（省略可）_ レジャーから取得する1つのオブジェクトの[オブジェクトID](ledger-object-ids.html)を指定します。                                                 |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_ripplestate",
  "command": "ledger_entry",
  "ripple_state": {
    "accounts": [
      "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW"
    ],
    "currency": "USD"
  },
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "ripple_state": {
      "accounts": [
        "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW"
      ],
      "currency": "USD"
    },
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "ripple_state": { "accounts": ["rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW"], "currency": "USD"}, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-ripplestate)



### Get Check Object

`check` - 受取人が現金化できる支払いである[Checkオブジェクト](check.html)を取得します。 \[新規: rippled 1.0.0\]\[\]

| Field   | Type | Description                                                                           |
|:------- |:---- |:------------------------------------------------------------------------------------- |
| `check` | 文字列  | _（省略可）_ レジャーから取得する[Checkオブジェクト](check.html)の[オブジェクトID](ledger-object-ids.html)を指定します。 |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_check",
  "command": "ledger_entry",
  "check": "C4A46CCD8F096E994C4B0DEAB6CE98E722FC17D7944C28B95127C2659C47CBEB",
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "check": "C4A46CCD8F096E994C4B0DEAB6CE98E722FC17D7944C28B95127C2659C47CBEB",
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "check": "C4A46CCD8F096E994C4B0DEAB6CE98E722FC17D7944C28B95127C2659C47CBEB", "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-check)



### Get Escrow Object

`escrow` - 特定の時刻または条件に一致するまでXRPを保有する[Escrowオブジェクト](escrow-object.html)を取得します。 Can be provided as string (object ID of the Escrow) or as an object. \[新規: rippled 1.0.0\]\[\]

| Field          | Type               | Description                                                                                                                                                                  |
|:-------------- |:------------------ |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `escrow`       | オブジェクトまたは文字列       | The [Escrow object](escrow-object.html) to retrieve. 文字列の場合はディレクトリーの[オブジェクトID](ledger-object-ids.html)（16進数）である必要があります。 If an object, requires `owner` and `seq` sub-fields. |
| `escrow.owner` | 文字列 - \[アドレス\]\[\] | _（`escrow`がオブジェクトとして指定されている場合に必須）_ Escrowオブジェクトの所有者（送金元）。                                                                                                                    |
| `escrow.seq`   | 符号なし整数             | _（`escrow`がオブジェクトとして指定されている場合に必須）_ Escrowオブジェクトを作成したトランザクションのシーケンス番号。                                                                                                        |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_escrow",
  "command": "ledger_entry",
  "escrow": {
    "owner": "rL4fPHi2FWGwRGRQSH7gBcxkuo2b9NTjKK",
    "seq": 126
  },
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "escrow": {
      "owner": "rL4fPHi2FWGwRGRQSH7gBcxkuo2b9NTjKK",
      "seq": 126
    },
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "escrow": { "owner": "rL4fPHi2FWGwRGRQSH7gBcxkuo2b9NTjKK", "seq": 126 }, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-escrow)



### Get PayChannel Object

`payment_channel` - 非同期支払いのためにXRPを保有する[PayChannelオブジェクト](paychannel.html)を取得します。 \[新規: rippled 1.0.0\]\[\]

| Field             | Type | Description                                                                                |
|:----------------- |:---- |:------------------------------------------------------------------------------------------ |
| `payment_channel` | 文字列  | _（省略可）_ 取得する[PayChannel オブジェクト](paychannel.html)の[オブジェクトID](ledger-object-ids.html)を指定します。 |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_paychannel",
  "command": "ledger_entry",
  "payment_channel": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7",
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "payment_channel": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7",
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "payment_channel": "C7F634794B79DB40E87179A9D1BF05D05797AE7E92DF8E93FD6656E8C4BE3AE7", "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-paychannel)


### Get DepositPreauth Object

`deposit_preauth` - [Deposit Authorization](depositauth.html)を必要とするアカウントへの支払いの事前承認を追跡する[DepositPreauthオブジェクト](depositpreauth-object.html)を取得します。 Can be provided as string (object ID of the DepositPreauth) or as an object. \[新規: rippled 1.1.0\]\[\]

| Field                        | Type               | Description                                                                                                                                                                                                                               |
|:---------------------------- |:------------------ |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deposit_preauth`            | オブジェクトまたは文字列       | Specify a [DepositPreauth object](depositpreauth-object.html) to retrieve. If a string, must be the [object ID](ledger-object-ids.html) of the DepositPreauth object, as hexadecimal. オブジェクトの場合はサブフィールドとして`dir_root`または`owner`のいずれかが必要です。 |
| `deposit_preauth.owner`      | 文字列 - \[アドレス\]\[\] | _（`deposit_preauth`がオブジェクトとして指定されている場合に必須）_ 事前承認を提供したアカウント。                                                                                                                                                                               |
| `deposit_preauth.authorized` | 試してみる >            | _（`deposit_preauth`がオブジェクトとして指定されている場合に必須）_ 事前承認を受けたアカウント。                                                                                                                                                                                |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_deposit_preauth",
  "command": "ledger_entry",
  "deposit_preauth": {
    "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "authorized": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
  },
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "deposit_preauth": {
      "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "authorized": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
    },
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "deposit_preauth": { "owner": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "authorized": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX" }, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-depositpreauth)


### Get Ticket Object

Retrieve a [Ticket object](ticket.html), which represents a \[sequence number\]\[\] set aside for future use. Can be provided as string (object ID of the Ticket) or as an object. _(Added by the \[TicketBatch amendment\]\[\])_

| Field               | Type         | Description                                                                                                                                                                                                                                          |
|:------------------- |:------------ |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ticket`            | オブジェクトまたは文字列 | The [Ticket object](ticket.html) to retrieve. If a string, must be the [object ID](ledger-object-ids.html) of the Ticket, as hexadecimal. If an object, the `account` and `ticket_seq` sub-fields are required to uniquely specify the Ticket entry. |
| `ticket.account`    | 試してみる >      | _（`directory`がオブジェクトとして指定されており、`directory.owner`が指定されていない場合に必須）_ 取得するディレクトリーを識別する一意のインデックス（16進数）。                                                                                                                                                    |
| `ticket.ticket_seq` | Number       | _(Required if `ticket` is specified as an object)_ The Ticket Sequence number of the Ticket entry to retrieve.                                                                                                                                       |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "example_get_ticket",
  "command": "ledger_entry",
  "ticket": {
    "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
    "ticket_seq": 389
  },
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "ticket": {
      "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "ticket_seq": 389
    },
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "ticket": { "account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "ticket_seq: 389 }, "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-ticket)


### Get NFT Page

Return an NFT Page in its raw ledger format.

| `Field`    | Type | Description                                                                     |
|:---------- |:---- |:------------------------------------------------------------------------------- |
| `nft_page` | 文字列  | `directory` - 他のレジャーオブジェクトのリストが含まれている[DirectoryNode](directorynode.html)を取得します。 |

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "example_get_nft_page",
    "command": "ledger_entry",
    "nft_page": "255DD86DDF59D778081A06D02701E9B2C9F4F01DFFFFFFFFFFFFFFFFFFFFFFFF",
    "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "ledger_entry",
  "params": [{
    "nft_page": "255DD86DDF59D778081A06D02701E9B2C9F4F01DFFFFFFFFFFFFFFFFFFFFFFFF",
    "ledger_index": "validated"
  }]
}
```

*Commandline*

```sh
rippled json ledger_entry '{ "nft_page": "255DD86DDF59D778081A06D02701E9B2C9F4F01DFFFFFFFFFFFFFFFFFFFFFFFF", "ledger_index": "validated" }'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_entry-nft-page)

## 応答フォーマット

この応答は\[標準フォーマット\]\[\]に従っており、正常に完了した場合は結果に次のフィールドが含まれます。

| Field          | Type   | Description                                                                            |
|:-------------- |:------ |:-------------------------------------------------------------------------------------- |
| `index`        | 文字列    | この[レジャーオブジェクト](ledger-object-types.html)の一意のID。                                        |
| `ledger_index` | 符号なし整数 | このデータの取得時に使用されたレジャーの\[レジャーインデックス\]\[\]。                                                |
| `node`         | オブジェクト | _（`"binary": true`が指定されている場合は省略）_\[レジャーフォーマット\]\[\]に基づく、このレジャーオブジェクトのデータが含まれているオブジェクト。 |
| `node_binary`  | 文字列    | _（`"binary":true`が指定されていない場合は省略可）_ レジャーオブジェクトの[バイナリー表現](serialization.html)（16進数）。     |

処理が成功した応答の例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 3,
    "result": {
        "index": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
        "ledger_index": 6889347,
        "node": {
            "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "Balance": "27389517749",
            "Flags": 0,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 18,
            "PreviousTxnID": "B6B410172C0B65575D89E464AF5B99937CC568822929ABF87DA75CBD11911932",
            "PreviousTxnLgrSeq": 6592159,
            "Sequence": 1400,
            "index": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05"
        }
    },
    "status": "success",
    "type": "response"
}
```

*JSON-RPC*

```json
200 OK

{
  "result": {
    "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8",
    "ledger_hash": "395946243EA36C5092AE58AF729D2875F659812409810A63096AC006C73E656E",
    "ledger_index": 61966165,
    "node": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "AccountTxnID": "4E0AA11CBDD1760DE95B68DF2ABBE75C9698CEB548BEA9789053FCB3EBD444FB",
      "Balance": "424021949",
      "Domain": "6D64756F31332E636F6D",
      "EmailHash": "98B4375E1D753E5B91627516F6D70977",
      "Flags": 9568256,
      "LedgerEntryType": "AccountRoot",
      "MessageKey": "0000000000000000000000070000000300",
      "OwnerCount": 12,
      "PreviousTxnID": "4E0AA11CBDD1760DE95B68DF2ABBE75C9698CEB548BEA9789053FCB3EBD444FB",
      "PreviousTxnLgrSeq": 61965653,
      "RegularKey": "rD9iJmieYHn8jTtPjwwkW2Wm9sVDvPXLoJ",
      "Sequence": 385,
      "TransferRate": 4294967295,
      "index": "13F1A95D7AAB7108D5CE7EEAF504B2894B8C674E6D68499076441C4837282BF8"
    },
    "status": "success",
    "validated": true
  }
}
```

*Commandline*

```json
OK

{
    "result": {
        "index": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05",
        "ledger_index": 8696234,
        "node": {
            "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "Balance": "13176802787",
            "Flags": 0,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 17,
            "PreviousTxnID": "E5D0235A236F7CD162C1AB87A0325056AE61CFC63D92D1494AB5D826AAD0CDCA",
            "PreviousTxnLgrSeq": 8554742,
            "Sequence": 1406,
            "index": "4F83A2CF7E70F77F79A307E6A472BFC2585B806A70833CCD1C26105BAE0D6E05"
        },
        "status": "success",
        "validated": true
    }
}
```

<!-- MULTICODE_BLOCK_END -->


## 考えられるエラー

* いずれかの\[汎用エラータイプ\]\[\]。
* `deprecatedFeature` - 削除されたフィールド（`generator`など）が要求に指定されていました。
* `entryNotFound` - 要求されたレジャーオブジェクトはレジャーに存在しません。
* `invalidParams` - 1つ以上のフィールドの指定が正しくないか、1つ以上の必須フィールドが指定されていません。
* `lgrNotFound` - `ledger_hash`または`ledger_index`で指定したレジャーが存在しないか、存在してはいるもののサーバーが保有していません。
* `malformedAddress` - 要求の\[アドレス\]\[\]フィールドが誤って指定されています。
* `malformedCurrency` - 要求の\[通貨コード\]\[\]フィールドが誤って指定されています。
* `malformedOwner` - 要求の`escrow.owner`サブフィールドが誤って指定されています。
* `malformedRequest` - 要求にフィールドが無効な組み合わせで指定されているか、1つ以上のフィールドの型が誤っています。
* `unknownOption` - 要求に指定されたフィールドが、予期される要求フォーマットのいずれにも一致していません。



<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
