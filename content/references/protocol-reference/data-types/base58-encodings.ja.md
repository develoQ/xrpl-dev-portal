---
html: base58-encodings.html
parent: basic-data-types.html
blurb: 暗号鍵と関連データをbase58形式で表すフォーマットです。
---

# base58エンコード

XRP Ledgerにより、さまざまなタイプの値をエンコードする前に、データタイプを区別する固有の8ビット数値が値の前に付加されます。 XRP Ledgerのbase58ディクショナリの文字配列と組み合わされた、さまざまなタイプのエンコード値のbase58表現は、タイプごとに固有の文字で始まります。

The XRP Ledger prefixes different types of values with a specific 8-bit number before encoding them to distinguish between different data types. With the arrangement of characters in the XRP Ledger's base58 dictionary, the result is that the base58 representations for different types of encoded values start with specific letters by type.

以下の表に、XRP Ledgerで使用されるすべてのエンコードを示します。

| データタイプ         | 開始文字 | タイププレフィクス | コンテンツのサイズ¹ | 最大文字数 |
|:-------------- |:---- |:--------- |:---------- |:----- |
| [アカウント][]アドレス  | r    | `0x00`    | 20バイト      | 35    |
| アカウントの公開鍵      | a    | `0x23`    | 33バイト      | 53    |
| シード値（シークレットキー） | s    | `0x21`    | 16バイト      | 29    |
| 検証公開鍵またはノード公開鍵 | n    | `0x1C`    | 33バイト      | 53    |

¹ コンテンツのサイズでは1バイトのタイププレフィクスは除外されます。

## See Also

- [アドレスのエンコード](accounts.html#アドレスのエンコード) - アドレスのエンコードについての詳細な情報
- [暗号鍵](cryptographic-keys.html) - XRP Ledgerの暗号鍵のタイプとその使用法
- \[wallet_proposeリファレンス\]\[wallet_proposeメソッド\] - アカウントキーを生成するためのAPIメソッド
- \[validation_createリファレンス\]\[validation_createメソッド\] - バリデータキーを生成するためのAPIメソッド


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}

[アカウント]: accounts.html
