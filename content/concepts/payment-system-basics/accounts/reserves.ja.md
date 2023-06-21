---
html: reserves.html
parent: accounts.html
blurb: XRP Ledgerアカウントでは、レジャーデータ内のスパムを減らすためにXRPの準備金が必要です。
labels:
  - 手数料
  - アカウント
top_nav_grouping: 人気ページ
---

# Reserves

XRP Ledgerでは、スパムや悪意のある使用によって、共有グローバル台帳（レジャー）が過度に大きくならないように、 _準備金_ の仕組みをXRPに適用しています。 現在一般に市販されているのマシンで、処理中の現行レジャーを常にRAMに保存でき、全履歴がディスクに収まるように、技術の向上に合わせて台帳が大きくなるのを制限することが目的です。

取引（トランザクション）を送信するには、各アドレスが共有グローバル台帳内に最小量のXRPを保有している必要があります。 このXRPを他のアドレスに送信することはできません。 新しいアドレスに資金供給するには、必要準備金を満たすのに十分なXRPを送信する必要があります。

The reserve requirement changes from time to time due to the [Fee Voting](fee-voting.html) process, where validators can agree to new reserve settings.

## 基本準備金と所有者準備金

The reserve requirement has two parts:

* **基本準備金**は、レジャーの各アドレスに必要なXRPの最小額です。
* **所有者準備金**は、アドレスがレジャーに所有しているオブジェクトごとに必要な準備金の増加額です。 現在、これは1アイテムにつき2 XRP（`2000000` drop）です。

The current reserve requirements on Mainnet are:

- Base reserve: **10 XRP**
- Owner reserve: **2 XRP** per item

Reserves on other networks may vary.

## 所有者準備金

Many objects in the ledger (ledger entries) are owned by a particular account. Usually, the owner is the account that created the object. Each object increases the owner's total reserve requirement by the owner reserve. When objects are removed from the ledger, they no longer count against the reserve requirement.

[Checks](checks.html)は、作成したアドレス（送信先ではなく送信元）が所有します。

Some special cases:

- Non-Fungible Tokens (NFTs) are grouped into pages containing up to 32 NFTs each, and the owner reserve applies per page rather than per NFT. Due to the mechanism for splitting and combining pages, the number of NFTs actually stored per page varies. See also: [Reserve for NFTokenPage objects](nftokenpage.html#reserve-for-nftokenpage-objects).
- Trust lines (`RippleState` entries) are shared between two accounts. The owner reserve can apply to one or both of them. Most often, the token holder owes a reserve and the issuer does not. 詳細については、[Contributing to the Owner Reserve](ripplestate.html#所有者の準備金への資金供給)を参照してください。
- \[MultiSignReserve Amendment\]\[\]が有効になっている場合、1つのSignerListは、メンバーの数に関係なく、所有者準備金用に1つのオブジェクトとしてカウントされます。 関連項目: [SignerListと準備金](signerlist.html#signerlistと準備金)
- [所有者ディレクトリー](directorynode.html)には、アドレスの所有者の準備金の対象となるすべてのレジャーオブジェクトが一覧表示されます。 所有者ディレクトリー自体は準備金としてカウントされません。

### Looking Up Reserves

Applications can look up the current base and incremental reserve values using the \[server_info method\]\[\] or \[server_state method\]\[\]:

| Method                      | Units                | Base Reserve Field                  | Incremental Reserve Field          |
| --------------------------- | -------------------- | ----------------------------------- | ---------------------------------- |
| \[server_info method\]\[\]  | Decimal XRP          | `validated_ledger.reserve_base_xrp` | `validated_ledger.reserve_inc_xrp` |
| \[server_state method\]\[\] | Integer drops of XRP | `validated_ledger.reserve_base`     | `validated_ledger.reserve_inc`     |

To determine the owner reserve of an account, multiply the incremental reserve by the number of objects the account owns. To look up the number of objects an account owns, call the \[account_info method\]\[\] and take `account_data.OwnerCount`.

To calculate an address's total reserve requirement, multiply `OwnerCount` by `reserve_inc_xrp`, then add `reserve_base_xrp`. [Here is a demonstration](build-a-desktop-wallet-in-python.html#codeblock-17) of this calculation in Python.


## Going Below the Reserve Requirement

トランザクション処理中、[トランザクションコスト](transaction-cost.html)によって、送信元アドレスのXRP残高の一部が消却されます。 その結果、そのアドレスのXRPが必要準備金を下回る可能性があります。 You can even destroy _all_ of your XRP this way.

アドレスが保持しているXRPが、現在の必要準備金を下回ると、XRPを他のアドレスに転送するトランザクションを送信したり、自身の準備金を増やしたりできなくなります。 このような場合でも、そのアドレスはレジャー内に存在し、トランザクションコストを支払うのに十分なXRPを持っている限り、その他のトランザクションを送信することができます。 必要準備金を満たすために十分なXRPを受け取った場合、またはそのアドレスのXRP保有額よりも[準備金の必要額が減少した](#必要準備金の変更)場合、そのアドレスはすべてのタイプのトランザクションを再度送信できるようになります。

**ヒント:** アドレスが必要準備金を下回った場合は、新しい\[OfferCreateトランザクション\]\[\]を送信して、追加のXRP、または既存のトラストライン上の他の通貨を入手することができます。 このような取引では、新しい[トラストライン](ripplestate.html)や[レジャー内のオファーノード](offer.html)を作成することはできないため、すでにオーダーブック内にあるオファーを実行するトランザクションのみを実行することができます。


## 必要準備金の変更

The XRP Ledger has a mechanism to adjust the reserve requirements. Such adjustments may consider, for example, long-term changes in the value of XRP, improvements in the capacity of commodity-level machine hardware, or increased efficiency in the server software implementation. 変更はすべて、コンセンサスプロセスによる承認が必要です。 詳細については、[手数料の投票](fee-voting.html)を参照してください。

## See Also

- \[account_objects method\]\[\]
- \[AccountRoot Object\]\[\]
- [Fee Voting](fee-voting.html)
- \[SetFee pseudo-transaction\]\[\]
- [Tutorial: Calculate and display the reserve requirement (Python)](build-a-desktop-wallet-in-python.html#3-display-an-account)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
