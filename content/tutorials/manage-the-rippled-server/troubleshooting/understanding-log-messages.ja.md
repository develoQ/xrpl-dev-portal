---
html: understanding-log-messages.html
parent: troubleshoot-the-rippled-server.html
blurb: デバッグログの警告メッセージとエラーメッセージを解釈して対応します。
labels:
  - コアサーバー
---

# ログメッセージについて

以下のセクションでは、[`rippled`サーバー](xrpl-servers.html)のデバッグログに出力される最も一般的なログメッセージタイプとその解釈を説明します。

これは、`rippled`の[問題を診断する](diagnosing-problems.html)上で重要なステップです。

## Log Message Structure

The following shows the format of the log file:

```text
2020-Jul-08 20:10:17.372178946 UTC Peer:WRN [236] onReadMessage from n9J2CP7hZypxDJ27ZSxoy4VjbaSgsCNaRRJtJkNJM5KMdGaLdRy7 at 197.27.127.136:53046: stream truncated
2020-Jul-08 20:11:13.582438263 UTC PeerFinder:ERR Logic testing     52.196.126.86:13308 with error, Connection timed out
2020-Jul-08 20:11:57.728448343 UTC Peer:WRN [242] onReadMessage from n9J2CP7hZypxDJ27ZSxoy4VjbaSgsCNaRRJtJkNJM5KMdGaLdRy7 at 197.27.127.136:53366: stream truncated
2020-Jul-08 20:12:12.075081020 UTC LoadMonitor:WRN Job: sweep run: 1172ms wait: 0ms
```

Each line represents one log entry, with the following parts in order, separated by spaces:

1. The date the log entry was written, such as `2020-Jul-08`.
2. The time the log entry was written, such as `20:12:12.075081020`.
3. The time zone indicator `UTC`. (Log dates are always in UTC.) \[New in: rippled 1.5.0\]\[\]
4. The log partition and severity, such as `LoadMonitor:WRN`.
5. The log message, such as `Job: sweep run: 1172ms wait: 0ms`.

For simplicity, the examples in this page omit the date, time, and time zone indicator.


## クラッシュ

ログに記録されたランタイムエラーのメッセージは、サーバーがクラッシュしたことを示している可能性があります。 このようなメッセージは通常、以下の例に示すいずれかのメッセージで始まります。

```
Throw<std::runtime_error>
```

```
Terminating thread rippled: main: unhandled St13runtime_error
```

サーバーが起動時に常にクラッシュする場合は、[サーバーが起動しない](server-wont-start.html)で考えられる原因を参照してください。

サーバーが稼働中にランダムにクラッシュする場合、または特定のコマンドを実行するとクラッシュする場合は、`rippled`が最新バージョンに[更新](install-rippled.html)されていることを確認してください。 最新バージョンに更新済で、サーバーがクラッシュする場合は、以下の点を確認してください。

