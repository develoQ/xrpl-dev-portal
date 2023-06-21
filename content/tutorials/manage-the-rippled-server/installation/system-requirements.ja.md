---
html: system-requirements.html
parent: install-rippled.html
blurb: rippledのハードウェアやソフトウェアのシステム要件
labels:
  - コアサーバー
---

# システム要件

## 推奨される仕様

For reliable performance in production environments, it is recommended to run an XRP Ledger (`rippled`) server on bare metal with the following characteristics or better:

- オペレーティングシステム: Ubuntu (LTF) 、CentOS または Red Hat Enterprise Linux (最新版)
- CPU: Intel Xeon 3GHz以上のプロセッサー、8コア以上、ハイパースレッディング有効
- Disk: SSD / NVMe (10,000 IOPS sustained - not burst or peak - or better). Minimum 50 GB for the database partition. Do not use Amazon Elastic Block Store (AWS EBS) because its latency is too high to sync reliably.
- RAM: 64GB
- ネットワーク: ホスト上にギガビットネットワークインターフェイスを備える企業データセンターネットワーク


## 最小仕様

テスト目的やたまにしか使わない場合は、一般的なハードウェア上でXRP Ledgerサーバーを稼働させることができます。 以下の最低要件を満たせば、ほとんどの場合は動作しますが、必ずしも[ネットワークと同期](server-doesnt-sync.html)しているとは限りません。

- オペレーティングシステム: macOS、Windows（64ビット）、またはほとんどのLinuxディストリビューション(Red Hat、 Ubuntu、 Debianをサポート)
- CPU: 64ビット x86_64、4コア以上
- ディスク: SSD / NVMe（10,000 IOPS以上） ディスク: データベースパーティション用に最低50GB。 Do not use Amazon Elastic Block Store (AWS EBS) because its latency is too high to sync reliably.
- RAM: 16GB以上

<!-- SPELLING_IGNORE: iops, ntp, x86_64, ec2, nvme -->

作業負荷によっては、Amazon EC2の`m3.large` VMサイズが適切な場合があります。 高速のネットワーク接続が望ましいです。 サーバーのクライアント処理負荷が増加すると、必要なリソースも増加します。

For a validator, consider `z1d.2xlarge` with an extra 1 TB disk for logging and core dump storage.


## システム時刻

`rippled`サーバーは、正確な時刻が維持されていることを前提としています。 `ntpd`や`chrony`などのデーモンで、ネットワークタイムプロトコル（NTP）を使用してシステムの時刻を同期することを推奨します。


## See Also

- **コンセプト:**
    - [`rippled`サーバー](xrpl-servers.html)
    - [Introduction to Consensus](intro-to-consensus.html)
- **チュートリアル:**
    - [容量の計画](capacity-planning.html) - 本番環境向けの推奨仕様および計画についての詳細情報
    - [`rippled`のインストール](install-rippled.html)
    - [rippledのトラブルシューティング](troubleshoot-the-rippled-server.html)
- **リファレンス:**
    - [rippled APIリファレンス](http-websocket-apis.html)
        - [`rippled`コマンドラインの使用](commandline-usage.html)
        - \[server_infoメソッド\]\[\]


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
