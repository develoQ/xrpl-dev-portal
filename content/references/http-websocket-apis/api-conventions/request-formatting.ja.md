---
html: request-formatting.html
parent: api-conventions.html
blurb: WebSocket、JSON-RPC、コマンドラインインターフェイスの標準の要求フォーマットと例です。
---

# 要求フォーマット

## 要求の例

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
 "id": 2,
 "command": "account_info",
 "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
 "strict": true,
 "ledger_index": "validated"
}
```

*JSON-RPC*

```json
POST http://s1.ripple.com:51234/
{
   "method": "account_info",
   "params": [
       {
           "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
           "strict": true,
           "ledger_index": "validated"
       }
   ]
}
```

*コマンドライン*

```sh
rippled account_info r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59 validated true
```

<!-- MULTICODE_BLOCK_END -->


## WebSocketフォーマット

`rippled`サーバーへのWebSocketを開いた後、以下の属性を使用して、コマンドを[JSON](https://en.wikipedia.org/wiki/JSON)オブジェクトとして送信できます。

| Field                                             | Type      | Description                                                                                                                                       |
|:------------------------------------------------- |:--------- |:------------------------------------------------------------------------------------------------------------------------------------------------- |
| `コマンド名を最上位の<code>"command"`フィールドに指定します。</code> | String    | The name of the [API method](public-api-methods.html).                                                                                            |
| `id`                                              | (Various) | オプションで、任意の値を指定した`"id"`フィールドを指定します。 この要求への応答では、同一の`"id"`フィールドを使用します。 そうすることで、応答が順不同で到達した場合も、どの要求によってどの応答を得られたのかがわかります。                             |
| `api_version`                                     | Number    | _(Optional)_ The API version to use. If omitted, use version 1. For details, see [API Versioning](#api-versioning). \[New in: rippled 1.5.0\]\[\] |
| (Method Parameters)                               | (Various) | Provide any parameters to the method at the top level.                                                                                            |

See [Response Formatting](response-formatting.html) for the response from the server.

## JSON-RPCフォーマット

JSON-RPC要求を実行するには、`rippled`サーバーがJSON-RPC接続をリッスンしているポートおよびIPで、HTTP **POST**要求をルートパス（`/`）に送信します。 HTTP/1.0またはHTTP/1.1を使用できます。 HTTPSを使用する場合は、TLS v1.2を使用してください。 セキュリティ上の理由から、`rippled`ではSSL v3以前を _サポートしていません_ 。

常に`Content-Type`ヘッダー（値`application/json`）を指定してください。

複数の要求を実行する予定の場合は、要求間で接続を閉じてから開く操作を行わずに済むように、[Keep-Alives](http://tools.ietf.org/html/rfc7230#section-6.3)を使用してください。 <!-- SPELLING_IGNORE: alives -->

以下の属性を指定した要求本文を[JSON](https://en.wikipedia.org/wiki/JSON)オブジェクトとして送信します。


| Field    | Type   | Description                                                                                                                                   |
|:-------- |:------ |:--------------------------------------------------------------------------------------------------------------------------------------------- |
| `method` | String | The name of the [API method](public-api-methods.html).                                                                                        |
| `params` | Array  | このフィールドの内容は、コマンドのすべてのパラメーターが指定された1つの入れ子JSONオブジェクトのみを保持している**1要素配列**です。 You may omit this field if the method does not require any parameters. |

The object inside the `params` array can contain the following fields:

| Field               | Type      | Description                                                                                                                                       |
|:------------------- |:--------- |:------------------------------------------------------------------------------------------------------------------------------------------------- |
| `api_version`       | Number    | _(Optional)_ The API version to use. If omitted, use version 1. For details, see [API Versioning](#api-versioning). \[New in: rippled 1.5.0\]\[\] |
| (Method Parameters) | (Various) | Provide any parameters to the method here.                                                                                                        |

See [Response Formatting](response-formatting.html) for the response from the server.

## コマンドライン形式

Put the API method name after any normal (dash-prefaced) commandline options, followed by a limited set of parameters, separated by spaces. スペースやその他の特殊文字が含まれている可能性のあるパラメーター値は、一重引用符で囲みます。 Not all methods have commandline API syntax. For more information, see [Commandline Usage](https://xrpl.org/commandline-usage.html#client-mode-options).

応答はJSONオブジェクトとして返されます。

The commandline always uses the latest [API version](#api-versioning).

**Caution:** The commandline interface is intended for administrative purposes only and is _not a supported API_. New versions of `rippled` may introduce breaking changes to the commandline API without warning!

## API Versioning

The `rippled` server uses a single integer to identify the API version to use. The first API version is `1`; currently, this is the only version of the `rippled` API. (There is no API version 0.) \[New in: rippled 1.5.0\]\[\]

Future versions of `rippled` that introduce breaking changes will introduce a new API version number, such as `2`. The server will support a range of API versions, which it reports in the `version` API method. <!-- STYLE_OVERRIDE: will --> <!-- TODO: add a link when `version` method is documented. --> <!-- Uncomment when multiple API versions exist: Separate API requests can use different API versions even on the same persistent connection. For example, if you connect WebSocket to a server that supports API versions 1 and 2, you can make a server_info request using API version 2 and then make another server_info request using API version 1 from the same connection. -->

### Breaking Changes

The following types of changes are **breaking changes**:

- Removing or renaming a field of a request or response.
- Changing the type of a field of a request or response.
- Changing the meaning of a field of a request or a response.
- Changing the order of positional parameters, or adding a new field before other positional parameters.
- Removing or renaming an API method.
- Changing the behavior of an API function visible to existing clients.
- The following types of breaking changes only apply to the gRPC API:
    - Changing a `proto` field number.
    - Removing or renaming an enum or enum value.
    - Adding or removing fields from a `oneof`.
    - Splitting or merging a `oneof`.
    - Changing whether a message field is `optional`, `repeated`, or `required`.
    - Changing the stream value of a request or response.
    - Deleting or renaming a package or service.

Any time a full release introduces a breaking change, it introduces a new API version number. Pre-release, beta, and development versions may introduce breaking changes to the same API version number.

### Non-Breaking Changes

The following types of changes are **non-breaking changes** and may occur without a change of API version number:

- Adding a new field to a request or response, not including positional parameters.
- Adding a new API method.

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
