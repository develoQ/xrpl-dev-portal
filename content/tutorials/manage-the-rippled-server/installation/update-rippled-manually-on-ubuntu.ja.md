---
html: update-rippled-manually-on-ubuntu.html
parent: install-rippled.html
blurb: Ubuntu Linuxでrippledを手動更新します。
labels:
  - コアサーバー
  - セキュリティ
---

# UbuntuまたはDebianでの手動更新

このページでは、Ubuntu Linuxで最新リリースの`rippled`に手動で更新する手順を説明します。 以下の手順は、[`rippled`がすでにネイティブパッケージを使用してインストール](install-rippled-on-ubuntu.html)されていることを前提としています。 可能であれば手動更新ではなく[自動更新](update-rippled-automatically-on-linux.html)を設定することが推奨されます。

> **Caution:** Ripple renewed the GPG key used to sign binary packages shortly before the release of v1.7.0. If you are upgrading from a version earlier than 1.7.0, you must first download and manually trust the updated public key as follows:
> 
>     wget -q -O - "https://repos.ripple.com/repos/api/gpg/key/public" | \
>       sudo apt-key add -
>     
> 
> For more information, see the [`rippled` 1.7.0 release notes](https://xrpl.org/blog/2021/rippled-1.7.0.html#upgrading-special-action-required).

**ヒント:** これらの手順をすべて一度に実行するには、`rippled`パッケージに含まれている`/opt/ripple/bin/update-rippled.sh`スクリプトを実行します。 このスクリプトは`sudo`ユーザーとして実行する必要があります。

手動で更新するには、以下の手順を実行します。

1. リポジトリを更新します。
   
        $ sudo apt -y update

2. `rippled`パッケージをアップグレードします。
   
        $ sudo apt -y upgrade rippled

3. `systemd`ユニットファイルを再度読み込みます。
   
        $ sudo systemctl daemon-reload

4. `rippled`サービスを再起動します。
   
        $ sudo service rippled restart


## See Also

- **コンセプト:**
    - [`rippled`サーバー](xrpl-servers.html)
    - [Introduction to Consensus](intro-to-consensus.html)
- **チュートリアル:**
    - [`rippled` v1.3.xへの移行手順](rippled-1-3-migration-instructions.html) <!-- Note: remove when versions older than v1.3 are basically extinct -->
    - [rippledのトラブルシューティング](troubleshoot-the-rippled-server.html)
- **リファレンス:**
    - [rippled APIリファレンス](http-websocket-apis.html)
        - [`rippled`コマンドラインの使用](commandline-usage.html)
        - \[server_infoメソッド\]\[\]


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
