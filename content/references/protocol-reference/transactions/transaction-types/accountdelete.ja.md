---
html: accountdelete.html
parent: transaction-types.html
blurb: Delete an account.
labels:
  - アカウント
---

# AccountDelete

[[ソース]](https://github.com/ripple/rippled/blob/develop/src/ripple/app/tx/impl/DeleteAccount.cpp "Source")

_[DeletableAccounts Amendment](known-amendments.html#deletableaccounts)が必要です_

AccountDeleteトランザクションは、XRP Ledgerで[アカウント](accountroot.html)と、アカウントが所有するオブジェクトを削除し、可能であれば、アカウントの残りのXRPを指定された送金先アカウントに送信します。 アカウントを削除する要件については、[アカウントの削除](accounts.html#アカウントの削除)を参照してください。

## {{currentpage.name}} JSONの例

```json
{
    "TransactionType": "AccountDelete",
    "Account": "rWYkbWkCeg8dP6rXALnjgZSjjLyih5NXm",
    "Destination": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
    "DestinationTag": 13,
    "Fee": "5000000",
    "Sequence": 2470665,
    "Flags": 2147483648
}
```

[Query example transaction. >](websocket-api-tool.html?server=wss%3A%2F%2Fxrplcluster.com%2F&req=%7B%22id%22%3A%22example_AccountDelete%22%2C%22command%22%3A%22tx%22%2C%22transaction%22%3A%221AF19BF9717DA0B05A3BFC5007873E7743BA54C0311CCCCC60776AAEAC5C4635%22%2C%22binary%22%3Afalse%7D)

{% include '_snippets/tx-fields-intro.ja.md' %}
<!--{# fix md highlighting_ #}-->

| フィールド            | JSONの型             | \[内部の型\]\[\] | 説明                                                                                                                 |
|:---------------- |:------------------ |:------------ |:------------------------------------------------------------------------------------------------------------------ |
| `Destination`    | 文字列 - \[アドレス\]\[\] | AccountID    | 送金元アカウントを削除した後に残ったXRPを受信するアカウントのアドレス。 Must be a funded account in the ledger, and must not be the sending account. |
| `DestinationTag` | 数値                 | UInt32       | _（省略可）_ ホストされた受取人、または削除されたアカウントの残りのXRPの受取人に関するその他の情報を識別する任意の[宛先タグ](source-and-destination-tags.html)。              |

## Special Transaction Cost

As an additional deterrent against ledger spam, the AccountDelete transaction requires a much higher than usual [transaction cost](transaction-cost.html): instead of the standard minimum of 0.00001 XRP, AccountDelete must destroy at least the owner reserve amount, currently 2 XRP. This discourages excessive creation of new accounts because the [reserve requirement](reserves.html) cannot be fully recouped by deleting the account.

The transaction cost always applies when a transaction is included in a validated ledger, even if the transaction fails to delete the account. (See [Error Cases](#error-cases).) To greatly reduce the chances of paying the high transaction cost if the account cannot be deleted, [submit the transaction](submit.html) with `fail_hard` enabled.


## エラーケース

すべてのトランザクションで発生する可能性のあるエラーに加えて、{{currentpage.name}}トランザクションでは、次の[トランザクション結果コード](transaction-results.html)が発生する可能性があります。

| エラーコード               | 説明                                                                                                                                                                                                                                                                                                                 |
|:-------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `temDISABLED`        | [DeletableAccounts Amendment](known-amendments.html#deletableaccounts)が有効でない場合に発生します。                                                                                                                                                                                                                              |
| `temDST_IS_SRC`      | `Destination`がトランザクションの送金元（`Account`フィールド）と一致している場合に発生します。                                                                                                                                                                                                                                                         |
| `tecDST_TAG_NEEDED`  | `Destination`アカウントに[宛先タグ](source-and-destination-tags.html)が必要であるのに、`DestinationTag`フィールドが指定されていない場合に発生します。                                                                                                                                                                                                        |
| `tecNO_DST`          | `Destination`アカウントが、レジャーの資金供給のあるアカウントではない場合に発生します。                                                                                                                                                                                                                                                                 |
| `tecNO_PERMISSION`   | `Destination`アカウントに[Deposit Authorization](depositauth.html)が必要で、送金元が事前に承認されていない場合に発生します。                                                                                                                                                                                                                          |
| `tecTOO_SOON`        | 送金元の`Sequence`番号が大きすぎる場合に発生します。 トランザクションの`Sequence`番号に256を加えた値が、現行の\[レジャーインデックス\]\[\]より小さい値でなければなりません。 This prevents replay of old transactions if this account is resurrected after it is deleted.                                                                                                                |
| `tecHAS_OBLIGATIONS` | 削除するアカウントが、レジャーの削除できないオブジェクトに接続されている場合に発生します。 (This includes objects created by other accounts, such as [escrows](escrow.html) and for example [NFT's minted](nftokenmint.html), [even if owned by another account](https://github.com/XRPLF/rippled/blob/develop/src/ripple/app/tx/impl/DeleteAccount.cpp#L197).) |
| `tefTOO_BIG`         | 送金元アカウントが、レジャーの1,000個を超えるオブジェクトにリンクされている場合に発生します。 これらのオブジェクトの一部が先行して個別に削除された場合、トランザクションは再試行で成功する可能性があります。                                                                                                                                                                                                          |


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
