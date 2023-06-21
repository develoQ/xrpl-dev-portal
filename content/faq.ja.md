---
html: faq.html
parent: xrp-ledger-overview.html
blurb: Answers to frequently asked questions about the XRP Ledger, the XRPL ecosystem and the community.
subtitle: Your Questions About XRPL, Answered
top_nav_grouping: About
sidebar: disabled
labels:
  - Blockchain
template: page-faq2.html.jinja
filters:
  - faq
name: よくある質問
---

###### よくある質問
# Your Questions About XRPL, Answered

<!--#{ Use H4s for questions and H2s for sections. This keeps the sidebar nav from getting too clustered and allows the faq filter to stylize things as an accordion. #}-->

#### XRP Ledger(XRPL)はRippleが所有するプライベートブロックチェーンですか？

いいえ。 XRP Ledgerは、中央権限のないシステムであるため、正式なバリデータのオンボーディングプロセスのようなものは存在しません。 トランザクション処理やコンセンサスに影響を与えるような変更は、ネットワークバリデーターの80％以上の承認が必要です。 Rippleはネットワークへの貢献者の一員ですが、その権利は他の貢献者たちと同じです。 ネットワークには150以上のバリデーターが参加しており、うちデフォルトのユニークノードリスト(UNLs)に登録されているものは35以上です。 Rippleはこれらのうち[2つのノード](https://foundation.xrpl.org/2022/10/06/unl-update-october-2022)を運営しています。 ユニークノードリスト(UNLs)に関しては[ユニークノードリスト（UNL）とは何ですか?](#ユニークノードリストunlとは何ですか)の項目を参照してください。

#### プルーフ・オブ・ワーク(PoW)が最善の検証メカニズムではないのですか？

プルーフ・オブ・ワークは、信頼できる第三者を必要とせずに二重支出の問題を解決する最初のコンセンサンス・メカニズムでした。 [XRP Ledgerのコンセンサス・メカニズム](consensus.html)は、同じ問題をはるかに速く、安く、より良いエネルギー効率で解決します。

#### どのようにして持続可能なブロックチェーンを実現するのでしょうか？

ビットコインのエネルギー消費量は、2021年現在アルゼンチンのエネルギー総消費量に相当します。 また、ビットコインの採掘者が使用する電力の多くは環境を汚染するリソースから供給されていることが広く報告されています。 XRPLは、プルーフ・オブ・ワークのようにエネルギーを浪費しない[コンセンサス・メカニズム](intro-to-consensus.html)を通じてトランザクションを承認し、カーボン・オフセットを活用して、[真にカーボンニュートラルな最初のブロックチェーンのひとつ](https://ripple.com/ripple-press/ripple-leads-sustainability-agenda-to-achieve-carbon-neutrality-by-2030)となっています。

#### XRPLではXRP以外の通貨も取引できますか？

Yes, the XRP Ledger was built specifically to be able to tokenize arbitrary assets, such as USD, EUR, Oil, Gold, reward points, and more. Any currency can be issued on the XRP Ledger. This is illustrated by XRPL’s growing community that backs a variety of fiat and crypto tokens.

#### XRPLは決済用だけではないのですか？

当初は決済のユースケース向けに開発されましたが、台帳であるXRP Ledgerとそのネイティブ・デジタルアセットであるXRPの両方は、NFTなどの様々な革新的ブロックチェーンのユースケースで更に人気を集めています。 New standards proposals for an automated market maker (AMM), hooks amendment for smart contract functionality, and cross-chain bridges are all currently works in progress.

## バリデータ（検証者）とユニークノードリスト

#### トランザクション（取引）のバリデータはどのようなサービスを提供するのですか?

All nodes ensure that transactions meet protocol requirements, and are therefore “valid.” The service that validators uniquely provide is administratively grouping transactions into ordered units, agreeing on one such ordering specifically to prevent double spending. <!-- STYLE_OVERRIDE: therefore -->

コンセンサスプロセスの詳細は、[コンセンサス](consensus.html)を参照してください。


#### How much does it cost to run a validator?

バリデータを実行するのに手数料やXRPは必要ありません。 メールサーバーを稼働するための電気代相当です。


#### ユニークノードリスト（UNL）とは何ですか?

UNLs are the lists of validators a given participant believes will not conspire to defraud them. Each server operator can choose their own UNL, usually based on a default set provided by a trusted publisher. (A default set from a publisher is sometimes called a default UNL, or _dUNL_.) <!-- STYLE_OVERRIDE: will --> <!-- SPELLING_IGNORE: dUNL -->


#### どのUNLを選択すればよいですか?

誰でもバリデータになりえるため、信頼できるセットを選択する責任はネットワーク参加者側にあります。 Currently, three publishers (Ripple, the XRP Ledger Foundation, and Coil) are known to publish recommended default lists of high quality validators, based on past performance, proven identities, and responsible IT policies.  However, every network participant can choose which validators it chooses as reliable and need not follow one of the three publishers noted above.


#### RippleがそのUNLの採用を推奨しているなら、それは集中型システムを形成することにならないのですか?

いいえ。 XRP Ledgerネットワークはオプトイン方式です。 各参加者は直接的または間接的に自身のUNLを選択することができます。 万が一、Rippleが活動を停止したり、Rippleが悪意を持って行動したりした場合、参加者は自身のUNLを変更してXRP Ledgerを引き続き使用することができます。


#### What is the incentive structure for validators?

バリデータを運用することの主なインセンティブは、ネットワークの安定した運用と合理的な進化を維持し、保護することです。 XRP Ledgerの進化はバリデータによって決定されるため、XRP Ledgerを使用するかこれに依存するビジネスには、ネットワークの信頼性と安定性が確保されるというインセンティブが内在することになります。 Validators also earn the respect and goodwill of the community by contributing this way.

XRP Ledgerサーバーを運用してネットワークに参加している場合、バリデータを運用するための費用や労力は最小限に抑えられます。 つまり、Bitcoinにおけるマイニング報酬といった、追加のインセンティブは不要であるということです。 Rippleでは、バリデータの運用報酬としてXRPを支払うことはしないため、そのようなインセンティブによってバリデータの行動が歪められることはありません。

For examples of how incentives can warp validation behavior, read about [miner extractable value (MEV)](https://arxiv.org/abs/1904.05234).


#### 金融機関は、特定の制度上の基準や要件を満たすのに役立つトランザクションバリデータを設定できますか?

いいえ。 トランザクション選択のためにカスタマイズされたバリデータポリシーを金融機関が設定することはできません。 Validators either follow the protocol, or they do not. ソフトウェアは、プロトコルルールに従わない場合は機能しません。 Thus, it is not recommended that institutions seek out custom implementations without in-house expertise.


#### ネットワーク内の20%を超えるノードが過半数と一致しない場合はどうなりますか? レジャー（台帳）の最終バージョンはどのように選択されますか?

Normally, if there is a dispute about the validity of one transaction, that transaction gets pushed back until the majority can come to an agreement. But if more than 20% of the network did not follow the same protocol rules as the majority, the network would temporarily halt. It could resume when participants reconfigure their UNLs based on those who want to reach consensus with each other. この一時的な処理の遅れは、むしろ二重支出のリスクを回避します。

レジャーの正式な最終バージョンを決定する過程で、一時的な内部バージョンが複数存在する可能性があります。 分散型システムでは、すべてのノードが同じ順序でトランザクションを受け取るわけではないため、そのような内部バージョンが発生します。 従来のBitcoinにおける類似の振る舞いとしては、2つのブロックがほぼ同時にマイニングされたために2つのサーバーがそれぞれ異なる最長チェーンを参照してしまう状況があります。

However, there can be only one latest _validated_ ledger version at any given time; other versions are irrelevant and harmless.

For more information about how the XRP Ledger's consensus mechanism behaves in adverse situations, see [Consensus Protections Against Attacks and Failure Modes](consensus-protections.html).


#### XRP Ledgerでは正式なバリデータのオンボーディングプロセスを使用していますか?

No, a formal process for adding validators is not compatible with the XRP Ledger, because it is a system with no central authority.

Publishers of individual default UNLs set their own policies for when to add or remove validators from their lists of recommendations.

推奨事項やベストプラクティスについては、[バリデータとしての`rippled`の実行](run-rippled-as-a-validator.html)を参照してください。


#### If the dUNL has the most influence on the network, then is the XRPL centralized?
Validators can choose to not use the dUNL, or any widely-used UNL for that matter. Anyone can create a new UNL at any time.

There can be multiple UNLs in use on the same network. Each operator can customize their server's own UNL or choose to follow a different recommended list. All these servers can still run the same chain and reach consensus with one another.

However, if your UNL does not have enough overlap with the UNLs used by others, there is a risk that your server forks away from the rest of the network. As long as your UNL has > 90% overlap with the one used by people you're transacting with, you are completely safe from forking. If you have less overlap, you may still be able to follow the same chain, but the chances of forking increase with lower overlap, worse network connectivity, and the presence of unreliable or malicious validators on your UNL.


## XRPの役割


#### RippleはなぜXRPを多く保有しているのですか?

XRP was created as the XRP Ledger's native asset to empower a new generation of digital payments—faster, greener, and cheaper than any previous digital asset. It also serves to protect the ledger from spam, and to [bridge currencies](autobridging.html) in the XRP Ledger's decentralized exchange, when doing so is beneficial to users. Over time, the XRP Ledger community has pioneered new [use cases](uses.html) for XRP as well was the XRP Ledger itself.


#### XRP Ledgerでは大量のトランザクションが行われている場合にどう対応しますか?

XRP Ledgerは、スパム対策として、需要に基づいてトランザクションコストを動的に設定するように設計されています。 潜在的なXRPの操作による影響は、時価総額とトランザクション量の増加に伴うネットワークサイズの拡大によって最小限に抑えられます。


#### What about money laundering and suspicious economic activity?

<!-- STYLE_OVERRIDE: regarding -->

The XRP Ledger network is an open network, and all transactions are publicly visible.

Rippleは、XRP Ledgerネットワーク全体でAML(Anti-Money Laundering)フラグを監視および報告するとともに、必要に応じてFinCEN(Financial Crimes Enforcement Network )に疑わしい活動を報告することをコミットしています。

[XRP Forensics / xrplorer](https://xrplorer.com/) maintains an advisory list to track and minimize money laundering, scams, fraud, and illicit use of the XRP Ledger. Exchanges and other service providers can use this service to prevent and react to financial crimes.


## セキュリティー上の懸念


#### What is the process for reviewing third-party code contributions?

コード提供プロセスは、開発者がRippleの`rippled`リポジトリーに対してプルリクエストを出すことから始まります。

このプルリクエストがあると、自動化された単体テストと統合テスト、およびそのプルリクエストによって変更されるコードについて専門知識を持つ開発者によりコードレビューが行われます。

プルリクエストが自動テストに合格し、レビュー担当者から承認されると、[リポジトリの信頼できる保守担当者](https://opensource.guide/best-practices/)によって、次のベータ版に含められるようにステージングされます。


#### RippleはXRP LedgerまたはXRP Ledgerネットワークを所有または管理していますか?

いいえ、RippleはXRP LedgerとXRP Ledgerネットワークを所有も管理もしていません。

Rippleは、コアとなるXRP Ledgerサーバー（[`rippled`](https://github.com/ripple/rippled)）のリファレンス実装を公開し、オープンソースコードベースに貢献しているエンジニアチームを雇用しています。 Rippleはまた、利用可能なソフトウェアのプリコンパイル済みバイナリーパッケージも定期的に発行しています。 必要に応じて、誰でも自由に[ソースからソフトウェアをダウンロードしてコンパイル](install-rippled.html)できます。

Several entities publish recommended validator lists (UNLs). As of September 2022, Ripple runs 3 of the 35 validators in the default UNL.


#### Rippleは検証用のコードベースとユーザーソフトウェア用のコードベースを区別していますか?

はい。 XRPLは米ドル、ユーロ、石油、金、リワードポイントなど、任意の資産をトークン化できるように構築されており、どんな通貨でもXRPL上で発行することができます。 成長中のXRPLコミュニティは様々なフィアットトークンや暗号通貨をサポートしており、これを裏付けています。
