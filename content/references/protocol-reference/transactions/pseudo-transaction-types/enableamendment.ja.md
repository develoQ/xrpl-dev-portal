---
html: enableamendment.html
parent: pseudo-transaction-types.html
blurb: トランザクション処理を変更するAmendmentプロセスの進行状況を追跡します。
labels:
  - Blockchain
---

# EnableAmendment

An `EnableAmendment` pseudo-transaction marks a change in the status of a proposed amendment when it:

- Gains supermajority approval from validators.
- Loses supermajority approval.
- Is enabled on the XRP Ledger protocol.

<!-- TODO: Move to propose amendments tutorial.

A server only enables amendments when these conditions are met:
  
- A previous ledger includes an `EnableAmendment` pseudo-transaction with the `tfGotMajority` flag enabled.
- The previous ledger in question is an ancestor of the current ledger.
- The previous ledger in question has a close time that is at least two weeks before the close time of the latest flag ledger.
- There are no `EnableAmendment` pseudo-transactions for this amendment with the `tfLostMajority` flag enabled in the consensus ledgers between the `tfGotMajority` pseudo-transaction and the current ledger.

-->

## Example {{currentpage.name}} JSON

```json
{
  "Account": "rrrrrrrrrrrrrrrrrrrrrhoLvTp",
  "Amendment": "42426C4D4F1009EE67080A9B7965B44656D7714D104A72F9B4369F97ABF044EE",
  "Fee": "0",
  "LedgerSequence": 21225473,
  "Sequence": 0,
  "SigningPubKey": "",
  "TransactionType": "EnableAmendment"
}  
```


{% include '_snippets/pseudo-tx-fields-intro.md' %}
<!--{# fix md highlighting_ #}-->

| フィールド            | JSONの型 | \[内部の型\]\[\] | 説明                                                                                                  |
|:---------------- |:------ |:------------ |:--------------------------------------------------------------------------------------------------- |
| `Amendment`      | 文字列    | Hash256      | Amendmentの一意のID。 人間が読み取れる形式の名前ではありません。 既知のAmendmentのリストについては、[Amendment](amendments.html)を参照してください。 |
| `LedgerSequence` | 数値     | UInt32       | Amendmentが含まれているレジャーバージョンのインデックス。 これにより、この疑似トランザクションと別途発生する同様の変更が区別されます。                            |

## EnableAmendment Flags

EnableAmendment疑似トランザクションの`Flags`の値は、この疑似トランザクションが記録されているレジャーでのAmendmentのステータスを示します。

`Flags`の値が`0`（フラグなし）の場合、Amendmentは有効化されており、これ以降のすべてのレジャーに適用されます。 `Flags`のその他の値を以下に示します。

| フラグ名             | Hex Value    | 10進値   | 説明                                                  |
|:---------------- |:------------ |:------ |:--------------------------------------------------- |
| `tfGotMajority`  | `0x00010000` | 65536  | このレジャーバージョン以降、信頼できるバリデータのAmendment支持率は80%以上に増加しました。 |
| `tfLostMajority` | `0x00020000` | 131072 | このレジャーバージョン以降、信頼できるバリデータのAmendment支持率が80%未満に減少しました。 |

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
