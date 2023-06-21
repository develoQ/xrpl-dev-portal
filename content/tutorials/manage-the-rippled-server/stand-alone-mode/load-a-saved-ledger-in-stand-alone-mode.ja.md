---
html: load-a-saved-ledger-in-stand-alone-mode.html
parent: use-stand-alone-mode.html
blurb: 特定の保存済みレジャーからスタンドアロンモードで開始して、トランザクションのテストやリプレイを行います。
labels:
  - コアサーバー
---

# スタンドアロンモードでの保存済みレジャーの読み込み

以前にディスクに保存していた[履歴レジャーバージョン](ledgers.html)を使用して、`rippled`サーバーを[スタンドアロンモード](rippled-server-modes.html)で起動できます。 例えば、以前に`rippled`サーバーをXRP Ledgerのピアツーピアネットワーク（[本番Mainnet、Testnet、Devnetなど](parallel-networks.html)）と同期していた場合は、過去にサーバーで使用できていた任意のレジャーバージョンを読み込むことができます。

履歴レジャーバージョンを読み込むことにより、レジャーを「リプレイ」して、トランザクションがネットワークのルールに従って処理されていたか検証したり、異なる[Amendment](amendments.html)を有効にした場合のトランザクションセットの処理の結果を比較したりすることができます。 万が一、[XRP Ledgerのコンセンサスメカニズムに対する攻撃](consensus-protections.html)が発生して共有レジャーの状態に悪影響が及んでも、このプロセスを始めることで、バリデータのコンセンサスを以前の良好だったネットワークの状態に「ロールバック」できる可能性があります。

**Caution:** As `rippled` is updated to newer versions, amendments are retired and become core functions of the ledger, which can affect how transactions are processed. To produce historically accurate results, you need to replay ledgers using the version of `rippled` the transaction was processed in.

## 1. `rippled`を通常の方法で起動します。

既存のレジャーを読み込むには、最初にネットワークから当該のレジャーを取得する必要があります。 `rippled`をオンラインモードで通常の方法で起動します。

```
rippled --conf=/path/to/rippled.cfg
```

## 2. `rippled`が同期されるまで待ちます。

\[server_infoメソッド\]\[\]を使用して、ネットワークに対するサーバーの状態を確認します。 `server_state`に以下のいずれかの値が示される場合は、サーバーは同期しています。

* `full`
* `proposing`
* `validating`

詳細は、[考えられるサーバーの状態](rippled-server-states.html)を参照してください。

## 3. （省略可）特定のレジャーバージョンを取得します。

最新レジャーのみを必要とする場合は、このステップをスキップできます。

特定の履歴レジャーバージョンを読み込むには、\[ledger_requestメソッド\]\[\]を実行して`rippled`にそのレジャーバージョンを取得させます。 `rippled`にまだレジャーバージョンがない場合は、レジャーの取得が完了するまで`ledger_request`コマンドを複数回実行する必要があります。

特定の履歴レジャーバージョンをリプレイする場合は、リプレイするレジャーバージョンとその直前のレジャーバージョンの両方を取得する必要があります。 （前のレジャーバージョンにより、リプレイするレジャーバージョンに記述されている変更を適用する初期状態が設定されます。 ）

## 4. `rippled`をシャットダウンします。

\[stopメソッド\]\[\]を使用します。

```
rippled stop --conf=/path/to/rippled.cfg
```

## 5. スタンドアロンモードで`rippled`を起動します。

特定の履歴レジャーを読み込むには、`--load`パラメーターと`--ledger`パラメーターを使用し、読み込むレジャーバージョンのレジャーインデックスまたは識別用ハッシュを指定してサーバーを起動します。

```
rippled -a --load --conf=/path/to/rippled.cfg
```

To load a specific historical ledger, start the server with the `--load` parameter along with the `--ledger` parameter, providing the ledger index or identifying hash of the ledger version to load:

```
rippled -a --load --ledger 19860944 --conf=/path/to/rippled.cfg
```

スタンドアロンモードで`rippled`を起動するときに使用可能なオプションについての詳細は、[コマンドラインの使用: スタンドアロンモードのオプション ](commandline-usage.html#スタンドアロンモードのオプション)を参照してください。

## 6. レジャーを手動で進めます。

In stand-alone mode, you must manually advance the ledger with the `ledger_accept` method:

```
rippled ledger_accept --conf=/path/to/rippled.cfg
```

If a transaction depends on the result of a transaction from a different address, advance the ledger to ensure they are processed in the correct order. Otherwise, you can submit multiple transactions from a single address `rippled` sorts transactions from the same address by ascending `Sequence` number.


## See Also

- **リファレンス:**
    - \[ledger_acceptメソッド\]\[\]
    - \[server_infoメソッド\]\[\]
    - [`rippled`コマンドラインの使用](commandline-usage.html)
- **ユースケース:**
    - [XRP Ledgerへのコードの提供](contribute-code.html)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
