---
html: transaction-common-fields.html
parent: transaction-formats.html
blurb: These common fields can be provided on any XRP Ledger transaction.
labels:
  - トランザクション送信
---

# トランザクションの共通フィールド

どのトランザクションについても、共通する一連のフィールドに加え、[トランザクションのタイプ](transaction-types.html)に応じた追加のフィールドがあります。 フィールドの名前では、大文字と小文字が区別されます。 すべてのトランザクションに共通するフィールドは、以下のとおりです。

| フィールド                           | JSONの型    | \[内部の型\]\[\] | 説明                                                                                                                                                                                                                                                                          |
|:------------------------------- |:--------- |:------------ |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Account`                       | 文字列       | AccountID    | _（必須）_ トランザクションを開始した[アカウント](accounts.html)の一意アドレス。                                                                                                                                                                                                                          |
| `TransactionType`               | 文字列       | UInt16       | _（必須）_ トランザクションのタイプ。 有効なタイプは、`Payment`、`OfferCreate`、`OfferCancel`、`TrustSet`、`AccountSet`、`SetRegularKey`、`SignerListSet`、`EscrowCreate`、`EscrowFinish`、`EscrowCancel`、`PaymentChannelCreate`、`PaymentChannelFund`、`PaymentChannelClaim`、`DepositPreauth`です。               |
| `Fee`                           | 文字列       | Amount       | _（必須。 [自動入力可能][]）_ 整数で表したXRPの額（drop単位）。 このトランザクションをネットワークに送信するためのコストとして消却されます。 トランザクションのタイプによっては、最小要件が異なります。 詳細は、\[トランザクションコスト\]\[\]を参照してください。                                                                                                                              |
| `Sequence`                      | Number    | UInt32       | _（必須。 [自動入力可能][]）_ トランザクションを開始したアカウントに関連付けられた、トランザクションのシーケンス番号。 トランザクションが有効とみなされるのは、その`Sequence`番号が、同一のアカウントの直前トランザクションよりも1大きい場合のみです。 The special case `0` means the transaction is using a [Ticket](tickets.html) instead _(Added by the \[TicketBatch amendment\]\[\].)_. |
| [`AccountTxnID`](#accounttxnid) | 文字列       | Hash256      | _（省略可）_ 別のトランザクションを識別するためのハッシュ値。 このハッシュがある場合、このトランザクションが有効になるのは、送信側のアカウントの直前送信トランザクションがこのハッシュと一致しているときのみです。                                                                                                                                                                 |
| [`Flags`](#flags-field)         | Number    | UInt32       | _（省略可）_ このトランザクションのビットフラグのセット。                                                                                                                                                                                                                                              |
| `LastLedgerSequence`            | 数値        | UInt32       | _（省略可。 使用を強く推奨）_ このトランザクションを登録できるレジャーインデックスの最大値。 このフィールドを指定することにより、トランザクションが検証または拒否されるのを待たなければならない期間の上限を設定することができます。 詳細は、[信頼できるトランザクションの送信](reliable-transaction-submission.html)を参照してください。                                                                                 |
| [`Memos`](#memos-field)         | オブジェクトの配列 | 配列           | _（省略可）_ このトランザクションの識別に使用される任意の追加情報。                                                                                                                                                                                                                                         |
| [`Signers`](#signers-field)     | 配列        | 配列           | _（省略可）_ このトランザクションを承認するための[マルチ署名](multi-signing.html)を表すオブジェクトの配列。                                                                                                                                                                                                          |
| `SourceTag`                     | Number    | UInt32       | _（省略可）_ この支払いの理由、またはこのトランザクションの実行元である送信者を識別するために使用される任意の整数。 一般的に、返金については、最初の支払いの`SourceTag`を返金の`DestinationTag`として指定する必要があります。                                                                                                                                              |
| `SigningPubKey`                 | 文字列       | Blob         | _（署名時に自動追加）_ このトランザクションへの署名に使用される秘密鍵に対応する公開鍵の16進表現。 空文字列の場合は、代わりに`Signers`フィールドにマルチ署名が保持されていることを示します。                                                                                                                                                                       |
| `TicketSequence`                | Number    | UInt32       | _(Optional)_ The sequence number of the [ticket](tickets.html) to use in place of a `Sequence` number. If this is provided, `Sequence` must be `0`. Cannot be used with `AccountTxnID`.                                                                                     |
| `TxnSignature`                  | 文字列       | Blob         | _（署名時に自動追加）_ このトランザクションが、発信元であると主張しているアカウントから発信されたものであることを検証するための署名。                                                                                                                                                                                                        |
\[Removed in: rippled 0.28.0\]\[\]: The `PreviousTxnID` field of transactions was replaced by the [`AccountTxnID`](#accounttxnid) field. This String / Hash256 field is present in some historical transactions. This is unrelated to the field also named `PreviousTxnID` in some [ledger objects](ledger-data-formats.html).


## AccountTxnID

<!-- SPELLING_IGNORE: AccountTxnID -->

`AccountTxnID`フィールドにより、直前のトランザクション（シーケンス番号で識別）も有効で、かつ期待するトランザクションに一致しない限り、現在のトランザクションが有効にならないよう、トランザクションどうしをチェーンにすることができます。

Unlike the `PreviousTxnID` field, which tracks the last transaction to _modify_ an account (regardless of sender), the `AccountTxnID` tracks the last transaction _sent by_ an account. AccountTxnIDを使用するには、アカウントの1つ前のトランザクションのIDがレジャーで追跡されるよう、最初に[asfAccountTxnID](accountset.html#accountsetのフラグ)フラグを設定する必要があります。 (`PreviousTxnID`, by comparison, is always tracked.)

このフィールドが有用になるのは、例えば、トランザクション送信用のプライマリーシステムと受動的なバックアップシステムを運用している場合です。 受動的なバックアップシステムがプライマーリから切断されたものの、プライマリが完全に稼働停止となったわけではなく、両システムが同時に稼働を開始した場合は、トランザクションが2回送信される、あるいはまったく送信されないなど、深刻な問題が発生するおそれがあります。 `AccountTxnID`を使用してトランザクションどうしをチェーンにすると、両方のシステムがアクティブになったときも、有効なトランザクションを送信できるのはいずれか一方のみとなります。

The `AccountTxnID` field cannot be used on transactions that use [Tickets](tickets.html). Transactions that use `AccountTxnID` cannot be placed in the [transaction queue](transaction-queue.html).



## 自動入力可能なフィールド

一部のフィールドについては、トランザクションの署名前に、`rippled`サーバーによって、または署名に使用される\[ripple-lib\]\[\]などのライブラリーによって値を自動入力できます。 値を自動入力するには、最新の状態を取得するためのXRP Ledgerへのアクティブな接続が必要です。 したがって、オフラインでは実行できません。 The details can vary by library, but auto-filling always provides suitable values for at least the following fields:

* `Fee` - ネットワークに基づいて\[トランザクションコスト\]\[\]を自動的に入力します。

    **注記:**`rippled`の\[signメソッド\]\[\]を使用するときは、`fee_mult_max`パラメーターと`fee_mult_div`パラメーターを使用して、自動入力値の上限を設定できます。

* `Sequence` - トランザクションを送信する側のアカウントの次のシーケンス番号を自動的に使用します。

本番システムについては、これらのフィールドの値がサーバーによって入力される状態に _しない_ ことをお勧めします。 例えば、ネットワークの負荷が一時的に急上昇したためにトランザクションコストが高騰した場合、トランザクションによっては、一時的な高額のコストを支払うよりも、必要に応じて待機し、コストが低下してから送信したほうが好ましいことがあります。

\[Paymentトランザクション\]\[\]タイプの[`Paths`フィールド](payment.html#パス)についても、値を自動入力できます。


## Flagsフィールド

`Flags`フィールドには、トランザクションの行動を調整する各種のオプションを設定できます。 オプションは、ビット単位のOR操作と組み合わせることで複数のフラグを同時に設定できるバイナリー値として表現します。

トランザクションで所定のフラグが有効になっているかどうかを確認するには、ビット単位のAND演算子をフラグの値と`Flags`フィールドで使用します。 結果が0の場合は無効になっていることを示し、結果がフラグ値と等しい場合は有効になっていることを示します（その他の結果の場合は、実行した操作に誤りがあることを示します）。 (If you got any other result, you did something wrong.)

ほとんどのフラグは、特定のタイプのトランザクションに対してのみ効果があります。 複数のタイプのトランザクションに対して、同一のビット単位値をフラグに再利用できるため、フラグの設定と読み取りでは`TransactionType`フィールドに留意することが重要です。

フラグとして定義しないビットは、0にする必要があります(\[fix1543 Amendment\]\[\]では、一部のタイプのトランザクションについて、このルールが適用されます。 (The \[fix1543 amendment\]\[\] enforces this rule on some transaction types. デフォルトでは、ほとんどのタイプのトランザクションでこのルールが強制されます)。

### グローバルフラグ

すべてのトランザクションにグローバルに適用される唯一のフラグは、以下のとおりです。

| フラグの名前                | Hex Value    | 10進値       | 説明                                                                                                                                                                                                            |
|:--------------------- |:------------ |:---------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `tfFullyCanonicalSig` | `0x80000000` | 2147483648 | **DEPRECATED** No effect. (If the \[RequireFullyCanonicalSig amendment\]\[\] is not enabled, this flag enforces a [fully-canonical signature](transaction-malleability.html#alternate-secp256k1-signatures).) |

\[signメソッド\]\[\]（または「署名と送信」モードの\[submitメソッド\]\[\]）を使用すると、`rippled`は、`Flags`フィールドがすでに存在している場合を除き、`tfFullyCanonicalSig`フラグを有効にした状態で`Flags`フィールドを追加します。 `tfFullyCanonicalSig`フラグは、`Flags`が明示的に指定されている場合、自動的には有効に***なりません***。 また、\[sign_forメソッド\]\[\]を使用してマルチ署名済みトランザクションに署名を追加する場合も、自動的には有効に***なりません***。

**警告:** `tfFullyCanonicalSig`を有効にしない場合は、不正使用者がトランザクションの署名を改変して、期待されるものとは別のハッシュを使用してトランザクションを成功させることが理論上可能になります。 The \[RequireFullyCanonicalSig amendment\]\[\] ended compatibility with such legacy software and made the protections the default for all transactions. If you are using a [parallel network](parallel-networks.html) that does not have RequireFullyCanonicalSig enabled, you should always enable the `tfFullyCanonicalSig` flag to protect against transaction malleability.

### フラグの範囲

トランザクションの`Flags`フィールドでは、さまざまなレベルや状況に適用されるフラグを設定できます。 個々の状況に関するフラグは、以下の範囲に限定されます。

| 範囲の名前      | ビットマスク       | 説明                                                           |
|:---------- |:------------ |:------------------------------------------------------------ |
| ユニバーサルフラグ  | `0xff000000` | すべてのタイプのトランザクションに対して一様に適用されるフラグ。                             |
| タイプに基づくフラグ | `0x00ff0000` | フラグを使用する[トランザクションのタイプ](transaction-types.html)に応じて意味が異なるフラグ。 |
| 予約済みのフラグ   | `0x0000ffff` | 現時点では定義されていないフラグ。 トランザクションが有効になるのは、これらのフラグが無効になっている場合のみです。   |

**注記:** \[AccountSetトランザクション\]\[\]タイプには、タイプに基づくフラグと似た目的を果たす[ビット単位ではない独自のフラグ](accountset.html#accountsetのフラグ)があります。 [レジャーオブジェクト](ledger-object-types.html)にも、さまざまなビット単位のフラグが定義される`Flags`フィールドがあります。


## Memosフィールド

`Memos`フィールドは、トランザクションに関する任意のメッセージデータを保持します。 このフィールドは、オブジェクトの配列として表現します。 各オブジェクトには唯一のフィールド`Memo`があり、このフィールドは、以下のフィールドを*1つ以上*持つ別のオブジェクトを保持しています。

| フィールド        | 型   | \[内部の型\]\[\] | 説明                                                                                                                             |
|:------------ |:--- |:------------ |:------------------------------------------------------------------------------------------------------------------------------ |
| `MemoData`   | 文字列 | Blob         | Arbitrary hex value, conventionally containing the content of the memo.                                                        |
| `MemoFormat` | 文字列 | Blob         | URLで使用できる文字を表現する16進値。 通例、メモのエンコード方法に関する情報を保持しています（[MIMEタイプ](http://www.iana.org/assignments/media-types/media-types.xhtml)など）。 |
| `MemoType`   | 文字列 | Blob         | URLで使用できる文字を表現する16進値。 通例、このメモのフォーマットを定義する一意の関係（[RFC 5988](http://tools.ietf.org/html/rfc5988#section-4)に準拠）。                   |

MemoTypeフィールドとMemoFormatフィールドには、以下の文字のみを使用できます。 `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-._~:/?#[]@!$&'()*+,;=%`

