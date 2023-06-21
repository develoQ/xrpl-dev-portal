---
html: freezes.html
parent: tokens.html
blurb: Issuers can freeze their issued tokens for compliance purposes.
labels:
  - トークン
---

# Freezing Tokens

XRP Ledgerでは、XRP以外の通貨はすべて発行済み通貨として表すことができます。 **This does not apply to XRP,** which is the native asset of the XRP Ledger, not an issued token.

In certain cases, to meet regulatory requirements, or while investigating suspicious activity, an exchange or gateway may want to freeze token balances.

**ヒント:** 誰もXRPを凍結することはできません。 However, custodial exchanges can always freeze the funds they custody at their own discretion. For more details, see [Common Misunderstandings about Freezes](common-misconceptions-about-freezes.html).

凍結については、3種類の設定があります。

* [**Individual Freeze**](#individual-freeze) - 1件の取引相手を凍結します。
* [**Global Freeze**](#global-freeze) - 取引相手全員を凍結します。
* [**No Freeze**](#no-freeze) - 個々の取引相手の凍結機能と、Global Freezeを終了できる機能を永久に放棄します。

凍結対象の残高がプラス、マイナスにかかわらず、すべての凍結設定を行うことができます。 通貨イシュアーまたは通貨保持者のいずれかがトラストラインを凍結できますが、通貨保持者がイシュアーを凍結しても、その影響はわずかです。


## Individual Freeze

**Individual Freeze**機能は、[トラストライン](trust-lines-and-issuing.html)に関する設定です。 発行アドレスがIndividual Freeze設定を有効にすると、そのトラストラインの通貨に対して以下のルールが適用されます。

* 凍結されたトラストラインの両当事者間の直接決済は、凍結後も可能です。
* The counterparty of that trust line can no longer decrease its balance on the frozen trust line, except in direct payments to the issuer. The counterparty can only send the frozen currencies directly to the issuer.
* 取引相手は、凍結されたトラストライン上で引き続きその他の当事者からの支払を受け取ることができます。
* 取引相手が凍結されたトラストライン上の発行済み通貨の売りオファーを出した場合、[資金不足とみなされます](offers.html#オファーのライフサイクル)。

確認事項: トラストラインではXRPは保持されません。 XRPは凍結できません。

A financial institution can freeze the trust line linking it to a counterparty if that counterparty shows suspicious activity or violates the financial institution's terms of use. 金融機関は、同機関が運用する、XRP Ledgerに接続されているその他のシステムにおいても、その取引相手を凍結する必要があります。 （凍結しないと、アドレスから金融機関経由で支払を送金することで、望ましくない活動を行うことが依然として可能となります。 ）

各個別アドレスは金融機関とのトラストラインを凍結できます。 これは金融機関とその他のユーザーの間の取引には影響しません。 ただし、他のアドレス（[運用アドレス](issuing-and-operational-addresses.html)を含む）からその個別アドレスに対しては、その金融機関のイシュアンスを送信できなくなります。 このようなIndividual Freezeは、オファーには影響しません。

Individual Freezeは1つの通貨にのみ適用されます。 特定の取引相手の複数通貨を凍結するには、アドレスが各通貨のトラストラインで、個別にIndividual Freezeを有効にする必要があります。

[No Freeze](#no-freeze)設定を有効にしている場合、アドレスはIndividual Freeze設定を有効にできません。


## Global Freeze

The **Global Freeze** feature is a setting on an account. An account can enable a global freeze only on itself. 発行アドレスがGlobal Freeze機能を有効にすると、その発行アドレスのすべての発行済み通貨に対して以下のルールが適用されます:

* All counterparties of the frozen issuer can no longer decrease the balances in their trust lines to the frozen account, except in direct payments to the issuer. （これはすべての[運用アドレス](issuing-and-operational-addresses.html)にも影響します。 ）
* 凍結された発行アドレスの取引相手は、発行アドレスとの直接的な支払の送受信を引き続き行うことができます。
* 凍結アドレスによる発行済み通貨の売りオファーはすべて、[資金不足とみなされます](offers.html#オファーのライフサイクル)。

確認事項: アドレスはXRPを発行できません。 Global FreezeはXRPには適用されません。

運用アドレスのシークレットキーが漏えいした場合には、運用アドレスの制御を取り戻した後であっても金融機関の[発行アドレス](issuing-and-operational-addresses.html)に対してGlobal Freezeを有効にすることが有益です。 これにより資金流出を止め、攻撃者がそれ以上の資金を盗むことを防止し、少なくともそれまでの経過の追跡が容易になります。 XRP LedgerでGlobal Freezeを行う他に、金融機関は外部システムへのコネクターでの疑わしい活動を停止する必要があります。

また、金融機関が新しい[発行アドレス](issuing-and-operational-addresses.html)への移行や、営業の停止を予定している場合にも、Global Freezeを有効にすることが有用です。 これにより、特定の時点で資金がロックされるため、ユーザーは他の通貨で取引することができなくなります。

Global Freezeは、当該アドレスによって発行および保有されている _すべての_ 通貨に適用されます。 1つの通貨のみに対してGlobal Freezeを有効にすることはできません。 一部の通貨のみを凍結できるようにしたい場合は、通貨ごとに異なるアドレスを使用してください。

アドレスのGlobal Freeze設定はいつでも有効にできます。 ただし、アドレスの[No Freeze](#no-freeze)設定を有効にすると、Global Freezeを _無効にする_ ことはできません。


## No Freeze

**Global Freeze**機能は、アドレスに設定できます。 An issuer can use this feature to make its tokens as "more like physical money" in the sense that the issuer cannot interfere with counterparties trading the tokens among themselves.

確認事項: XRPはすでに凍結できません。 No Freeze機能は、XRP Ledgerで発行された他の通貨にのみ適用されます。

No Freeze設定には次の2つの効果があります。

* The issuer can no longer enable Individual Freeze on trust lines to any counterparty.
* 発行アドレスは、Global Freezeを有効にしてグローバル凍結を施行できますが、Global Freezeを _無効にする_ ことはできません。

XRP Ledgerは金融機関に対し、その発行資金が表す債務を履行することを強制できません。 このため、Global Freezeを有効にする機能を放棄しても顧客を保護できません。 However, No Freeze ensures that an issuer does not use the Global Freeze feature unfairly against specific users.

No Freeze設定は、アドレスに対して発行される通貨と、アドレスから発行される通貨のすべてに適用されます。 一部の通貨のみを凍結できるようにしたい場合は、通貨ごとに異なるアドレスを使用してください。

No Freeze設定は、アドレスのマスターキーのシークレットキーにより署名されたトランザクションでのみ有効にできます。 [レギュラーキー](setregularkey.html)または[マルチ署名済みトランザクション](multi-signing.html)を使用してNo Freezeを有効にすることはできません。



# See Also

- [凍結コードの例](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/_code-samples/freeze)
- **Concepts:**
    - [Trust Lines and Issuing](trust-lines-and-issuing.html)
- **Tutorials:**
    - [Enable No Freeze](enable-no-freeze.html)
    - [Enact Global Freeze](enact-global-freeze.html)
    - [Freeze a Trust Line](freeze-a-trust-line.html)
- **References:**
    - \[account_lines method\]\[\]
    - \[account_info method\]\[\]
    - \[AccountSet transaction\]\[\]
    - \[TrustSet transaction\]\[\]
    - [AccountRoot Flags](accountroot.html#accountroot-flags)
    - [RippleState (trust line) Flags](ripplestate.html#ripplestate-flags)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
