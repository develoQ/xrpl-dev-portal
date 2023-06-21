---
html: setfee.html
parent: pseudo-transaction-types.html
blurb: Change global reserve and transaction cost settings.
labels:
  - 手数料
---

# SetFee

[手数料投票](fee-voting.html)の結果として[トランザクションコスト](transaction-cost.html)または[アカウント準備金](reserves.html)の要件が変更されます。

**注記:** 疑似トランザクションは送信できませんが、レジャーの処理時に疑似トランザクションが見つかることがあります。

## Example {{currentpage.name}} JSON

```json
{
    "Account": "rrrrrrrrrrrrrrrrrrrrrhoLvTp",
    "BaseFee": "000000000000000A",
    "Fee": "0",
    "ReferenceFeeUnits": 10,
    "ReserveBase": 20000000,
    "ReserveIncrement": 5000000,
    "Sequence": 0,
    "SigningPubKey": "",
    "TransactionType": "SetFee",
    "date": 439578860,
    "hash": "1C15FEA3E1D50F96B6598607FC773FF1F6E0125F30160144BE0C5CBC52F5151B",
    "ledger_index": 3721729,
  }
```

{% include '_snippets/setfee_uniqueness_note.ja.md' %}
<!--{# fix md highlighting_ #}-->

| フィールド               | JSON Type | \[内部の型\]\[\] | Description                                                                                                                                          |
|:------------------- |:--------- |:------------ |:---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BaseFee`           | String    | UInt64       | The charge, in drops of XRP, for the reference transaction, as hex. (This is the [transaction cost](transaction-cost.html) before scaling for load.) |
| `ReferenceFeeUnits` | 符号なし整数    | UInt32       | リファレンストランザクションのコスト（手数料単位）                                                                                                                            |
| `ReserveBase`       | 符号なし整数    | UInt32       | The base reserve, in drops                                                                                                                           |
| `ReserveIncrement`  | 符号なし整数    | UInt32       | The incremental reserve, in drops                                                                                                                    |
| `LedgerSequence`    | 数値        | UInt32       | _（過去に発生した`SetFee`疑似トランザクションの場合は省略）_ この擬似トランザクションが表示されるレジャーバージョンのインデックス。 これにより、この疑似トランザクションと別途発生する同様の変更が区別されます。                                       |


If the _\[XRPFees amendment\]\[\]_ is enabled, `SetFee` pseudo-transactions use these fields instead:

| フィールド                   | JSONの型 | \[Internal Type\]\[\]                        | 説明                                                                                                                                                                                                                       |
|:----------------------- |:------ |:-------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `BaseFeeDrops`          | String | 手数料投票の結果としてトランザクションコストまたはアカウント準備金の要件が変更されます。 | リファレンストランザクションの手数料（XRPのdrop数、16進数）。 （これは、負荷スケーリング前の[トランザクションコスト](transaction-cost.html)です。 ）                                                                                                                             |
| `ReserveBaseDrops`      | 文字列    | 基本準備金（drop数）                                 | The base reserve, in drops                                                                                                                                                                                               |
| `ReserveIncrementDrops` | String | 増分準備金（drop数）                                 | The incremental reserve, in drops                                                                                                                                                                                        |
| `LedgerSequence`        | 数値     | UInt32                                       | _(Omitted for some historical `SetFee` pseudo-transactions)_ The index of the ledger version where this pseudo-transaction appears. This distinguishes the pseudo-transaction from other occurrences of the same change. |


{% include '_snippets/setfee_uniqueness_note.md' %}

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
