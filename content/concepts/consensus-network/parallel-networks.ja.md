---
html: parallel-networks.html
parent: consensus-network.html
blurb: テストネットワークおよび代替レジャーチェーンと本番環境のXRP Ledgerとの関係について説明します。
labels:
  - Blockchain
---

# 並列ネットワーク

XRP Ledgerにはピアツーピアの本番環境のネットワークが1つ存在し、XRP Ledger上で行われるすべての取引はその本番環境のネットワーク、すなわちMainnet内で発生します。

また、Rippleでは、XRPLコミュニティーのメンバーがMainnet上にあるものに影響を及ぼすことなくXRPLテクノロジーとやり取りできるように、TestnetとDevnetの2つの代替ネットワーク（AltNet）を提供しています。 Here's a breakdown of some public altnets:

| ネットワーク                                                      | アップグレード頻度                                                                                 | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------- |:----------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Mainnet                                                     | 安定版リリース                                                                                   | [XRP Ledger](xrp-ledger-overview.html)。 ピアツーピアサーバーのネットワーク機能を備えた分散型の暗号台帳であり、[XRP](xrp.html)の土台となるものです。                                                                                                                                                                                                                                                                                                                                      |
| Testnet                                                     | 安定版リリース                                                                                   | XRP Ledger上に構築したソフトウェアのテスト環境として動作する「代替環境」のネットワーク。 本番環境のXRP Ledgerユーザーに影響を及ぼすことも、本物の通貨をリスクにさらすこともありません。 Testnetの[Amendmentのステータス](known-amendments.html)は、Mainnetを厳密に反映するようになっていますが、分散型システムが持つ予測不可能な性質により、タイミングにわずかな違いが生じることがあります。                                                                                                                                                                                                          |
| Devnet                                                      | ベータ版リリース                                                                                  | XRP Ledgerのコアソフトウェアへの不安定な変更がテストされます。 このAltNetを使用すると、開発者はまだMainnetで有効になっていないXRPLの計画段階の新機能やAmendmentを操作したり学習したりすることができます。                                                                                                                                                                                                                                                                                                                    |
| [Hooks Testnet V2](https://hooks-testnet-v2.xrpl-labs.com/) | [Hooks server](https://github.com/XRPL-Labs/xrpld-hooks)                                  | A preview of on-chain smart contract functionality using [hooks](https://write.as/xumm/xrpl-labs-is-working-on-the-transaction-hooks-amendment-for-the-xrp-ledger).                                                                                                                                                                                                                                                                        |
| AMM-Devnet                                                  | [XLS-30d pre-release](https://github.com/gregtatcam/rippled/tree/amm-core-functionality/) | A preview of the [XLS-30d](https://github.com/XRPLF/XRPL-Standards/discussions/78) standard for Automated Market Makers on the XRP Ledger. <!-- SPELLING_IGNORE: 30d --> [Binary builds for testing](https://github.com/legleux/scheduled/releases) are also available. Library support: [xrpl.js 2.6.0-beta.0](https://www.npmjs.com/package/xrpl/v/2.6.0-beta.0), [xrpl-py 1.8.0b0](https://pypi.org/project/xrpl-py/1.8.0b0/) |

このテスト用XRPは、XRP Ledgerの試用およびアプリケーション開発やインテグレーションに関心のある対象者に、Rippleが[無料で提供](xrp-testnet-faucet.html)するものです。 テスト用XRPは、現実世界での価値はなく、ネットワークがリセットされると失われます。

**注意:** RippleはAltNetの安定性について一切保証しません。 これらのネットワークは、サーバー構成、ネットワークトポロジー、ネットワークパフォーマンスのさまざまな特性をテストする目的でこれまで使用され、またこれからも同様に使用されます。


## 並列ネットワークとコンセンサス

使用するネットワークを定義する`rippled`の設定はありません。 その代わり、信頼するバリデータのコンセンサスに基づいてどのレジャーを正しいレジャーとして受け入れるかを把握します。 `rippled`インスタンスからなる異なるコンセンサスグループが、同じグループの他のメンバーだけを信頼する場合、各グループは引き続き並列ネットワークとして機能します。 悪意のあるコンピューターや不適切に動作するコンピューターが両方のネットワークに接続している場合でも、各ネットワークのメンバーが、定数設定を超えて別のネットワークのメンバーを信頼するように設定されていない限り、コンセンサスプロセスに混乱は生じません。

Rippleでは、TestnetとDevnetでメインサーバーを運用しています。 [独自の`rippled`サーバーをTestnetに接続](connect-your-rippled-to-the-xrp-test-net.html)していただくことも可能です。 TestnetとDevnetでは、多様で検閲耐性のあるバリデータのセットが使用されていません。 そのため、RippleはTestnetやDevnetを定期的にリセットできます。


## See Also

- **ツール:**
    - [XRP Testnet Faucet](xrp-test-net-faucet.html)
- **コンセプト:**
    - [Introduction to Consensus](intro-to-consensus.html)
    - [Amendment](amendments.html)
- **チュートリアル:**
    - [XRP Testnetへの`rippled`の接続](connect-your-rippled-to-the-xrp-test-net.html)
    - [スタンドアロンモードでのrippledの使用](use-stand-alone-mode.html)
- **リファレンス:**
    - \[Server_infoメソッド\]\[\]
    - \[Consensus_infoメソッド\]\[\]
    - \[Validator_list_sitesメソッド\]\[\]
    - \[Validatorsメソッド\]\[\]
    - [デーモンモードのオプション](commandline-usage.html#daemon-mode-options)


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
