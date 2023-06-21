---
html: rippled-server-modes.html
parent: xrpl-servers.html
blurb: Learn about rippled server modes, including stock servers, validator servers, and rippled servers run in stand-alone mode.
labels:
  - コアサーバー
---

# rippledサーバーのモード

`rippled`サーバーソフトウェアは、その設定に応じて以下のようなさまざまなモードで実行できます。

- [**P2P Mode**](#p2p-mode) - This is the main mode of the server: it follows the peer-to-peer network, processes transactions, and maintains some amount of [ledger history](ledger-history.html). This mode can be configured to do any or all of the following roles:
    - [**Validator**](#validators) - Helps secure the network by participating in consensus
    - [**API Server**](#api-servers) - Provides [API access](get-started-using-http-websocket-apis.html) to read data from the shared ledger, submit transactions, and watch activity in the ledger. Optionally, this can be a [**Full History Server**](#full-history-servers), which keeps a complete record of transaction and ledger history.
    - [**Hub Server**](#public-hubs) - Relays messages between many other members of the peer-to-peer network.
- [**Reporting mode**](#reporting-mode) - A specialized mode for serving API requests from a relational database. It does not participate in the peer-to-peer network, so you need to run a P2P Mode server and connect the reporting mode server using a trusted gRPC connection. \[New in: rippled 1.7.0\]\[\]
- **注意:** スタンドアロンモードでは[レジャーを手動で進める](advance-the-ledger-in-stand-alone-mode.html)必要があります。 Does not connect to the peer-to-peer network or use consensus.

また、[`rippled` API](http-websocket-apis.html)にローカルでアクセスするためのクライアントアプリケーションとして、`rippled`実行可能ファイルを実行できます。 （この場合同じバイナリの2つのインスタンスを並列して実行できます。 1つのインスタンスをサーバーとして実行し、もう1つのインスタンスをクライアントとして一時的に実行して終了します。 ）

各モードでrippledを実行するためのコマンドについては、[rippledコマンドライン使用リファレンス](commandline-usage.html)を参照してください。


## P2P Mode

P2P Mode is the main and default mode of the `rippled` server, and it can handle almost anything you might want your server to do. These servers form a peer-to-peer network that processes transactions and maintains the shared state of the XRP Ledger. If you want to submit transactions, read ledger data, or otherwise participate in the network, your requests must go through a P2P Mode server at some point.

P2P Mode servers can be further configured to provide additional functionality. A server running in P2P Mode with a minimally-modified config file is also called a _stock server_. Other configurations include:

- [Validator](#validators)
- [API Server](#api-servers)
- [公開ハブ](#public-hubs)

P2P Mode servers connect to [Mainnet](parallel-networks.html) by default.


### API Servers

All P2P Mode servers provide [APIs](http-websocket-apis.html) for purposes like submitting transactions, checking balances and settings, and administering the server. If you query the XRP Ledger for data or submit transactions for business use, it can be useful to [run your own server](xrpl-servers.html#reasons-to-run-your-own-server).

#### Full History Servers

Unlike some other blockchains, the XRP Ledger does not require servers to have a complete transaction history to know the current state and process new transactions. As a server operator, you decide how much [ledger history](ledger-history.html) to store at a time. However, a P2P Mode server can only answer API requests using the ledger history it has locally available. For example, if you keep six months of history, your server can't describe the outcome of a transaction from a year ago. API servers with [full history](ledger-history.html#full-history) can report all transactions and balances since the start of the XRP Ledger.


### 関連項目

公開ハブは、他のサーバーへの[ピアプロトコル接続](peer-protocol.html)が多数あるストックサーバーを指します。 ストックサーバーを公開ハブとして実行することで、XRP Ledgerネットワークの効率的な接続を維持できます。 適切に運用されている公開ハブには、以下の特徴があります。

- 十分な帯域幅。

- 多数の信頼できるピアとの接続。

- メッセージを確実に中継する能力。

To configure your server as a hub, increase the maximum number of peers allowed and make sure you've [forwarded the appropriate ports](forward-ports-for-peering.html) through your firewall and NAT (network address translation) router as appropriate.


### Validators

XRP Ledgerの堅牢性は、バリデータが相互に接続されたネットワークに依存しています。 各バリデータは、他の何人かのバリデータが _共謀しない_ と信頼しています。 In addition to processing each transaction and calculating ledger state exactly like other P2P Mode servers, validators participate actively in the [consensus protocol](consensus.html). If you or your organization relies on the XRP Ledger, it is in your interest to contribute to the consensus process by running _one_ server as a validator.

Validation uses only a small amount of computing resources, but there is not much benefit to a single entity or organization running multiple validators because doing so does not provide more protections against collusion. Each validator identifies itself with a unique cryptographic key pair that must be carefully managed; multiple validators must not share a key pair. For these reasons, validation is disabled by default.

You can safely enable validation on a server that is also used for other purposes; this type of configuration is called an _all-purpose server_. Alternatively, you can run a _dedicated validator_ that does not perform other tasks, possibly in a [cluster](clustering.html) with other P2P Mode `rippled` servers.

バリデータの実行についての詳細は、[バリデータとしての`rippled`の実行](run-rippled-as-a-validator.html)を参照してください。


## レポーティングモード
\[導入: rippled 1.7.0\]\[\][]

Reporting mode is specialized mode for serving API requests more efficiently. In this mode, the server gets the latest validated ledger data over [gRPC](https://xrpl.org/configure-grpc.html) from a separate `rippled` server running in P2P Mode, then loads that data into a relational database ([PostgreSQL](https://www.postgresql.org/)). The reporting mode server does not directly participate in the peer-to-peer network, though it can forward requests such as transaction submission to the P2P Mode server it uses.

Multiple reporting mode servers can share access to a PostgreSQL database and [Apache Cassandra](https://cassandra.apache.org/) cluster to serve a large amount of history without each server needing a redundant copy of all the data. Reporting mode servers provide this data through the same [`rippled` APIs](http-websocket-apis.html) with some slight changes to accommodate for the differences in how they store the underlying data.

Most notably, reporting mode servers do not report pending, non-validated ledger data or transactions. This limitation is relevant to certain use cases that rely on rapid access to in-flux data, such as performing arbitrage in the [decentralized exchange](decentralized-exchange.html).

<!-- TODO: link setup steps for Reporting Mode when those are ready -->


## advance-the-ledger-in-stand-alone-mode.html

In stand-alone mode, the server operates without connecting to the network and participating in the consensus process. Without the consensus process, you have to manually advance the ledger and no distinction is made between "closed" and "validated" ledgers. However, the server still provides API access and processes transactions the same. This enables you to:

- [Test the effects of Amendments](test-amendments.html) before those Amendments have gone into effect across the decentralized network.
- [新しいジェネシスレジャー](start-a-new-genesis-ledger-in-stand-alone-mode.html)を最初から作成する。
- ディスクから[既存のレジャーバージョンを読み込む](load-a-saved-ledger-in-stand-alone-mode.html)。


## See Also

- **チュートリアル:**
    - [`rippled`の構成](configure-rippled.html)
    - [スタンドアロンモードでレジャーを進める](use-stand-alone-mode.html)


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
