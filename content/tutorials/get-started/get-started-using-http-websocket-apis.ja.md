---
html: get-started-using-http-websocket-apis.html
parent: get-started.html
blurb: XRP Ledgerの操作に使用できるAPIとライブラリを使い始めましょう。
cta_text: Get Started
top_nav_name: HTTP / WebSocket
top_nav_grouping: Get Started
labels:
  - 開発
showcase_icon: assets/img/logos/globe.svg
---

# HTTP / WebSocket APIの使用開始

XRP Ledgerのコアサーバーソフトウェアは[`rippled`](xrpl-servers.html)です。 XRP Ledgerでの開発に進むには、`rippled`サーバーのAPIにアクセスします。 The server provides APIs over JSON-RPC and WebSocket protocols. If you don't [run your own instance of `rippled`](install-rippled.html) you can still use a \[public server\]\[public servers\].

APIにアクセスする最も簡単な方法は、[**WebSocket API Tool**](websocket-api-tool.html)を使用するか、[XRP Ledger Explorer](https://livenet.xrpl.org/)を使用してレジャーの進行状況をその場で確認することです。

## Differences Between JSON-RPC and WebSocket

Both JSON-RPC and WebSocket are HTTP-based protocols, and for the most part the data provided over both protocols is the same. The major differences are as follows:

- JSON-RPC uses individual HTTP requests and responses for each call, similar to a RESTful API. You can use any common HTTP client such as [curl](https://curl.se/), [Postman](https://www.postman.com/downloads/), or [Requests](https://requests.readthedocs.io/) to access this API.
- WebSocket uses a persistent connection that allows the server to push data to the client. Functions that require push messages, like [event subscriptions](subscribe.html), are only available using WebSocket.

Both APIs can be served unencrypted (`http://` and `ws://`) or encrypted using TLS (`https://` and `wss://`). Unencrypted connections should not be served over open networks, but can be used when the client is on the same machine as the server.


## 管理者アクセス権限

`rippled`サーバーの[管理メソッド](admin-api-methods.html)を使用するには、次のように行います。 この場合、サーバーのバインド用として設定したIPアドレスとポートを使用する必要があります（例えば`127.0.0.1:54321`）。 また、管理機能にアクセスするには、構成ファイルで管理用としてマークされているポートおよびIPアドレスから接続しなければなりません。

[構成ファイルの例](https://github.com/ripple/rippled/blob/8429dd67e60ba360da591bfa905b58a35638fda1/cfg/rippled-example.cfg#L1050-L1073)では、ローカルループバックネットワーク上（127.0.0.1）のポート5005でJSON-RPC（HTTP）、ポート6006でWebSocket（WS）の接続をリッスンし、接続されるすべてのクライアントを管理者として扱っています。


## WebSocket API

いくつかのメソッドをXRP Ledgerで試すことを予定している場合は、独自のWebSocketコードを記述することなく、[Ripple WebSocket APIツール](websocket-api-tool.html)でAPIをすぐに使用できます。 後ほど、独自の`rippled`サーバーへの接続が必要となった時点で、[ブラウザー](monitor-incoming-payments-with-websocket.html)または[Node.jsで独自のクライアントをビルド](https://www.npmjs.com/package/ws)することが可能です。

Example WebSocket API request:

```json
{
  "id": "my_first_request",
  "command": "server_info",
  "api_version": 1
}
```

The response shows you the current status of the server.

Read more: [Request Formatting >](request-formatting.html) [Response Formatting >](response-formatting.html) \[About the server_info method >\]\[server_info method\]

## JSON-RPC

任意のHTTPクライアント（[RESTED for Firefox](https://addons.mozilla.org/en-US/firefox/addon/rested/)、[Postman for Chrome](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)、[Online HTTP client ExtendsClass](https://extendsclass.com/rest-client-online.html)など）を使用して、JSON-RPCで`rippled`サーバーを呼び出すことができます。 ほとんどのプログラミング言語には、HTTP要求を組み込むためのライブラリーが用意されています。 <!-- SPELLING_IGNORE: extendsclass -->

JSON-RPC URL

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

The response shows you the current status of the server.

以下の属性を指定して、要求の本文を[JSON](https://en.wikipedia.org/wiki/JSON)オブジェクトとして送信します。

## コマンドライン

このコマンドラインインターフェイスは、JSON-RPCのものと同一のサービスに接続するため、公開サーバーおよびサーバー構成は同一です。 By default, the commandline connects to a `rippled` server running on the same machine.

要求の例

```
rippled --conf=/etc/rippled.cfg server_info
```

Read more: [Commandline Usage Reference >](commandline-usage.html)

**注記:** コマンドラインインターフェイスは、管理の目的でのみ使用されることを想定しています。  _サポートされるAPIではありません_。

## Available Methods

For a full list of API methods, see:

- [Public `rippled` Methods](public-api-methods.html): Methods available on public servers, including looking up data from the ledger and submitting transactions.
- [`rippled`の独自のインスタンスを実行](install-rippled.html)したり、[公開サーバー](#公開サーバー)を使用したりすることもできます。


## See Also

- **コンセプト:**
    - [XRP Ledgerの概要](xrp-ledger-overview.html)
    - [Client Libraries](client-libraries.html)
    - [並列ネットワーク](parallel-networks.html)
- **チュートリアル:**
    - [xrpl.js for JavaScriptの使用開始](get-started-using-javascript.html)
    - [信頼できるトランザクションの送信](reliable-transaction-submission.html)
    - [rippledサーバーの管理](manage-the-rippled-server.html)
- **リファレンス:**
    - [rippled APIリファレンス](http-websocket-apis.html)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
