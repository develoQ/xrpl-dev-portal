---
html: offers.html
parent: decentralized-exchange.html
blurb: オファーはXRP Ledgerの通貨取引に関する機能の一つです。 オファーのライフサイクルと特性について説明します。
labels:
  - Decentralized Exchange
---

# オファー

XRP Ledgerの[分散型取引所](decentralized-exchange.html)では、通貨の取引注文は「オファー」と呼ばれます。 オファーはXRPと[トークン](tokens.html)の取引、またはトークン間の取引（同一通貨コードだが発行者が異なるトークンを含む）を行うことができます。 （同一通貨コードで発行者が異なる通貨は、[rippling](rippling.html)によって取引することもできます。 ）

- オファーを作成するには、\[OfferCreateトランザクション\]\[\]を送信します。
- 即時に約定されないオファーはレジャーデータの[Offerオブジェクト](offer.html)になります。 その後のオファーとPaymentにより、レジャーのOfferオブジェクトは約定されます。
- [クロスカレンシー支払い](cross-currency-payments.html)はオファーを約定して流動性を提供します。 However, they never create Offer objects in the ledger.

## オファーのライフサイクル

\[OfferCreate トランザクション\]\[\]は、2つのトークン、またはトークンとXRPの間で取引を行なうための命令です。 それぞれのトランザクションは購入額（`TakerPays`）と売却額（`TakerGets`）を含みます。 トランザクションが処理されると、自動的に一致または交差するオファーが可能な限り約定されます。 その結果、新しいオファーを完全に約定しきれない場合、残りは台帳上のOfferオブジェクトとなります。

Offerオブジェクトは、他のオファーやクロスカレンシー決済で完全に約定されるまで、台帳に保存されます。 オファーを作成したアカウントは、そのオファーの所有者と呼ばれます。 自分が作成したオファーは、専用の\[OfferCancelトランザクション\]\[\]、または\[OfferCreateトランザクション\]\[\]のオプションとして、いつでもキャンセルすることが可能です。

台帳にOfferオブジェクトがある間は、あなたのXRPの一部が[所有者準備金](reserves.html)として設定されます。 何らかの理由でOfferオブジェクトが削除されると、そのXRPは再び使えるようになります。

### Variations

- **Buy vs. Sell:** By default, Offers are "buy" Offers and are considered fully filled when you have acquired the entire "buy" (`TakerPays`) amount. (You may spend less than you expected while receiving the specified amount.) By contrast, a "Sell" Offer is only considered fully filled when you have spent the entire "sell" (`TakerGets`) amount. (You may receive more than you expected while spending the specified amount.) This is only relevant if the Offer _initially_ executes at better than its requested exchange rate: after the Offer gets placed into the ledger, it only ever executes at _exactly_ the requested exchange rate.
- An **Immediate or Cancel** Offer is not placed into the ledger, so it only trades up to the amount that matches existing, matching Offers at the time the transaction is processed.
- A **Fill or Kill** Offer is not placed into the ledger, _and_ it is canceled if the full amount is not filled when it initially executes. This is similar to "Immediate or Cancel" except it _cannot_ be partially filled.
- A **Passive** Offer does not consume matching Offers that have the exact same exchange rate (going the other direction), and instead is placed directly into the ledger. You can use this to create an exact peg between two assets. Passive Offers still consume other Offers that have a _better_ exchange rate going the other way.


### 必要となる資金

オファーを作成する際、その取引で売却する資産の一部でも保有していない場合、取引は「資金不足」として拒否されます。 More specifically:

**トークンを売却する** には、以下のいずれかが必要です。

- そのトークンの任意の正の量を保持する、_または_
- そのトークンの発行者になる。

ただし、オファーで指定された全額を保有する必要はありません。 オファーを作成することで資金が拘束されるわけではないので、同じトークン（またはXRP）を売却するために複数のオファーを作成したり、オファーを作成した後で十分なトークンまたはXRPを調達することも可能です。

**XRPを売却する** には、Offerオブジェクトを台帳に保存するための準備金と、購入するトークンを保存するためのトラストラインの準備金を含む、必要な[準備金](reserves.html)を確保する必要があります。 準備金を確保した後にXRPが残っていれば、Offerオブジェクトを作成することができます。

他のオファーと自身のオファーがマッチした場合、両方のオファーが、その時点における所有者の資金の範囲内で実行されます。 マッチングしたオファーがあり、自分のオファーが完全に約定される前に資金が尽きてしまった場合、残りのオファーはキャンセルされます。 トークンの発行者でない限り、オファーによってトークンの残高がマイナスになることはありません。 (発行者であれば、オファーを使って、オファーで指定された合計金額まで新しいトークンを発行できます。 発行したトークンは、発行者の立場からはマイナス残高として表示されます)。

台帳に存在する自身のオファーと重なるオファーを作成した場合、金額にかかわらず、重なった古いオファーは自動的にキャンセルされます。

It is possible for an Offer to become temporarily or permanently _unfunded_ in the following cases:

- 所有者が売却する資産を一切保有しなくなった場合。
    - オーナーがその資産を再度取得すると、オファーに資金が供給されるようになります。
- 売却する資産が[凍結されたトラストライン](freezes.html)に含まれるトークンである場合。
    - The Offer becomes funded again when the trust line is no longer frozen.