`Memos`フィールドのサイズの上限は1KBです（バイナリーフォーマットでシリアル化されている場合）。

以下に、Memosフィールドが定義されているトランザクションの例を示します。

```json
{
    "TransactionType": "Payment",
    "Account": "rMmTCjGFRWPz8S2zAUUoNVSQHxtRQD4eCx",
    "Destination": "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV",
    "Memos": [
        {
            "Memo": {
                "MemoType": "687474703a2f2f6578616d706c652e636f6d2f6d656d6f2f67656e65726963",
                "MemoData": "72656e74"
            }
        }
    ],
    "Amount": "1"
}
```


## Signersフィールド

`Signers`フィールドには、最大8つのキーペアから取得された署名を保持し、トランザクションを承認するための[マルチ署名](multi-signing.html)が含まれています。 `Signers`リストはオブジェクトの配列であり、各オブジェクトが1つの`Signer`フィールドを保持しています。 `Signer`フィールドには、以下の入れ子フィールドがあります。

| フィールド           | 型   | \[内部の型\]\[\] | 説明                                       |
|:--------------- |:--- |:------------ |:---------------------------------------- |
| `Account`       | 文字列 | AccountID    | SignerListに記述され、この署名に関連付けられているアドレス。      |
| `TxnSignature`  | 文字列 | Blob         | `SigningPubKey`を使用して検証できる、このトランザクションの署名。 |
| `SigningPubKey` | 文字列 | Blob         | この署名の作成に使用される公開鍵。                        |

`SigningPubKey`は、`Account`アドレスに関連付けられているキーでなければなりません。 参照されている`Account`が、レジャーにあり資金供給済みアカウントである場合、SigningPubKeyには、そのアカウントの現在のレギュラーキー（設定されている場合）を指定できます。 また、[lsfDisableMaster](accountroot.html#accountrootのフラグ)フラグが有効になっている場合を除き、そのアカウントのマスターキーを指定することもできます。 参照されている`Account`アドレスが、レジャーの資金供給済みのアカウントではない場合、`SigningPubKey`は、そのアドレスに関連付けられているマスターキーでなければなりません。

署名の検証は大量の演算能力を消費するタスクであるため、マルチ署名済みトランザクションをネットワークに中継するには、追加のXRPがコストとしてかかります。 マルチ署名に含まれている署名ごとに、トランザクションに必要な\[トランザクションコスト\]\[\]が増加します。 例えば、トランザクションをネットワークに中継するための現在の最小トランザクションコストが`10000`dropである場合、`Signers`配列に3つのエントリーが含まれているマルチ署名済みトランザクションを中継するには、`Fee`の値を少なくとも`40000`dropにする必要があります。



<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}

[自動入力可能]: #自動入力可能なフィールド
