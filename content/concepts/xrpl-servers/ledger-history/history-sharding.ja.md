---
html: history-sharding.html
parent: ledger-history.html
blurb: 履歴シャーディングは、履歴レジャーデータを保持する任務をrippledサーバー間で分担するようにします。
labels:
  - データ保持
  - コアサーバー
---

# 履歴シャーディング

\[導入: rippled 0.90.0\]\[\]

稼働中のサーバーは、ネットワーク実行時に検知または取得したレジャーに関するデータを格納したデータベースを作成します。 Each server stores that ledger data in its _ledger store_, but [online deletion](online-deletion.html) removes old ledgers' data automatically over time. History sharding provides a separate storage system for older ledger history so that the network can divide up the work of recording the entire (multiple terabyte) history of the XRP Ledger.

履歴シャーディングは、XRP Ledgerのトランザクション履歴をシャードと呼ばれるセグメントに分割し、XRP Ledgerネットワークのサーバー全体に分散します。 シャードは、一連のレジャーです。 `rippled`サーバーは、レジャーストアーとシャードストアーの両方にレジャーを同じ方法で保存します。

[![XRP Ledgerネットワーク: レジャーストアーとシャードストアーの図](img/xrp-ledger-network-ledger-store-and-shard-store.png)](img/xrp-ledger-network-ledger-store-and-shard-store.png)

<!-- Diagram source: https://docs.google.com/presentation/d/1mg2jZQwgfLCIhOU8Mr5aOiYpIgbIgk3ymBoDb2hh7_s/edit#slide=id.g417450e8da_0_316 -->

## 履歴シャードの取得と共有

`rippled` サーバーは履歴シャードを取得して保存します（この動作には設定が必要です）。 このようなサーバーでは、ネットワークとの同期を実行し、設定された数の最新レジャーへのレジャー履歴の埋め戻しが完了した後で、シャードの取得が開始されます。 ネットワークアクティビティがあまり発生しないこの期間に、`shard_db`を維持するように設定されている`rippled`サーバー が、シャードストアーに追加するシャードをランダムに選択します。 ネットワークレジャー履歴が均等に分散される確率を高めるため、取得対象のシャードはランダムに選択され、現行シャードが特に優先されることはありません。

シャードが選択されたら、レジャー取得プロセスが開始されます。 最初にシャードの最後のレジャーのシーケンスが取得され、最初のシャードに向けて逆方向に処理が進められます。 取得プロセスでは最初に、サーバーがローカルでデータを確認します。 取得できないデータについては、サーバーはピア`rippled`サーバーにデータを要求します。 要求された期間のデータを供給できるサーバーは、履歴で応答します。 要求側サーバーはこれらの応答を結合し、シャードを作成します。 The shard is complete when it contains all the ledgers in a specific range.

The server selects and downloads additional shards until it reaches the maximum number of shards it is configured to store. `rippled`サーバーが1つのシャードを完全に取得する前に容量不足になった場合、空き容量ができて処理を続行できるようになるまで取得プロセスを停止します。

## XRP Ledgerネットワークデータの整合性

すべてのレジャーの履歴は、特定範囲の履歴レジャーを維持することに同意したサーバー間で共有されます。 これにより、各サーバーは維持することに同意したデータがすべてあることを確認し、プルーフツリーまたはレジャーデルタを作成できるようになります。 履歴シャーディングが設定されている`rippled`サーバーは、保存するシャードをランダムに選択するため、すべての閉鎖済みレジャーの履歴全体が正規分布曲線で保存され、XRP Ledgerネットワークで履歴が均一に維持される確率が高くなります。

History shards are recorded in a deterministic format, so that any two servers assembling the same shard produce the exact same binary data no matter what order they acquired the data and where they got it from. This makes it possible to compare checksums or cryptographic hashes of the shard data to verify the integrity of the data, and it is possible to share and import history shards through other formats. (For example, you could download shard data using Bittorrent or acquire physical media with the shard data pre-loaded on it, and verify that it matches the data that can be downloaded from the network.) \[New in: rippled 1.8.1\]\[\]


## See Also

- **Concepts:**
    - [Ledgers](ledgers.html)
    - [Introduction to Consensus](intro-to-consensus.html)
- **Tutorials:**
    - [Capacity Planning](capacity-planning.html)
    - [Configure `rippled`](configure-rippled.html)
        - [履歴シャーディングの設定](configure-history-sharding.html)
- **References:**
    - \[crawl_shardsメソッド\]\[\]
    - \[download_shardメソッド\]\[\]
    - [Peer Crawler](peer-crawler.html)


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
