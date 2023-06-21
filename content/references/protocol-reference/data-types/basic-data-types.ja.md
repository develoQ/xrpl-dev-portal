---
html: basic-data-types.html
parent: protocol-reference.html
blurb: Format and meaning of fundamental data types like addresses, ledger index, and currency codes.
---

# 基本的なデータ型

さまざまなタイプのオブジェクトがそれぞれ異なる方法で一意に識別されます。

[アカウント](accounts.html)は[アドレス][]で識別されます。 例えば、`"r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"`など。 アドレスは常に「r」で始まります。 `rippled`メソッドの多くは、16進数表記に対応しています。

[トランザクション](transaction-formats.html)は、トランザクションのバイナリフォーマットの[ハッシュ][]で識別されます。 また、トランザクションは送信アカウントと[シーケンス番号][]でも識別できます。

閉鎖された各[レジャー](ledger-data-formats.html)は、[レジャーインデックス][]と[ハッシュ][]値を保有します。 [レジャーを指定する](#レジャーの指定)場合、いずれか1つを使用できます。

## アドレス

{% include '_snippets/data_types/address.ja.md' %}
<!--{#_ #}-->


## ハッシュ

{% include '_snippets/data_types/hash.ja.md' %}
<!--{#_ #}-->

### ハッシュプレフィクス
[[ソース]](https://github.com/ripple/rippled/blob/master/src/ripple/protocol/HashPrefix.h "Source")

多くの場合、XRP Ledgerではオブジェクトのバイナリデータに4バイトのプレフィクスを付けてからハッシュを計算するため、異なるタイプのオブジェクトが同じバイナリフォーマットである場合でも、異なるハッシュが設定されます。 既存の4バイトコードは、ASCIIでエンコードされた英字3文字の後に0バイトが続く構成となっています。

ある種のハッシュは、APIの要求と応答に使用されます。 またある種のデータに署名するときの最初のステップで計算されるだけのものや、より高度なハッシュを計算するためのものもあります。 XRP Ledgerで使用されるすべての4バイトのハッシュプレフィクスは以下の表の通りです。

| オブジェクトタイプ                  | APIフィールド                        | ハッシュプレフィクス（16進数） | ハッシュプレフィクス（テキスト） |
|:-------------------------- |:------------------------------- |:---------------- |:---------------- |
| コンセンサスの提案                  | N/A                             | `0x50525000`     | `PRP\0`         |
| レジャーバージョン                  | `ledger_hash`                   | `0x4C575200`     | `LWR\0`         |
| レジャー状態データ                  | `account_state` （[レジャーヘッダー][]内） | `0x4D4C4E00`     | `MLN\0`         |
| レジャーデータ内部ノード               | N/A                             | `0x4D494E00`     | `MIN\0`         |
| レジャーデータ内部ノード（[SHAMapv2][]） | N/A                             | `0x494E5200`     | `INR\0`         |
| Payment Channelのクレーム       | N/A                             | `0x434C4D00`     | `CLM\0`         |
| 署名済みのトランザクション              | トランザクションの`hash`                 | `0x54584E00`     | `TXN\0`         |
| メタデータを持つトランザクション           | N/A                             | `0x534E4400`     | `SND\0`         |
| 未署名のトランザクション（シングル署名）       | N/A                             | `0x53545800`     | `STX\0`         |
| 未署名のトランザクション（マルチ署名）        | N/A                             | `0x534D5400`     | `SMT\0`         |
| 検証の投票                      | N/A                             | `0x56414C00`     | `VAL\0`         |
| Validator manifest         | N/A                             | `0x4D414E00`     | `MAN\0`         |

[レジャーオブジェクトID](ledger-object-ids.html)も似た方法で計算されますが、ここで説明したプレフィクスの代わりに「スペースキー」という2バイトのプレフィクスを使用します。


## アカウントシーケンス

{% include '_snippets/data_types/account_sequence.ja.md' %}
<!--{#_ #}-->


## レジャーインデックス

{% include '_snippets/data_types/ledger_index.ja.md' %}
<!--{#_ #}-->


### レジャーの指定

APIメソッドの多くは、レジャーのインスタンスを指定する必要があります。 その場合、共有されたレジャーの特定バージョンで最新と見なされるデータで指定する必要があります。 レジャーバージョンを受け入れるコマンドは、すべて同様に機能します。 使用するレジャーを指定するには、以下の3つの方法があります。

1. `ledger_index`パラメーターにレジャーの[レジャーインデックス][]を指定します。 Each closed ledger has a ledger index that is 1 higher than the previous ledger. （最初のレジャーのインデックスは1です。 ）
   
        "ledger_index": 61546724

2. `ledger_hash`パラメーターにレジャーの[ハッシュ][]値を指定します。
   
        "ledger_hash": "8BB204CE37CFA7A021A16B5F6143400831C4D1779E6FE538D9AC561ABBF4A929"

3. `ledger_index`パラメーターに以下のいずれかのショートカットを指定します。

    * `validated`: ネットワーク全体で検証された最新のレジャー
      
            "ledger_index": "validated"

    * `closed`: 変更できないように閉鎖され、検証を提案されている最新のレジャー

    * `current`: サーバーで現在処理中のレジャーバージョン

上記3つのフォーマットすべてを受け入れる、廃止予定の`ledger`パラメーターもあります。 このパラメーターは使用*しないでください*。 今後予告なしに廃止される可能性があります。

If you do not specify a ledger, the server decides which ledger to use to serve the request. レジャーを指定しない場合、デフォルトで`current`（処理中）レジャーが選択されます。 In [Reporting Mode](rippled-server-modes.html#reporting-mode), the server uses the most recent validated ledger instead. Do not provide more than one field specifying ledgers.

**注記:** レジャーを指定する際に上記のデフォルトの動作に頼らないでください。 変更される場合があります。 可能であれば、常に要求にてレジャーバージョンを指定してください。

Reporting Mode does not record ledger data until it has been validated. If you make a request to a Reporting Mode server for the `current` or `closed` ledger, the server forwards the request to a P2P Mode server. If you request a ledger index or hash that is not validated, a Reporting Mode server responds with a `lgrNotFound` error.


## 通貨

XRP Ledgerには2種類の通貨があります。 XRPとその他のあらゆる通貨です。 These two types of currencies are specified in different formats, with different precision and rounding behavior.

Some fields, such as the destination `Amount` of a \[Payment transaction\]\[\], can be either type. Some fields only accept XRP specifically, such as the `Fee` field ([transaction cost](transaction-cost.html)).

）XRPは正式には「drop」で指定します。 これは0.000001（百万分の1）のXRPと等価です。 Tokens are instead specified as an object with fields for the decimal amount, currency code, and issuer. For example:

- **注意:** XRP Ledgerでは、通常の浮動小数点数とは異なる精度の小数点計算を使用するため、通貨額は常に文字列として表されます。
  
        "1000000"

- **注意:** これらのフィールド名は大文字小文字の区別があります。
  
        {
          "currency": "USD",
          "value": "153.75",
          "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
      }

For more information, see [Currency Formats](currency-formats.html).


## 時間の指定

`rippled`サーバーとそのAPIでは、時間を符号なし整数で表します。 この数値は、「Rippleエポック」である2000年1月1日（00:00 UTC）から経過した秒数を表しています。 これは[UNIXエポック](http://en.wikipedia.org/wiki/Unix_time)と同様に機能しますが、RippleエポックはUNIXエポックより946684800秒遅れています。

Rippleエポック時間を32ビット変数でUNIXエポック時間に変換しないでください。 整数のオーバーフローが発生する恐れがあります。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}

[アドレス]: #アドレス
[ハッシュ]: #ハッシュ

[レジャーヘッダー]: ledger-header.html
[SHAMapv2]: known-amendments.html#shamapv2
[シーケンス番号]: #アカウントシーケンス
[レジャーインデックス]: #レジャーインデックス
