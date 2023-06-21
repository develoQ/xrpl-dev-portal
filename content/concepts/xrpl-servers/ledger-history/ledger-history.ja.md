---
html: ledger-history.html
parent: xrpl-servers.html
blurb: rippledサーバーはトランザクションの変動金額と状態の履歴をローカルに保管します。
labels:
  - データ保持
  - Blockchain
  - コアサーバー
---

# レジャー履歴

[コンセンサスプロセス](intro-to-consensus.html)により、[検証済みレジャーバージョン](ledgers.html)のチェーンが作成されます。 各バージョンは、以前のバージョンに[トランザクション](transaction-basics.html)のセットを適用して生成されます。 各[`rippled`サーバー](xrpl-servers.html)には、レジャーバージョンとトランザクション履歴がローカルに保管されます。 サーバーに保管されるトランザクション履歴の量は、サーバーがオンラインであった期間と、サーバーが取得し、保持する履歴量の設定に応じて異なります。

ピアツーピアのXRP Ledgerネットワーク内のサーバーは、コンセンサスプロセスの一環としてトランザクションやその他のデータを相互に共有します。 各サーバーは個別に新しいレジャーバージョンを作成し、その結果を信頼できるバリデータと比較して、整合性を維持します。 （信頼できるバリデータのコンセンサスがサーバーの結果と一致しない場合は、サーバーがピアから必要なデータを取得して整合性を維持します。 ）サーバーはピアから古いデータをダウンロードして、利用可能な履歴のギャップを埋めることができます。 レジャーはデータの暗号[ハッシュ](basic-data-types.html#ハッシュ)を使用した構造となっているため、すべてのサーバーがデータの整合性の検証を行えます。

## データベース

サーバーはレジャーの状態データとトランザクションを _レジャーストアー_ と呼ばれるkey-valueストアで保持します。 また、`rippled`にはいくつかのSQLiteデータベースファイルが維持されているので、トランザクション履歴などへより柔軟にアクセスし、特定の設定変更を追跡できます。

一般に、`rippled`サーバーが稼働していないときにはそのサーバーのすべてのデータベースファイルを安全に削除できます。 （たとえばサーバーのストレージ設定を変更する場合や、Test Netから本番環境ネットワークに切り替える場合に、このような削除が必要となることがあります。 ）

## 利用可能な履歴

設計上、XRP Ledgerのすべてのデータとトランザクションは公開されており、誰でもすべてのデータを検索または照会できます。 ただし、サーバーが検索できるデータは、そのサーバーがローカルで使用できるデータに限られます。 サーバーで利用できないレジャーバージョンやトランザクションを照会しようとすると、そのデータが見つからないという応答がサーバーから返されます。 必要な履歴を保持している他のサーバーに対して同じ照会を実行すると、正常な応答が返されます。 XRP Ledgerデータを使用する企業では、サーバーで利用可能な履歴の量に注意してください。

\[server_infoメソッド\]\[\]は、サーバーで利用可能なレジャーバージョンの数を`complete_ledgers`フィールドで報告します。

## 履歴の取得

`rippled`サーバーは起動されると、最優先で最新の検証済みレジャーの完全なコピーを取得します。 From there, it keeps up with advances in the ledger progress. サーバーは同期される前から履歴の埋め戻しを行い、同期後にも収集した履歴のギャップを埋めることができます。 （レジャー履歴のギャップは、サーバーの使用率が一時的に高くなりネットワークと同期をとることができない場合、ネットワークとの接続が失われた場合、またはその他の一時的な問題の影響を受けた場合に発生する可能性があります。 When downloading ledger history, the server requests the missing data from its [peer servers](peer-protocol.html), and verifies the data's integrity using cryptographic \[hashes\]\[Hash\].

履歴の埋め戻しは、サーバーでは最も優先度の低い処理の1つであるため、サーバーの使用率が高い場合やハードウェアとネットワークのスペックが十分ではない場合には、欠落している履歴を埋めるのに時間がかかることがあります。 推奨されるハードウェアスペックについては、[容量の計画](capacity-planning.html)を参照してください。 履歴の埋め戻しでは、サーバーのダイレクトピアの1つ以上に当該の履歴が保持されている必要もあります。 For more information on managing your server's peer-to-peer connections, see [Configure Peering](configure-peering.html).

XRP Ledgerは、コンテンツの一意のハッシュを使用して（さまざまなレベルの）データを識別します。 XRP Ledgerの状態データには、レジャーの履歴の概要が[LedgerHashesオブジェクトタイプ](ledgerhashes.html)の形式で含まれています。 サーバーはLedgerHashesオブジェクトを使用して取得するレジャーバージョンを認識し、受信するレジャーデータが正しく完全であることを確認します。

<a id="with-advisory-deletion"></a><!-- old anchor to this area -->
### Backfilling
\[Updated in: rippled 1.6.0\]\[\]

The amount of history a server attempts to download depends on its configuration. The server automatically tries to fill gaps by downloading history up to **the oldest ledger it already has available**. サーバーが埋め戻す量は、`[ledger_history]`設定で定義されます。 However, the server never downloads ledgers that would be scheduled for [deletion](online-deletion.html).

The `[ledger_history]` setting defines a minimum number of ledgers to accumulate from before the current validated ledger. Use the special value `full` to download the [full history](#full-history) of the network. If you specify a number of ledgers, it must be equal to or more than the `online_deletion` setting; you cannot use `[ledger_history]` to make the server download _less_ history. To reduce the amount of history a server stores, change the [online deletion](online-deletion.html) settings instead. <!-- STYLE_OVERRIDE: a number of -->



## すべての履歴

XRP Ledgerネットワーク内の一部のサーバーは、「すべての履歴が記録される」サーバーとして設定されています。 これらのサーバーは、使用可能なすべてのXRP Ledgerの履歴を収集しますが、**オンライン削除は使用しません**。 このため他の追跡サーバーよりもかなり多くのディスク容量が必要です。

The XRP Ledger Foundation provides access to a set of full history servers operated by community members (see [xrplcluster.com](https://xrplcluster.com) for more details). Rippleは、`s2.ripple.com`ですべての履歴が記録される一連の公開サーバーを公開サービスとして提供しています。 <!-- SPELLING_IGNORE: xrplcluster -->

Providers of Full History servers reserve the right to block access that is found to abuse resources, or put inordinate load on the systems.

**ヒント:** 一部の暗号資産ネットワークとは異なり、XRP Ledgerのサーバーは、現在の状態を認識して最新のトランザクションを把握するのにすべての履歴を必要としません。

詳細は、[履歴シャーディングの設定](configure-history-sharding.html)を参照してください。

## 履歴シャーディング

XRP Ledgerのすべての履歴を1台の高価なマシンに保管する代わりに、複数のサーバーがレジャー履歴の一部分を保管するように構成できます。 これは[履歴シャーディング](history-sharding.html)機能によって実現します。 一定範囲のレジャー履歴が _シャードストアー_ という個別の保管領域に保管されます。 ピアサーバーから（上記の[履歴の取得](#履歴の取得)で説明したとおり）特定のデータが要求されると、サーバーはレジャーストアーまたはシャードストアーのデータを使用して応答できます。

オンライン削除ではシャードストアーのデータは削除**されません**。 ただし、32768個以上のレジャーバージョンをサーバーのレジャーストアーに保持するようにオンライン削除が設定されていれば、レジャーストアーからデータが自動的に削除される前に、サーバーはレジャーストアーからシャードストアーにすべてのシャードをコピーできます。

ブロックチェーン


## See Also

- **Concepts:**
    - [Ledgers](ledgers.html)
    - [Introduction to Consensus](intro-to-consensus.html)
- **Tutorials:**
    - [Configure `rippled`](configure-rippled.html)
        - [Configure Online Deletion](configure-online-deletion.html)
        - [Configure Advisory Deletion](configure-advisory-deletion.html)
        - [Configure History Sharding](configure-history-sharding.html)
        - [Configure Full History](configure-full-history.html)
- **References:**
    - \[ledger method\]\[\]
    - \[server_info method\]\[\]
    - \[ledger_request method\]\[\]
    - \[can_delete method\]\[\]
    - \[ledger_cleaner method\]\[\]

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