- オファーが新しいトラストラインを作成する必要があるが、オーナーがその[準備金](reserves.html)の増加に伴う十分なXRPを持っていない場合。 (See [Offers and Trust](#offers-and-trust).)
    - The offer becomes funded again when the owner obtains more XRP, or the reserve requirements decrease.
- オファーが失効した場合。 ([オファーの有効期限](#オファーの有効期限)を参照)

資金不足のOfferオブジェクトは、トランザクションによって削除されるまで、台帳に残ります。 台帳からOfferオブジェクトを削除するには、以下の方法があります。

- 一致するオファーまたは[クロスカレンシー支払い](cross-currency-payments.html)によってオファーが全額約定される。
- 所有者が明示的にオファーをキャンセルする。
- 所有者が交差する新しいオファーを作成することにより、暗黙のうちにオファーをキャンセルする。
- トランザクション処理中にオファーが資金不足または期限切れであることが判明する。 Typically this means that another Offer tried to consume it and could not.
    - これには、オファーが支払うことができる残額がゼロになる場合も含まれます。

### 資金不足のオファーの追跡

すべてのオファーの資金状況の追跡は、コンピューターにとって負荷の高い処理となることがあります。 特に積極的に取引しているアドレスでは大量のオファーがオープンです。 1つの残高が、さまざまな通貨を購入する多数のオファーの資金状況に影響することがあります。 このため、XRP Ledgerでは資金不足や期限切れのオファーの検出と削除を _積極的には_ 行なっていません。

クライアントアプリケーションでオファーの資金化の状況をローカルで追跡できます。 このためには、最初に\[book_offersメソッド\]\[\]を使用してオーダーブックを取得し、次にオファーの`taker_gets_funded`フィールドを調べます。 次に`transactions`ストリームを[サブスクライブ](subscribe.html)し、トランザクションメタデータを監視してどのオファーが変更されるかを確認します。


## オファーとトラスト

トラストラインの限度額（[TrustSet](trustset.html)を参照）はオファーに影響しません。 つまり、オファーを使用して、発行者に対して設定したトラストラインの限度額を上回る額を取得できます。

ただし、トークンを保有するには、それらの残高を発行するアドレスへのトラストラインが必要です。 オファーが約定されると、必要なトラストラインが自動的に作成され、その限度額が0に設定されます。 [アカウントが保有する必要のある準備金はトラストラインによって増加する](reserves.html)ため、新しいトラストラインを必要とするオファーがある場合、アカウントはそのトラストラインの準備金として十分なXRPを保有している必要があります。

トラストラインの制限は、あなたの希望以上のトークンを受け取ることを防ぐためのものです。 オファーとは、トークンをどれだけ欲しいかを明示的に示すものであるため、この制限を超えることができます。


## オファーの優先度

取引レートは、`TakerGets`と`TakerPays`の比率として計算されます。 Offers with a higher exchange rate are taken preferentially. （つまり、オファーを約定する人は、支払われる通貨額に対して可能な限り多くの額を受領します。 Offers with the same exchange rate are taken on the basis of which offer was placed first.

同じ取引レートのOfferオブジェクトが同じ台帳ブロックに記録されている場合、オファーの処理順序は[レジャーへトランザクションを適用する](https://github.com/ripple/rippled/blob/5425a90f160711e46b2c1f1c93d68e5941e4bfb6/src/ripple/app/consensus/LedgerConsensus.cpp#L1435-L1538 "Source Code: Applying transactions")ための[正規順序](https://github.com/ripple/rippled/blob/release/src/ripple/app/misc/CanonicalTXSet.cpp "Source Code: Transaction ordering")によって決定します。 この動作は確定的かつ効率的であり、操作することが困難であるように設計されています。


## オファーの有効期限

オファーを設定する際、オプションで有効期限を追加することができます。 デフォルトでは、オファーに有効期限はありません。 すでに有効期限切れのオファーを新たに作成することはできません。

有効期限は秒単位で指定できますが、オファーが期限切れとなる時刻は、実際には正確ではありません。 オファーが期限切れとなるのは、期限切れ時刻が直前の台帳のクローズ時刻より前か等しい場合です。 それ以外の場合は、現実の時刻がオファー期限よりも後でも、オファーが約定する可能性があります。 言い換えると、有効期限が最新の有効な台帳のクローズ時刻よりも遅ければ、実際の時計がどうであろうと、オファーはまだ「有効」なのです。

これは、ネットワークのコンセンサス形成の仕組みによるものです。 ピアツーピアネットワーク全体が合意に達するには、トランザクションを実行する際に、すべてのサーバーがどのオファーが期限切れであるかに合意する必要があります。 個々のサーバーはシステム時刻の設定にわずかな違いがあるため、各サーバーが「現在」時刻を使用した場合、どのオファーが期限切れであるかについて同じ結論に達しない可能性があります。 The close time of a ledger is not known until after the transactions in that ledger have been executed, so servers use the official close time of the _previous_ ledger instead. [台帳のクローズ時刻は丸められます](ledgers.html#ledger-close-times)。 このため、オファーが期限切れかどうかを判断するための時刻と実世界の時刻の差が生じる場合があるのです。

**注記:** 期限切れのOfferオブジェクトは、トランザクションによって削除されるまで、台帳内に残ります。 それまでは、APIから取得したデータ（\[ledger_entryメソッド\]\[\]などを使用）に表示され続ける可能性があります。 トランザクションは、有効期限が切れたOfferオブジェクトや資金不足のOfferオブジェクトを見つけると自動的に削除します。 通常、オファーやクロスカレンシー決済を実行すると、そのOfferオブジェクトはマッチングまたはキャンセルされます。 Offerオブジェクトに対応する所有者準備金は、Offerオブジェクトが実際に削除されたときにのみ再び利用可能になります。


## See Also

- **コンセプト:**
    - [トークン](tokens.html)
    - [パス](paths.html)
- **リファレンス:**
    - \[account_offersメソッド\]\[\]
    - \[book_offersメソッド\]\[\]
    - \[OfferCreateトランザクション\]\[\]
    - \[OfferCancelトランザクション\]\[\]
    - [Offerオブジェクト](offer.html)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
