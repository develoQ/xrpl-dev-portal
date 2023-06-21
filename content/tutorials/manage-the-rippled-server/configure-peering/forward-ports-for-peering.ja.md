---
html: forward-ports-for-peering.html
parent: configure-peering.html
blurb: 受信ピアがrippledサーバーに接続できるようにファイアウォールを設定します。
labels:
  - コアサーバー
---

# ピアリングのポート転送

XRP Ledgerのピアツーピアネットワーク内にあるサーバーは、[ピアプロトコル](peer-protocol.html)を介して通信します。 セキュリティとネットワークの他の部分との接続を両立させるために、ファイアウォールを使用して、サーバーをほとんどのポートから保護し、ピアプロトコルポートだけを開放するか転送するようにする必要があります。

`rippled`サーバーの稼動中に、\[server_infoメソッド\]\[\]を実行すると、いくつのピアがあるか確認することができます。 `info`オブジェクトの`peers`フィールドは、サーバーに現在接続しているピアの数を示します。 この数が10または11の場合、通常はファイアウォールが着信接続をブロックしていることを示します。

ファイアウォールが着信ピア接続をブロックしていると思われるためピアが10個しかないことを示している`server_info`の結果の例（一部省略）は次のとおりです。

```json
$ ./rippled server_info
Loading: "/etc/opt/ripple/rippled.cfg"
2019-Dec-23 22:15:09.343961928 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "info" : {
         ...（省略）...
         "load_factor" : 1,
         "peer_disconnects" : "0",
         "peer_disconnects_resources" : "0",
         "peers" : 10,
         "pubkey_node" : "n9KUjqxCr5FKThSNXdzb7oqN8rYwScB2dUnNqxQxbEA17JkaWy5x",
         "pubkey_validator" : "n9KM73uq5BM3Fc6cxG3k5TruvbLc8Ffq17JZBmWC4uP4csL4rFST",
         "published_ledger" : "none",
         "server_state" : "connected",
         ...（省略）...
      },
      "status" : "success"
   }
}
```

To allow incoming connections, configure your firewall to allow incoming traffic on the peer protocol port, which is served on **port 51235** in the default config file. ポートの転送の手順はファイアウォールによって異なります。 If your server is behind a router that performs Network Address Translation (NAT), you must configure your router to forward the port to your server.

例えば、Red Hat Enterprise Linuxで`firewalld`ソフトウェアファイアウォールを使用している場合は、[`firewall-cmd`ツールを使用](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-port_forwarding)して、TCPトラフィックを次のように転送します。

_Assuming `--zone=public` is your public [zone](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-working_with_zones#sec-Listing_Zones)._

```sh
$ sudo firewall-cmd --add-forward-port=port=51235:proto=tcp:toport=51235
```

Then, restart the `rippled` server:

```sh
$ sudo systemctl restart rippled.service
```

To make it permanent:

```sh
$ sudo firewall-cmd --zone=public --permanent --add-port=51235/tcp
```

その他のソフトウェアファイアウォールとハードウェアファイアウォールについては、メーカー公式のドキュメントを参照してください。

If you are using a hosting service with a virtual firewall (for example, [AWS Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)), you do not need to use `firewalld`, but you still need to allow inbound traffic from the open internet on the peer port. Make sure you apply the relevant rules to your host or virtual machine.


## See Also

- **コンセプト:**
    - [ピアプロトコル](peer-protocol.html)
    - [`rippled`サーバー](xrpl-servers.html)
- **チュートリアル:**
    - [容量の計画](capacity-planning.html)
    - [`rippled`サーバーのトラブルシューティング](troubleshoot-the-rippled-server.html)
- **リファレンス:**
    - \[connectメソッド\]\[\]
    - \[peersメソッド\]\[\]
    - \[printメソッド\]\[\]
    - \[server_infoメソッド\]\[\]

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