- サーバーのメモリーが不足していませんか。 一部のシステムでは、OOM（Out Of Memory）Killerやその他の監視プロセスによって`rippled`が終了されることがあります。
- サーバーが共有環境で稼働している場合、他のユーザーや管理者によってマシンまたはサービスが再起動されますか。 たとえば、一部のホステッドプロバイダーは、長期にわたって共有マシンのリソースを大量に消費するサービスを自動的に終了します。
- サーバーは`rippled`を実行するための[最小要件](system-requirements.html)を満たしていますか。 [本番環境サーバーに関する推奨事項](system-requirements.html#推奨される仕様)を適用していますか。

上記のいずれにも該当しない場合は、その問題をセキュリティ上重要なバグとしてRippleに報告してください。 Rippleでクラッシュを再現できる場合は、報奨を受領できる可能性があります。 詳細は<https://ripple.com/bug-bounty/>を参照してください。


## Already validated sequence at or past

以下のようなログメッセージが出力される場合は、サーバーが異なるレジャーインデックスの検証を順不同で受信しています。

```text
2018-Aug-28 22:55:58.316094260 Validations:WRN Val for 2137ACEFC0D137EFA1D84C2524A39032802E4B74F93C130A289CD87C9C565011 trusted/full from nHUeUNSn3zce2xQZWNghQvd9WRH6FWEnCBKYVJu2vAizMxnXegfJ signing key n9KcRZYHLU9rhGVwB9e4wEMYsxXvUfgFxtmX25pc1QPNgweqzQf5 already validated sequence at or past 12133663 src=1
```

この種類のメッセージが時折発生しても通常は問題ありません。 この種類のメッセージが同じ送信バリデータで頻繁に発生する場合は、以下のような問題がある可能性があります（可能性の高いものから順に示しています）。

- メッセージを書き込むサーバーにネットワークの問題がある。
- メッセージに表示されているバリデータにネットワークの問題がある。
- The validator described in the message is behaving maliciously.


## async_send failed

The following log message indicates that [StatsD export](configure-statsd.html) failed:

```text
Collector:ERR async_send failed: Connection refused
```

This could mean:

- Your StatsD configuration has the wrong IP address or port.
- The StatsD server you were attempting to export to was down or not accessible from your `rippled` server.

Check the `[insight]` stanza in your `rippled`'s config file and confirm that you have network connectivity from your `rippled` server to your StatsD server.

This error has no other impact on the `rippled` server, which should continue to work as normal except for the sending of StatsD metrics.


## Check for upgrade

The following message indicates that the server has detected that it is running an older software version than at least 60% of its trusted validators:

```text
LedgerMaster:ERR Check for upgrade: A majority of trusted validators are running a newer version.
```

This is not strictly a problem, but an old server version is likely to become [amendment blocked](amendments.html#amendment-blocked-servers). You should [update `rippled`](install-rippled.html) to the latest stable version. (If you are connected to [devnet](parallel-networks.html), update to the latest nightly version instead.)


## Connection reset by peer

以下のメッセージは、ピア`rippled`サーバーによって接続が閉じられたことを示します。

```text
2018-Aug-28 22:55:41.738765510 Peer:WRN [012] onReadMessage: Connection reset by peer
```

ピアツーピアネットワークで接続が時折失われることは、特に異常な動作ではありません。 **この種類のメッセージが時折発生しても問題ではありません。 **

このようなメッセージが同時に大量に出力される場合は、以下のような問題がある可能性があります。

- 1つ以上の特定のピアへのインターネット接続が切断されている。
- Your server may have been overloading the peer with requests, causing the peer to disconnect your server.


## Consumer entry dropped with balance at or above drop threshold

The following log message indicates that a client to the server's public API has been dropped as a result of [rate limiting](rate-limiting.html):

```text
Resource:WRN Consumer entry 169.55.164.21 dropped with balance 15970 at or above drop threshold 15000
```

The entry contains the IP address of the client that exceeded its rate limit, and the client's "balance", which is a score estimating the rate at which the client has been using the API. The threshold for dropping a client is [hardcoded to a score of 15000](https://github.com/ripple/rippled/blob/06c371544acc3b488b9d9c057cee4e51f6bef7a2/src/ripple/resource/impl/Tuning.h#L34-L35).

If you see frequent messages from the same IP address, you may want to block those IP addresses from your network to reduce the load on your server's public API. (For example, you may be able to configure your firewall to block those IP addresses.)

To avoid being dropped by rate limiting on your own server, [connect as an admin](get-started-using-http-websocket-apis.html#admin-access).

## InboundLedger 11 timeouts for ledger

```text
InboundLedger:WRN 11 timeouts for ledger 8265938
```

これは、サーバーがそのピアに対して特定のレジャーデータを要求する際に問題が発生していることを示しています。 [レジャーインデックス](basic-data-types.html#レジャーインデックス)が、\[server_infoメソッド\]\[\]により報告される最新の検証済みレジャーのインデックスよりもかなり小さい場合は、サーバーが[履歴シャード](history-sharding.html)のダウンロード中である可能性があります。

これは厳密には問題ではありませんが、レジャー履歴を迅速に取得したい場合は、`[ips_fixed]`構成スタンザを追加または編集してからサーバーを再起動することで、すべての履歴が記録されたピアに接続するように`rippled`を構成できます。 たとえば、すべての履歴が記録されたRippleのサーバーに常に接続するには、以下のようにします。

```
[ips_fixed]
s2.ripple.com 51235
```


## InboundLedger Want hash

以下のようなログメッセージは、サーバーが他のサーバーにレジャーデータを要求していることを示しています。

```text
InboundLedger:WRN Want: 5AE53B5E39E6388DBACD0959E5F5A0FCAF0E0DCBA45D9AB15120E8CDD21E019B
```

これは、サーバーの同期中、埋め戻し中、[履歴シャード](history-sharding.html)のダウンロード中は正常です。


## LoadMonitor:WRN Job

以下のようなメッセージは、機能の実行に時間がかかっている場合（この例では11秒以上）に出力されます。

```text
2018-Aug-28 22:56:36.180827973 LoadMonitor:WRN Job: gotFetchPack run: 11566ms wait: 0ms
```

以下のようなメッセージは、ジョブの実行待機に時間がかかっている場合（この例でも11秒以上）に出力されます。

```text
2018-Aug-28 22:56:36.180970431 LoadMonitor:WRN Job: processLedgerData run: 0ms wait: 11566ms
2018-Aug-28 22:56:36.181053831 LoadMonitor:WRN Job: AcquisitionDone run: 0ms wait: 11566ms
2018-Aug-28 22:56:36.181110594 LoadMonitor:WRN Job: processLedgerData run: 0ms wait: 11566ms
2018-Aug-28 22:56:36.181169931 LoadMonitor:WRN Job: AcquisitionDone run: 0ms wait: 11566ms
```

ジョブの実行に時間がかかり、そのジョブの完了を他のジョブが待っている場合に、この2種類のメッセージが同時に出力されることがよくあります。

サーバー起動後の**最初の数分間**にこの種類のメッセージがいくつか発生することは**特に異常な動作ではありません**。

サーバーの起動後5分以上にわたってこれらのメッセージが継続する場合、特に`run`時間が1000msを大きく上回る場合は、**サーバーに十分なリソース（ディスクI/O、RAM、CPUなど）がない**可能性があります。 この原因として、使用しているハードウェアの性能が不十分であること、または同じハードウェアで実行されている他のプロセスがリソースをめぐって`rippled`と競合していることが考えられます。 （`rippled`とリソースをめぐって競合する可能性のある他のプロセスの例としては、スケジュール済みバックアップ、ウィルススキャナー、定期的なデータベースクリーナーなどがあります。 ）

考えられるもう1つの原因として、回転型ハードディスクでNuDBの使用を試みていることが挙げられます。 NuDBはソリッドステートドライブ（SSD）でのみ使用してください。 `rippled`のデータベースには常にSSDストレージの使用が推奨されますが、RocksDBを使用する回転型ディスクで`rippled`を正常に稼働できる _可能性があります_ 。 回転型ディスクを使用している場合は、`[node_db]`と`[shard_db]`（使用している場合）の両方がRocksDBを使用するように設定されていることを確認してください。 例:

```
[node_db]
type=RocksDB
# ... more config omitted

[shard_db]
type=RocksDB
```


## No hash for fetch pack

以下のようなメッセージは、[履歴シャーディング](history-sharding.html)のために履歴レジャーをダウンロードする際に、`rippled` v1.1.0以前のバグが原因で発生します。

```text
2018-Aug-28 22:56:21.397076850 LedgerMaster:ERR No hash for fetch pack. Missing Index 7159808
```

これらは安全に無視できます。


## Not deleting

Messages such as the following occur when [online deletion is interrupted](online-deletion.html#interrupting-online-deletion):

```text
SHAMapStore:WRN Not deleting. state: syncing. age 25s
```

The `state` indicates the [server state](rippled-server-states.html). The `age` indicates how many seconds since the last validated ledger was closed. (A healthy age for the last validated ledger is 7 seconds or less.)

During startup, these messages are normal and can be safely ignored. At other times, messages like this usually indicate that the server does not meet the [system requirements](system-requirements.html), especially disk I/O, to run online deletion at the same time as everything else the server is doing.


## Potential Censorship

XRP Ledgerが取引検閲の可能性を検出すると、以下のようなログメッセージが出力されます。 ログメッセージと取引検閲検出機能の詳細は、[取引検閲の検知](transaction-censorship-detection.html)を参照してください。

**警告メッセージ**

```text
LedgerConsensus:WRN Potential Censorship: Eligible tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7, which we are tracking since ledger 18851530 has not been included as of ledger 18851545.
```

**エラーメッセージ**

```text
LedgerConsensus:ERR Potential Censorship: Eligible tx E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7, which we are tracking since ledger 18851530 has not been included as of ledger 18851605. Additional warnings suppressed.
```


## rotating validatedSeq

This message indicates that [online deletion](online-deletion.html) has started running:

```text
SHAMapStore:WRN rotating  validatedSeq 54635511 lastRotated 54635255 deleteInterval 256 canDelete_ 4294967295
```

This log message is normal and indicates that online deletion is operating as expected.

The log message contains values describing the current online deletion run. Each keyword corresponds to the value immediately following it:

| Keyword          | Value                | Description                                                                                                                                                                                  |
|:---------------- |:-------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `validatedSeq`   | \[Ledger Index\]\[\] | The current validated ledger version.                                                                                                                                                        |
| `lastRotated`    | \[Ledger Index\]\[\] | The end of the ledger range in the ["old" (read-only) database](online-deletion.html#how-it-works). Online deletion deletes this ledger version and earlier.                                 |
| `deleteInterval` | Number               | How many ledger versions to keep after online deletion. The [`online_delete` setting](online-deletion.html#configuration) controls this value.                                               |
| `canDelete_`     | \[Ledger Index\]\[\] | The newest ledger version that the server is allowed to delete, if using [advisory deletion](online-deletion.html#advisory-deletion). If not using advisory deletion, this value is ignored. |

When online deletion finishes, it writes the following log message:

```text
SHAMapStore:WRN finished rotation 54635511
```

The number at the end of the message is the \[ledger index\]\[\] of the validated ledger at the time online deletion started, matching the `validatedSeq` value of the "rotating" message. This becomes the `lastRotated` value the next time online deletion runs.

If the server falls out of sync while running online deletion, it interrupts online deletion and writes a ["Not deleting" log message](#not-deleting) instead of a "finished rotation" message.


## シャード: No such file or directory

`rippled` 1.3.1のバグが原因で、[履歴シャーディング](history-sharding.html)を有効にしたときに次のようなログメッセージが書き込まれることがあります。

```text
ShardStore:ERR shard 1804: No such file or directory
ShardStore:ERR shard 354: No such file or directory
ShardStore:ERR shard 408: No such file or directory
ShardStore:ERR shard 2927: No such file or directory
ShardStore:ERR shard 2731: No such file or directory
ShardStore:ERR shard 2236: No such file or directory
```

このエラーは、上記のバグのほかに、`rippled`が**起動後**に基となるファイルシステムに書き込めなくなった場合にも起こります。 考えられる原因は次のとおりです。

- ストレージメディアのハードウェア障害
- ファイルシステムがアンマウントされた
- シャードフォルダーが削除された

**ヒント:** 一般的に、サービスが停止している場合は、`rippled`のデータベースファイルを削除しても安全ですが、サーバーの稼働中には決してデータベースファイルを削除しないでください。


## Unable to determine hash of ancestor

以下のようなログメッセージは、サーバーがピアからの検証メッセージを認識するけれども、サーバーが基盤としている親レジャーバージョンを認識しない場合に発生します。 これは、サーバーがネットワークの他の部分と同期していない場合に発生することがあります。

```text
2018-Aug-28 22:56:22.256065549 Validations:WRN Unable to determine hash of ancestor seq=3 from ledger hash=00B1E512EF558F2FD9A0A6C263B3D922297F26A55AEB56A009341A22895B516E seq=12133675
```

{% include '_snippets/unsynced_warning_logs.ja.md' %}
<!--_ -->


## [veto_amendments] section in config file ignored
<!-- SPELLING_IGNORE: veto_amendments -->

Log messages such as the following occur when  your `rippled.cfg` file contains a legacy `[veto_amendments]` stanza. The first time the server starts on version 1.7.0 or higher, it reads the stanza to set amendment votes; on later restarts, it ignores the `[amendments]` and `[veto_amendments]` stanzas and prints this message instead.

```text
Amendments:WRN [veto_amendments] section in config file ignored in favor of data in db/wallet.db.
```

To resolve this error, remove the `[amendments]` and `[veto_amendments]` stanzas from your config file. For more information, see [Amendment Voting](amendments.html#amendment-voting).


## View of consensus changed during open

以下のようなログメッセージが出力される場合は、サーバーがネットワークの他の部分と同期していません。

```text
2018-Aug-28 22:56:22.368460130 LedgerConsensus:WRN View of consensus changed during open status=open,  mode=proposing
2018-Aug-28 22:56:22.368468202 LedgerConsensus:WRN 96A8DF9ECF5E9D087BAE9DDDE38C197D3C1C6FB842C7BB770F8929E56CC71661 to 00B1E512EF558F2FD9A0A6C263B3D922297F26A55AEB56A009341A22895B516E
2018-Aug-28 22:56:22.368499966 LedgerConsensus:WRN {"accepted":true,"account_hash":"89A821400087101F1BF2D2B912C6A9F2788CC715590E8FA5710F2D10BF5E3C03","close_flags":0,"close_time":588812130,"close_time_human":"2018-Aug-28 22:55:30.000000000","close_time_resolution":30,"closed":true,"hash":"96A8DF9ECF5E9D087BAE9DDDE38C197D3C1C6FB842C7BB770F8929E56CC71661","ledger_hash":"96A8DF9ECF5E9D087BAE9DDDE38C197D3C1C6FB842C7BB770F8929E56CC71661","ledger_index":"3","parent_close_time":588812070,"parent_hash":"5F5CB224644F080BC8E1CC10E126D62E9D7F9BE1C64AD0565881E99E3F64688A","seqNum":"3","totalCoins":"100000000000000000","total_coins":"100000000000000000","transaction_hash":"0000000000000000000000000000000000000000000000000000000000000000"}
```

{% include '_snippets/unsynced_warning_logs.ja.md' %}
<!--_ -->


## We are not running on the consensus ledger

```text
NetworkOPs:WRN We are not running on the consensus ledger
```

{% include '_snippets/unsynced_warning_logs.ja.md' %}
<!--_ -->


## See Also

- **コンセプト:**
    - [`rippled`サーバー](xrpl-servers.html)
    - [技術に関するよくある質問](technical-faq.html)
- **チュートリアル:**
    - [問題の診断](diagnosing-problems.html)
    - [容量の計画](capacity-planning.html)
- **リファレンス:**
    - [rippled APIリファレンス](http-websocket-apis.html)
        - [`rippled`コマンドラインの使用](commandline-usage.html)
        - \[server_infoメソッド\]\[\]

<!-- SPELLING_IGNORE: oom, async_send, statsd, inboundledger, loadmonitor, validatedseq -->

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
