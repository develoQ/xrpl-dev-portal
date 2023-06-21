---
html: ledger_current.html
parent: ledger-methods.html
blurb: Get the current working ledger version.
labels:
  - Blockchain
---

# ledger_current
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/LedgerCurrent.cpp "Source")

`ledger_current`メソッドは、現在進行中のレジャーの一意のIDを返します。 このコマンドで返されるレジャーは確定されたものではないため、このコマンドは主にテストに有用です。

## 要求フォーマット

要求フォーマットの例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id":2,
  "command":"ledger_current"
}
```

*JSON-RPC*

```json
{
   "method":"ledger_current",
   "params":[
       {}
   ]
}
```

*コマンドライン*

```sh
#Syntax: ledger_current
rippled ledger_current
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_current)

この要求にはパラメーターは含まれていません。


## 応答フォーマット
処理が成功した応答の例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
 "id":2,
 "status":"success",
 "type":"response",
 "result":{
   "ledger_current_index":6643240
 }
}
```

*JSON-RPC*

```json
200 OK

{
   "result":{
       "ledger_current_index":8696233,
       "status":"success"
   }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_current_index" : 56844050,
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

応答は\[標準フォーマット\]\[\]に従っており、正常に完了した場合は結果に次のフィールドが含まれています。

| `Field`                | 型                                       | 説明                                       |
|:---------------------- |:--------------------------------------- |:---------------------------------------- |
| `ledger_current_index` | Unsigned Integer - \[Ledger Index\]\[\] | The ledger index of this ledger version. |

現行レジャーのハッシュは、レジャーの内容とともに常に変化するため、`ledger_hash`フィールドはありません。

## 考えられるエラー

* \[汎用エラータイプ\]\[\]のすべて。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
