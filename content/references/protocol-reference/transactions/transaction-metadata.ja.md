---
html: transaction-metadata.html
parent: transaction-formats.html
blurb: トランザクションのメタデータは、トランザクションの処理後にトランザクションに追加されるひとまとまりのデータです。
labels:
  - Blockchain
---

# トランザクションのメタデータ

トランザクションのメタデータは、トランザクションの処理後にトランザクションに追加されるひとまとまりのデータです。 レジャーに記録されるトランザクションは、トランザクションが成功するかどうかにかかわらず、メタデータを保持しています。 トランザクションのメタデータには、トランザクションの結果の詳細が含まれます。

**警告:** トランザクションのメタデータに示された変更が最終的なものになるのは、トランザクションが検証済みバージョンのレジャーに記録された場合のみです。

以下に、トランザクションのメタデータに含まれる可能性があるフィールドをいくつか示します。

{% include '_snippets/tx-metadata-field-table.ja.md' %} <!--_ -->

## Example Metadata

The following JSON object shows the metadata for [a complex cross-currency payment](https://xrpcharts.ripple.com/#/transactions/8C55AFC2A2AA42B5CE624AEECDB3ACFDD1E5379D4E5BF74A8460C5E97EF8706B):

```json
{% include '_api-examples/metadata/cross-currency-payment.json' %}
```

## AffectedNodes

The `AffectedNodes` array contains a complete list of the [objects in the ledger](ledger-object-types.html) that this transaction modified in some way. Each entry in this array is an object with one top-level field indicating what type it is:

- `CreatedNode` indicates that the transaction created a new object in the ledger.
- `DeletedNode` indicates that the transaction removed an object from the ledger.
- `ModifiedNode` indicates that the transaction modified an existing object in the ledger.

The value of each of these fields is a JSON object describing the changes made to the ledger object.

### CreatedNode Fields

A `CreatedNode` object contains the following fields:

| Field             | Value                 | Description                                                                                                                                                                                                                                |
|:----------------- |:--------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `LedgerEntryType` | String                | The [type of ledger object](ledger-object-types.html) that was created.                                                                                                                                                                    |
| `LedgerIndex`     | String - \[Hash\]\[\] | The [ID of this ledger object](ledger-object-ids.html) in the ledger's [state tree](ledgers.html). **Note:** This is **not the same** as a [ledger index](basic-data-types.html#ledger-index), even though the field name is very similar. |
| `NewFields`       | Object                | The content fields of the newly-created ledger object. Which fields are present depends on what type of ledger object was created.                                                                                                         |

### DeletedNode Fields

A `DeletedNode` object contains the following fields:

| Field             | Value                 | Description                                                                                                                                                                                                                                |
|:----------------- |:--------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `LedgerEntryType` | String                | The [type of ledger object](ledger-object-types.html) that was deleted.                                                                                                                                                                    |
| `LedgerIndex`     | String - \[Hash\]\[\] | The [ID of this ledger object](ledger-object-ids.html) in the ledger's [state tree](ledgers.html). **Note:** This is **not the same** as a [ledger index](basic-data-types.html#ledger-index), even though the field name is very similar. |
| `FinalFields`     | Object                | The content fields of the ledger object immediately before it was deleted. Which fields are present depends on what type of ledger object was created.                                                                                     |

### ModifiedNode Fields

A `ModifiedNode` object contains the following fields:

| Field               | Value                         | Description                                                                                                                                                                                                                                                                              |
|:------------------- |:----------------------------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `LedgerEntryType`   | String                        | The [type of ledger object](ledger-object-types.html) that was deleted.                                                                                                                                                                                                                  |
| `LedgerIndex`       | String - \[Hash\]\[\]         | The [ID of this ledger object](ledger-object-ids.html) in the ledger's [state tree](ledgers.html). **Note:** This is **not the same** as a [ledger index](basic-data-types.html#ledger-index), even though the field name is very similar.                                               |
| `FinalFields`       | Object                        | The content fields of the ledger object after applying any changes from this transaction. Which fields are present depends on what type of ledger object was created. This omits the `PreviousTxnID` and `PreviousTxnLgrSeq` fields, even though most types of ledger objects have them. |
| `PreviousFields`    | Object                        | The previous values for all fields of the object that were changed as a result of this transaction. If the transaction _only added_ fields to the object, this field is an empty object.                                                                                                 |
| `PreviousTxnID`     | String - \[Hash\]\[\]         | _(May be omitted)_ The \[identifying hash\]\[\] of the previous transaction to modify this ledger object. Omitted for ledger object types that do not have a `PreviousTxnID` field.                                                                                                      |
| `PreviousTxnLgrSeq` | Number - \[Ledger Index\]\[\] | _(May be omitted)_  The \[Ledger Index\]\[\] of the ledger version containing the previous transaction to modify this ledger object. Omitted for ledger object types that do not have a `PreviousTxnLgrSeq` field.                                                                       |

**Note:** If the modified ledger object has `PreviousTxnID` and `PreviousTxnLgrSeq` fields, the transaction always updates them with the transaction's own identifying hash and the index of the ledger version that included the transaction, but these fields' new value is not listed in the `FinalFields` of the `ModifiedNode` object, and their previous values are listed at the top level of the `ModifiedNode` object rather than in the nested `PreviousFields` object.


## delivered_amount

\[Paymentトランザクション\]\[\]の`Amount`。 `Destination`に送金された金額を示し、トランザクションが成功すると、**[Partial Payments](partial-payments.html)であった場合を除いて、** 宛先は当該の金額を受取ります（Partial Paymentsの場合、`Amount`を上限とする正の金額が受取られます）。 `Amount`フィールドを信頼するかどうかを選択するのではなく、メタデータの`delivered_amount`フィールドを使用して、宛先に実際に到達する金額を確認してください。

トランザクションのメタデータの`delivered_amount`フィールドは、成功したすべてのPaymentトランザクションが保持しており、フォーマットは通常の通貨額と同様です。 This field is formatted like a normal currency amount. ただし、送金額は、以下の両方の条件に該当するトランザクションについては使用できません。

* Partial Paymentsである
* 2014-01-20よりも前の検証済みレジャーに含まれている

両方の条件に該当する場合、`delivered_amount`には、実際の金額ではなく文字列値`unavailable`が記述されます。 この場合、トランザクションのメタデータにあるAffectedNodesを読み取ることが、実際に送金された金額を割り出せる唯一の手段になります。

**Note:** The `delivered_amount` field is generated on-demand for the request, and is not included in the binary format for transaction metadata, nor is it used when calculating the [hash](basic-data-types.html#hashes) of the transaction metadata. In contrast, the `DeliveredAmount` field _is_ included in the binary format for partial payment transactions after 2014-01-20.

関連項目: [Partial Payments](partial-payments.html)

<!--{# Spell-check can ignore these field names used in headings #}-->
<!-- SPELLING_IGNORE: affectednodes, creatednode, deletednode, modifiednode, delivered_amount -->

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
