XRP Ledgerのアカウントは、XRP Ledgerの\[base58\]\[\]フォーマットのアドレスで識別されます。 このアドレスはアカウントのマスター[公開鍵](https://en.wikipedia.org/wiki/Public-key_cryptography)から生成され、マスター公開鍵は秘密鍵から生成されます。 アドレスはJSON文字列で記述され、以下の特徴があります。

* 長さは25から35文字です
* 文字`r`で始まります
* 英数字を使用します（数字「`0`」、大文字「`O`」、大文字「`I`」、小文字「`l`」を除く）
* Case-sensitive
* 4バイトのチェックサムを含むため、ランダムな文字から有効なアドレスが生成される確率はおよそ2^32分の1となります

> **注記:** XRPコミュニティは、取引所およびウォレットで[宛先タグ](https://xrpl.org/source-and-destination-tags.html)の代わりに使用できる新しいフォーマット、**X**アドレスを[提案](https://github.com/XRPLF/XRPL-Standards/issues/6)（これをサポートする[コーデック](https://github.com/xrp-community/xrpl-tagged-address-codec)も開発）しました。 これらの「パック化」したアドレスは、`r`ではなく`X`で開始します。 Exchanges and wallets can use X-addresses to represent all the data a customer needs to know in one value. 詳細は、[𝗫-address format](https://xrpaddress.info/)のサイトを参照してください。
> 
> The XRP Ledger protocol only supports "classic" addresses natively, but many [client libraries](client-libraries.html) support X-addresses too.

{% if currentpage.md != "concept-accounts.md" %}
詳しくは、[アカウント](accounts.html)と[base58エンコード](base58-encodings.html)を参照してください。
{% endif %}
