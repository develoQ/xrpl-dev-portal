---
html: ticket.html
parent: ledger-object-types.html
blurb: チケットは、将来使用するために確保されたアカウントのシーケンス番号を追跡します。
labels:
  - トランザクション送信
---

# Ticket

[[ソース]](https://github.com/ripple/rippled/blob/76a6956138c4ecd156c5c408f136ed3d6ab7d0c1/src/ripple/protocol/impl/LedgerFormats.cpp#L155-L164)

_(\[TicketBatch amendment\]\[\]が必要です)_

`Ticket`オブジェクトタイプは、将来の使用のために確保されたアカウント[シーケンス番号](basic-data-types.html#アカウントシーケンス)を追跡する[Ticket](tickets.html)を表します。 \[TicketCreate トランザクション\]\[\]で新しいチケットを作成することができます。 \[New in: rippled 1.7.0\]\[\].

## {{currentpage.name}} JSONの例

```json
{
  "Account" : "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
  "Flags" : 0,
  "LedgerEntryType" : "Ticket",
  "OwnerNode" : "0000000000000000",
  "PreviousTxnID" : "F19AD4577212D3BEACA0F75FE1BA1644F2E854D46E8D62E9C95D18E9708CBFB1",
  "PreviousTxnLgrSeq" : 4,
  "TicketSequence" : 3
}
```

## {{currentpage.name}}フィールド

`Ticket`オブジェクトのフィールドは次のとおりです。

| Name                | JSONの型 | 内部の型      | Required? | 説明                                                                                                                                           |
|:------------------- |:------ |:--------- |:--------- |:-------------------------------------------------------------------------------------------------------------------------------------------- |
| `Account`           | 文字列    | AccountID | Yes       | このチケットを所有する[アカウント](accounts.html)です。                                                                                                         |
| `Flags`             | Number | UInt32    | Yes       | A bit-map of boolean flags enabled for this object. Currently, the protocol defines no flags for `Ticket` objects. The value is always `0`.  |
| `LedgerEntryType`   | 文字列    | UInt16    | Yes       | 文字列 `Ticket` にマッピングされた値 `0x0054` は、このオブジェクトが {{currentpage.name}} オブジェクトであることを示しています。                                                        |
| `OwnerNode`         | 文字列    | UInt64    | Yes       | 送金元の所有者ディレクトリが複数ページで構成されている場合に、このオブジェクトにリンクしているページを示すヒントです。 注記: このオブジェクトには、オブジェクトを含む所有者ディレクトリへの直接リンクは含まれていません。 これは、その値を`Account`から取得できるためです。 |
| `PreviousTxnID`     | 文字列    | Hash256   | Yes       | 最後にこのオブジェクトを変更した[トランザクション](transaction-basics.html)の識別用ハッシュ。                                                                                 |
| `PreviousTxnLgrSeq` | Number | UInt32    | Yes       | 最後にこのオブジェクトを変更したトランザクションを含む\[レジャーインデックス\]\[Ledger Index\]。                                                                                   |
| `TicketSequence`    | Number | UInt32    | Yes       | 本チケットが設定する\[シーケンス番号\]\[\]。                                                                                                                   |

## {{currentpage.name}} IDのフォーマット

TicketオブジェクトのIDは、以下の値がこの順序で連結されているSHA-512ハーフです

* Ticketスペースキー (`0x0054`)
* チケットの所有者のアカウントID
* チケットの`TicketSequence`

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
