---
html: currency-formats.html
parent: basic-data-types.html
blurb: 通貨番号の精度と範囲、カスタム通貨コードのフォーマットです。
label:
  - XRP
  - トークン
---

# 通貨フォーマット

XRP Ledgerには2種類の通貨（[XRP](xrp.html)と[発行済み通貨](issued-currencies.html)）があります。 XRP Ledgerでは、これらの通貨のフォーマットは異なりますが、いずれの通貨も高精度です。

## Comparison

The following table summarizes some of the differences between XRP and tokens in the XRP Ledger:

| XRP                                                                                      | Tokens                                                                                                           |
|:---------------------------------------------------------------------------------------- |:---------------------------------------------------------------------------------------------------------------- |
| Has no issuer.                                                                           | Always issued by an XRP Ledger account.                                                                          |
| Specified as a string.                                                                   | Specified as an object.                                                                                          |
| Tracked in [accounts](accountroot.html).                                                 | Tracked in [trust lines](ripplestate.html).                                                                      |
| Can never be created; can only be destroyed.                                             | Can be issued or redeemed freely.                                                                                |
| 最小値: `0` (Cannot be negative.)                                                           | 最小値: `-9999999999999999e80` 非ゼロの最小絶対値: `1000000000000000e-96`                                                    |
| 最大値: `100000000000`（10<sup>11</sup>）XRP `"100000000000000000"`（10<sup>17</sup>） dropのXRP | 最大値: `9999999999999999e80`                                                                                       |
| Precise to the nearest "drop" (0.000001 XRP)                                             | 10進15桁の精度                                                                                                        |
| Can't be [frozen](freezes.html).                                                         | The issuer can [freeze](freezes.html) balances.                                                                  |
| No transfer fees; XRP-to-XRP payments are always direct.                                 | Can take indirect [paths](paths.html) with each issuer charging a percentage [transfer fee](transfer-fees.html). |
| Can be used in [Payment Channels](payment-channels.html) and [Escrow](escrow.html).      | Not compatible with Payment Channels or Escrow.                                                                  |

For more information, see [XRP](xrp.html) and [Tokens](tokens.html).

## 発行済み通貨の計算

Use the appropriate format for the type of currency you want to specify:

- [XRP Ledgerの発行済み通貨は、以下の精度のカスタムフォーマットで表現されます。](#xrp-amounts)
- [Token Amounts](#token-amounts)

### `"1"` dropのXRP

To specify an amount of XRP, use a [String Number][] indicating _drops_ of XRP, where each drop is equal to 0.000001 XRP. For example, to specify 13.1 XRP:

```
"13100000"
```

**Do not specify XRP as an object.**

XRP amounts cannot be negative.

### Token Amounts

To specify an amount of a [(fungible) token](tokens.html), use an Amount object. This is a JSON object with three fields:

| `Field`           | Type                                               | Description                                                                                                                                                                                                                                                                                                       |
|:----------------- |:-------------------------------------------------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `発行済み通貨額フォーマットの図` | 同一コードの通貨は接続トラストラインを通じて[ripple](rippling.html)できます。 | Arbitrary currency code for the token. 通貨コード`XRP`は発行済み通貨には使用できません。                                                                                                                                                                                                                                                |
| `value`           | [String Number][]                                  | Quoted decimal representation of the amount of the token. This can include scientific notation, such as `1.23e11` meaning 123,000,000,000. Both `e` and `E` may be used. This can be negative when displaying balances, but negative values are disallowed in other contexts such as specifying how much to send. |
| `issuer`          | 文字列フォーマット                                          | Generally, the [account](accounts.html) that issues this token. In special cases, this can refer to the account that holds the token instead.                                                                                                                                                                     |

**Caution:** These field names are case-sensitive.

For example, to represent $153.75 US dollars issued by account `r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59`, you would specify:

```json
{
    "currency": "USD",
    "value": "153.75",
    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
}
```

### Specifying Without Amounts

In some cases, you need to define an asset (which could be XRP or a token) without a specific amount, such as when defining an order book in the [decentralized exchange](decentralized-exchange.html).

To describe a token without an amount, specify it as a currency object, but omit the `value` field. For example:

```json
{
  "currency": "TST",
  "issuer": "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
}
```

To describe XRP without an amount, specify it as a JSON object with _only_ a `currency` field. Never include an `issuer` field for XRP. For example:

```json
{
  "currency": "XRP"
}
```


## String Numbers

{% include '_snippets/string-number-formatting.md' %}

## XRPの精度

XRPの精度は、64ビット符号なし整数と同等であり、各単位は0.000001 XRPと同等です。 It uses integer math, so that any amount less than a full drop is rounded down.

## 発行済み通貨の精度

Tokens can represent a wide variety of assets, including those typically measured in very small or very large denominations. このフォーマットでは、有効数字と10のべき乗の指数を科学的記数法と同様の方法で使用します。 このフォーマットは、指定された範囲内のプラスまたはマイナスの有効桁数と指数に対応しています。 非整数値の一般的な浮動小数点表記とは異なり、このフォーマットでは整数を用いて計算します。 このため、常に15桁の精度が維持されます。 Multiplication and division have adjustments to compensate for over-rounding in the least significant digits.

When sending token amounts in the XRP Ledger's peer-to-peer network, servers [serialize](serialization.html) the amount to a 64-bit binary value.

**Tip:** For tokens that should not be divisible at all, see [Non-Fungible Tokens (NFTs)](non-fungible-tokens.html).

## 通貨コード

{% include '_snippets/data_types/currency_code.md' %}
<!--{#_ #}-->


### 標準通貨コード

The standard format for currency codes is a three-character string such as `USD`. This is intended for use with [ISO 4217 Currency Codes](https://www.xe.com/iso4217.php). The following rules apply:

- Currency codes must be exactly 3 ASCII characters in length. ただし、すべての大文字と小文字、桁数、および記号`?`、`!`、`@`、`#`、`$`、`%`、`^`、`&`、`*`、`<`、`>`、`(`、`)`、`{`、`}`、`[`、`]`、および<code>|</code>の組み合わせを使用できます。
- 標準通貨コードのフォーマット
- 通常、XRP額の指定時には通貨コードは使用しません。 フィールドにXRPの通貨コードが指定されている稀なケースでは、通貨コードのバイナリ形式はすべてゼロになります。

At the protocol level, this format is [serialized](serialization.html#currency-codes) into a 160-bit binary value starting with `0x00`.

### 非標準通貨コード

通貨コードとして160ビット（40文字）16進文字列（例: `015841551A748AD2C1F76FF6ECB0CCCD00000000`）を使用して、その他のタイプの通貨を発行することもできます。 異なる通貨コードタイプとして扱われないようにするには、先頭8ビットが`0x00`であってはなりません。

**廃止予定:** 一部の旧バージョンの[ripple-lib](https://github.com/XRPLF/xrpl.js)では通貨コードタイプとして「有利子」または「マイナス利子」がサポートされていました。 これらの通貨の先頭8ビットは`0x01`です。 マイナス利子/有利子通貨はサポートされなくなりましたが、レジャーデータにこのような通貨が現れることがあります。 詳しくは、[マイナス利子](demurrage.html)を参照してください。

[String Number]: #string-numbers
