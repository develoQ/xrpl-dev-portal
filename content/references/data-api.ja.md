---
html: data-api.html
parent: references.html
blurb: XRP Ledger分析と履歴データに対するRESTfulインターフェイスです。
status: removed
nav_omit: true
---

# Ripple Data API v2

**警告:** Ripple Data API v2は廃止されました。 代わりに[HTTP / WebSocket API](https://xrpl.org/http-websocket-apis.html)を使って下さい。

廃止されましたRipple Data APIについては[rippled-historical-database レポジトリー](https://github.com/ripple/rippled-historical-database)をご覧下さい.

## Alternatives

For most common operations, like requesting account balances or transaction history, you can query a self-hosted or [public XRP Ledger server](public-servers.html) using a [WebSocket connection](get-started-using-http-websocket-apis.html#websocket-api) or [JSON-RPC (HTTP POST)](get-started-using-http-websocket-apis.html#json-rpc).

See the [Get Started Using HTTP / WebSocket APIs](get-started-using-http-websocket-apis.html) page for more information.

Ripple Data API v2を使用すると、XRP Ledgerの変更に関する情報（トランザクション履歴や処理済みの分析データなど）にアクセスできます。
