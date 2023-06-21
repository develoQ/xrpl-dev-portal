---
html: pseudo-transaction-types.html
parent: transaction-formats.html
blurb: バリデータがXRP Ledgerに適用する場合がある疑似トランザクションのフォーマットです。
template: pagetype-category.html.jinja
labels:
  - Blockchain
---

# 疑似トランザクション

疑似トランザクションはユーザーにより送信されることがなく、またネットワークで伝搬されません。 その代わりに、サーバーは提案されたレジャーに疑似トランザクションを直接挿入することを選択できます。 十分な数のサーバーがこの疑似トランザクションを挿入し、疑似トランザクションがコンセンサスを通過すると、この疑似トランザクションはレジャーに記録され、それ以降レジャーデータに表示されます。

## Special Values for Common Fields

通常のトランザクションの必須フィールドの中には、疑似トランザクションには不適切なものがあります。 このような場合、疑似トランザクションのデフォルト値は以下のようになります。

| ""              | JSON Type | \[Internal Type\]\[\] | デフォルト値                                          |
|:--------------- |:--------- |:--------------------- |:----------------------------------------------- |
| `Account`       | String    | AccountID             | [ACCOUNT_ZERO](accounts.html#special-addresses) |
| `Fee`           | String    | Amount                | `0`                                             |
| `Sequence`      | Number    | UInt32                | `0`                                             |
| `SigningPubKey` | String    | Blob                  | `""` (Empty string)                             |
| `Signature`     | String    | Blob                  | `""` (Empty string)                             |

Pseudo-transactions use the following common fields as normal:

- `TransactionType`
- `Flags`

| フィールド             | JSON Type | \[Internal Type\]\[\] | Description                                                                                                               |
|:----------------- |:--------- |:--------------------- |:------------------------------------------------------------------------------------------------------------------------- |
| `TransactionType` | String    | UInt16                | _(Required)_ The type of transaction.                                                                                     |
| `Flags`           | Number    | UInt32                | _(Optional)_ A set of bit-flags for this transaction. The meaning of specific flags varies based on the transaction type. |

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
