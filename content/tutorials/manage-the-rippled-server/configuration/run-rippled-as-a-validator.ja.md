---
html: run-rippled-as-a-validator.html
parent: configure-rippled.html
blurb: サーバーがコンセンサスレジャーで投票できるようにします。
labels:
  - コアサーバー
  - Blockchain
top_nav_grouping: 人気ページ
top_nav_name: UNLに参加しよう
---

# Run rippled as a Validator

[バリデータモード](rippled-server-modes.html)で実行されている[`rippled`サーバー](xrpl-servers.html)は、ストックサーバーが実行するあらゆる処理を実行します。

- [ピアのネットワーク](consensus-network.html)への接続

- 暗号署名された[トランザクション](transaction-basics.html)の中継

- 完全な共有グローバル[レジャー](ledgers.html)のローカルコピーの維持

バリデータが _特異である_ のは、検証メッセージも発行するという点です。 これらのメッセージは、[コンセンサスプロセス](consensus-principles-and-rules.html#コンセンサスの仕組み)の進行中、XRP Ledgerネットワークによる評価の対象となる候補のトランザクションです。

ただし、単に検証メッセージを発行するだけで、バリデータにコンセンサスプロセスでの発言権が自動的に付与されるわけではありません。 他のサーバーがバリデータ（モードのサーバー）を彼らのユニークノードリスト（UNL）に追加しない限り、彼らは（バリデータモードのサーバーからの）検証メッセージを無視します。 バリデータがUNLに含まれている場合、 _信頼できる_ バリデータであり、その提案は、信頼する側のサーバーによってコンセンサスプロセスで検討されます。

バリデータが _信頼できる_ バリデータではない場合も、ネットワークの全体的な健全性に関して、重要な役割を果たすことに変わりはありません。 これらのバリデータは、信頼できるバリデータを評価するための基準の確立を支援します。 例えば、信頼できるバリデータが、UNLに含まれていない多数のバリデータに対して異議を唱えている場合、問題があることを示しているおそれがあります。

**Warning:** Validators should not be accessible to the public. Do not allow public WebSocket access to your validator server or any other form of public access.



## 1. 優れたバリデータの特徴の理解

Strive to have your validator embody the following properties. Being a good validator helps `rippled` server operators and validator list publishers (such as https://vl.ripple.com and https://vl.xrplf.org) trust your validator before adding it to their UNLs.

- **可用性**

    優れたバリデータは、常に稼働し、提案されるあらゆるレジャーについて検証投票を送信します。 100%のアップタイムを実現するよう努めてください。

- **合意**

    優れたバリデータの投票は、可能な限り高い頻度で、コンセンサスプロセスの結果と合致します。 これに該当しない場合は、バリデータのソフトウェアが最新のものではないか、不具合があるか、意図的な偏りがあることを示唆している可能性があります。 常に[最新の`rippled`リリース](https://github.com/ripple/rippled/tree/master)を、修正を加えることなく実行します。 新規リリースについて知るために、[`rippled`のリリースを確認](https://github.com/ripple/rippled/releases)してください。

- **適時の投票**

    A good validator's votes arrive quickly and not after a consensus round has already finished. 適時の投票を維持するには、バリデータが推奨される[システム要件](system-requirements.html)を満たしていることを確認してください。 これには、高速のインターネット接続が含まれます。

    It is possible to submit new transactions and query data using a validator, but heavy loads of API queries may make the validator less reliable at keeping up with consensus. If your API needs are light enough, then you can use a server for both purposes. Ideally, a validator should be dedicated to participating in consensus.

- **Identified**

    優れたバリデータには、身元が明確な所有者が存在します。 [ドメイン検証](#6-ドメイン検証の提供)を提供することは、その第一歩になります。 XRP LedgerネットワークのUNLに、多くの法的な管轄域および地域のさまざまな所有者によって運営されているバリデータが含まれていると理想的です。 結果として、信頼できるバリデータの公正な運用が地域特有の事象によって損なわれるおそれが低減されます。 <!-- STYLE_OVERRIDE: clearly -->

Ripple社は、推奨される一連のバリデータを記載した[バリデータリスト](https://github.com/ripple/rippled/blob/develop/cfg/validators-example.txt)を公開しています。



## 2. `rippled`サーバーのインストール

詳細は、[`rippled`のインストール](install-rippled.html)を参照してください。



## 3. `rippled`サーバーで検証を有効化

`rippled`サーバーで検証を有効にすることは、サーバーの`rippled.cfg`ファイルにあるバリデータトークンを提供することを意味します。 バリデータキーとトークンを安全に生成して管理するために、`validator-keys`ツール（`rippled` RPMに含まれる）を使用することをお勧めします。

バリデータ（サーバー）**以外の**場所で、以下の手順に従います。

1. `validator-keys`ツールを`rippled` RPMを通じてまだインストールしていない場合は、手動でビルドして実行します。

    `validator-keys`ツールを手動でビルドして実行する方法については、[validator-keys-tool](https://github.com/ripple/validator-keys-tool)を参照してください。

2. `create_keys`コマンドを使用して、バリデータキーペアを生成します。
   
        $ validator-keys create_keys

      Ubuntuでの出力の例:
   
        Validator keys stored in /home/my-user/.ripple/validator-keys.json
       
        This file should be stored securely and not shared.

      macOSでの出力の例:
   
        Validator keys stored in /Users/my-user/.ripple/validator-keys.json
       
        This file should be stored securely and not shared.

      **警告:** 生成した`validator-keys.json`キーファイルは、暗号化されたUSBフラッシュドライブなど、安全かつ回復可能なオフラインの場所に保管してください。 特に、キーの使用場所となるバリデータにキーファイルを保存しないようにします。 バリデータの`secret_key`が悪用された場合は、ただちに[キーを破棄](https://github.com/ripple/validator-keys-tool/blob/master/doc/validator-keys-tool-guide.md#key-revocation)します。 Do not modify the contents of `validator-keys.json`, except to update the backup after generating a new token. If you generate more than one token from the same backup without updating, the network ignores the later tokens because they use the same `token_sequence` number.

      `validator-keys`ツールおよびツールで生成されるキーペアの詳細は、[Validator Keys Tool Guide](https://github.com/ripple/validator-keys-tool/blob/master/doc/validator-keys-tool-guide.md)を参照してください。

3. `create_token`コマンドを使用して、バリデータトークンを生成します。
   
        $ validator-keys create_token --keyfile /PATH/TO/YOUR/validator-keys.json

    出力の例:
   
        Update rippled.cfg file with these values:
       
       # validator public key: nHUtNnLVx7odrz5dnfb2xpIgbEeJPbzJWfdicSkGyVw1eE5GpjQr
       
        [validator_token]
        eyJ2YWxpZGF0aW9uX3NlY3J|dF9rZXkiOiI5ZWQ0NWY4NjYyNDFjYzE4YTI3NDdiNT
        QzODdjMDYyNTkwNzk3MmY0ZTcxOTAyMzFmYWE5Mzc0NTdmYT|kYWY2IiwibWFuaWZl
        c3QiOiJKQUFBQUFGeEllMUZ0d21pbXZHdEgyaUNjTUpxQzlnVkZLaWxHZncxL3ZDeE
        hYWExwbGMyR25NaEFrRTFhZ3FYeEJ3RHdEYklENk9NU1l1TTBGREFscEFnTms4U0tG
        bjdNTzJmZGtjd1JRSWhBT25ndTlzQUtxWFlvdUorbDJWMFcrc0FPa1ZCK1pSUzZQU2
        hsSkFmVXNYZkFpQnNWSkdlc2FhZE9KYy9hQVpva1MxdnltR21WcmxIUEtXWDNZeXd1
        NmluOEhBU1FLUHVnQkQ2N2tNYVJGR3ZtcEFUSGxHS0pkdkRGbFdQWXk1QXFEZWRGdj
        VUSmEydzBpMjFlcTNNWXl3TFZKWm5GT3I3QzBrdzJBaVR6U0NqSXpkaXRROD0ifQ==

On your validator:

1. `[validator_token]`とその値を、バリデータの`rippled.cfg`ファイルに追加します。

    以前に、`validator-keys`ツールを使用せずにバリデータを設定している場合は、`[validation_seed]`とその値を`rippled.cfg`ファイルから削除します。 これにより、バリデータの公開鍵が変更されます。

2. `rippled`を再起動します。
   
        $ sudo systemctl restart rippled.service

3. `server_info`コマンドを使用してバリデータの情報を取得し、バリデータとして実行されていることを確認します。
   
        $ rippled server_info

      - 応答に含まれている`pubkey_validator`の値は、バリデータで使用するために生成した`validator-keys.json`ファイルの`public_key`と一致している必要があります。

      - `server_state`の値は、 _**proposing**_ にする必要があります。

**セキュリティのヒント:** `rippled.cfg`ファイルに対する権限をより制限的なものに変更します。 Linuxでは、`0600`にすることを推奨します。 `chmod 0600 rippled.cfg`を使用して変更できます。

## 4. ネットワークへの接続

このセクションでは、バリデータをXRP Ledgerネットワークに接続するために使用できる3種類の構成について説明します。 ユースケースに最適な構成を使用してください。

- [検出されたピア](#検出されたピアを使用した接続): ピアツーピアネットワーク内の任意のサーバーに接続します。

- [プロキシ](#プロキシを使用した接続): ストック`rippled`サーバーを、バリデータとピアツーピアネットワークの他の部分との間のプロキシとして実行します。

- [公開ハブ](#公開ハブを使用した接続): 評価の高い特定の公開サーバーにのみ接続します。

これらのアプローチの違いについては、[ピア接続設定のメリットとデメリット](peer-protocol.html#ピア接続設定のメリットとデメリット)を参照してください。


### 検出されたピアを使用した接続

この構成では、[検出されたピア](peer-protocol.html#ピアの検出)を使用してバリデータをXRP Ledgerネットワークに接続します。 これは`rippled`サーバーのデフォルトの動作です。

_**検出されたピアを使用してバリデータをXRP Ledgerネットワークに接続するには、**_ バリデータの`rippled.cfg`ファイルで`[peer_private]`スタンザを省略するか、それを`0`に設定します。 この構成の[サンプルのrippled.cfgファイル](https://github.com/ripple/rippled/blob/develop/cfg/rippled-example.cfg)が提供されています。


### プロキシを使用した接続

この構成は、自社で運用するストック`rippled`サーバーを通じてバリデータをネットワークに接続します。 これらのプロキシサーバーは、バリデータと発着信ネットワークトラフィックの間に設置します。

_**プロキシを使用してバリデータをXRP Ledgerネットワークに接続するには、次の手順を実行します。**_

1. ストック`rippled`サーバーを設置します。 詳細は、[rippledのインストール](install-rippled.html)を参照してください。

2. バリデータとストック`rippled`サーバーを設定して、[クラスター](cluster-rippled-servers.html)内で実行します。

3. バリデータの`rippled.cfg`ファイルで、`[peer_private]`を`1`に設定します。 そうすることで、バリデータのIPアドレスが転送されないようにします。 詳細は、[プライベートピア](peer-protocol.html#プライベートピア)を参照してください。 また、これによりクラスター内でバリデータを実行するよう`[ips_fixed]`スタンザで定義したサーバー以外のサーバーに、バリデータが接続しないようになります。

    **警告:** バリデータのIPアドレスを、その他の方法で公開していないことを確認してください。

4. 以下のトラフィックのみを許可するように、バリデータのホストマシンのファイアウォールを構成します。

    - 着信トラフィック: 構成したクラスター内にあるストック`rippled`サーバーのIPアドレスが発信元である場合のみ

    - 発信トラフィック: 構成したクラスター内にあるストック`rippled`サーバーのIPアドレスおよびポート443経由の<https://vl.ripple.com>が送信先である場合のみ

5. `rippled`を再起動します。
   
        $ sudo systemctl restart rippled.service

6. いずれかのストック`rippled`サーバーにある[ピアクローラー](peer-crawler.html)エンドポイントを使用します。 応答には、バリデータが含まれていないはずです。 これにより、バリデータの`[peer_private]`構成が機能していることが確認されます。 バリデータの`[peer_private]`を有効にした場合の効果の1つは、バリデータのピアによって、ピアクローラーの結果にバリデータが含まれないことです。
   
        $ curl --insecure https://STOCK_SERVER_IP_ADDRESS_HERE:51235/crawl | python3 -m json.tool

<!-- { TODO: Future: add a recommended network architecture diagram to represent the proxy, clustering, and firewall setup: https://ripplelabs.atlassian.net/browse/DOC-2046 }-->


### 公開ハブを使用した接続

この構成では、2つの[公開ハブ](rippled-server-modes.html#公開ハブ)を使用してバリデータをネットワークに接続します。 この構成は、[自社で運用しているプロキシを使用した接続](#プロキシを使用した接続)と似ていますが、公開ハブを通じて接続します。

_**公開ハブを使用してバリデータをネットワークに接続するには、次の手順を実行します。**_

1. バリデータの`rippled.cfg`ファイルに、次の`[ips_fixed]`スタンザを含めます。 2つの値`r.ripple.com 51235`と`zaphod.alloy.ee 51235`がデフォルトの公開ハブです。 このスタンザは、これらの公開ハブとのピア接続を常に維持するよう`rippled`に指示します。
   
        [ips_fixed]
        r.ripple.com 51235
        zaphod.alloy.ee 51235

    **注意:** この構成では、デフォルトの公開ハブを使用してバリデータをネットワークに接続します。 これらは _デフォルト_ の公開ハブであるため、ビジー状態になってバリデータにネットワークへの接続を提供できない場合があります。 この問題を避けるために、接続する公開ハブの数を増やすか、デフォルトでない公開ハブに接続するようにします。

    他の`rippled`サーバーのIPアドレスをここに記述することもできますが、それらのサーバーに対して以下の事項を期待できる場合に _**限ります**_ 。

      - メッセージを検閲することなく中継する。
      - オンライン状態を常に維持する。
      - Not DDoS you.
      - サーバーをクラッシュさせようとしない。
      - 未知の相手にバリデータのIPアドレスを公開しない。

2. また、バリデータの`rippled.cfg`ファイルに、次の`[peer_private]`スタンザを含めて、それを`1`に設定します。 それにより、バリデータのピアに対して、バリデータのIPアドレスをブロードキャストしないよう指示することになります。 また、バリデータに対して、`[ips_fixed]`スタンザで設定されているピアにのみ接続するよう指示することになります。 これにより、既知の信頼できるピア`rippled`サーバーに対してのみ、バリデータが接続を確立し、IPアドレスを共有することが保証されます。

        [peer_private] 1

    **警告:** バリデータのIPアドレスを、その他の方法で公開していないことを確認してください。

    `[peer_private]`が有効になっている場合、`rippled`は、`[ips]`スタンザで指定されている接続をすべて無視します。 現在`[ips]`スタンザにあるIPアドレスに接続する必要がある場合は、代わりにそれを`[ips_fixed]`スタンザに記述します。 ただし、それらのIPアドレスに対して、ステップ1で説明した確実な挙動を期待できる場合に _**限ります**_ 。

3. `rippled`を再起動します。

        $ sudo systemctl restart rippled.service



## 5. ネットワーク接続の確認

ここでは、バリデータがXRP Ledgerネットワークへの健全な接続を保持していることを検証する方法をいくつか紹介します。

- [`peers`](peers.html)コマンドを使用して、バリデータに接続しているすべての`rippled`サーバーのリストを取得します。 `peers`の配列が`null`である場合、ネットワークへの健全な接続が存在していません。 このドキュメントの手順に従ってバリデータを設置した場合、`peers`の配列には、`[ips_fixed]`スタンザで定義されているピアの数と同数のオブジェクトが含まれています。

    公開ハブを`[ips_fixed]`スタンザに記述した場合、そのハブがビジーになっているときは、バリデータの接続が拒否されることがあります。 この場合、接続の数は、`[ips_fixed]`スタンザで設定した数よりも最終的に少なくなることがあります。 初めて拒否された場合、バリデータは接続を再試行します。

    ネットワークへの安全かつ信頼できる接続を維持することが困難であり、公開ハブまたはプロキシを使用して接続を設定していない場合、[4. ネットワークへの接続](#4-ネットワークへの接続)を参照してください。 このセクションで説明されているいずれかの方法は、バリデータがネットワークへの健全な接続を維持する上で有用となる可能性があります。

- [`server_info`](server_info.html)コマンドを使用して、バリデータに関するいくつかの基本情報を取得します。 `server_state`は、`proposing`に設定されているはずです。 `full`または`validating`に設定されている場合もありますが、`proposing`に移行するまでの数分間に限られます。

    `server_state`が`proposing`に設定されている時間が大部分を占めていない場合、XRP Ledgerネットワークにバリデータが完全に参加できていないことを示している可能性があります。 サーバーの状態および`server_info`エンドポイントを使用してバリデータの問題を診断する方法の詳細は、[`rippled`サーバーの状態](rippled-server-states.html)および[`server_info`の取得](diagnosing-problems.html#server_infoの取得)を参照してください。

- [`validators`](validators.html)コマンドを使用して、バリデータによって使用される、公開済みかつ信頼できるバリデータの最新リストを取得します。 `validator_list_expires`の値が、`never`（無期限）、期限が切れていない、または期限切れ間近のいずれかであることを確認してください。



## 6. ドメイン検証の提供

検証リスト発行者およびXRP Ledgerネットワーク内のその他の参加者がバリデータの運用元を把握しやすいようにするには、バリデータのドメイン検証を提供します。 ドメイン検証とは、ハイレベルでは双方向リンクを指します。

- ドメインを使用して、バリデータキーの所有権を主張します。

- バリデータキーを使用して、ドメインの所有権を主張します。

このリンクを作成すると、バリデータキーとドメインの両方を所有しているという確固たる証拠が確立されます。 この証拠を提供することは、[優れたバリデータであること](#1-優れたバリデータの特徴の理解)の側面の1つにすぎません。

ドメイン検証を提供するには、以下の手順に従います。

1. バリデータと公に関連付ける、所有しているドメインの名前を選択します。 ）DDoS攻撃への対策として、ドメイン名によってバリデータのIPアドレスが解決されないようにする必要があります。

2. Serve an [`xrp-ledger.toml`](xrp-ledger-toml.html) file at your domain, and complete the [domain verification](xrp-ledger-toml.html#domain-verification) steps. Once you have completed these steps, your validator should be visible to the livenet [explorer](https://livenet.xrpl.org/network/validators) or any other site that monitors the validator network and supports decetralized domain verification.

3. バリデータの公開鍵を公開し、特に他の`rippled`オペレーターに知らせます。 例えば、Webサイト、ソーシャルメディア、[XRPChatコミュニティーフォーラム](https://www.xrpchat.com/)、またはプレスリリースでバリデータの公開鍵を公表できます。


## Revoke validator keys

If your validator's master private key is compromised, you must revoke it immediately and permanently.

`validator-keys`ツールでバリデータ用に生成したマスターキーペアを破棄する方法については、[Key Revocation](https://github.com/ripple/validator-keys-tool/blob/master/doc/validator-keys-tool-guide.md#key-revocation)を参照してください。


## See Also

- **コンセプト:**
    - [XRP Ledgerの概要](xrp-ledger-overview.html)
    - [コンセンサスネットワーク](consensus-network.html)
    - [`rippled`サーバー](xrpl-servers.html)
- **チュートリアル:**
    - [rippledサーバーのクラスター化](cluster-rippled-servers.html)
    - [`rippled`のインストール](install-rippled.html)
    - [容量の計画](capacity-planning.html)
    - [XRP Ledgerのビジネス](xrp-ledger-businesses.html)
- **リファレンス:**
    - [Validator Keysツールガイド](https://github.com/ripple/validator-keys-tool/blob/master/doc/validator-keys-tool-guide.md)
    - \[consensus_infoメソッド\]\[\]
    - \[validator_list_sitesメソッド\]\[\]
    - \[validatorsメソッド\]\[\]


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
