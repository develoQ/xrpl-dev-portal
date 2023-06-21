---
html: send-xrp.html
parent: get-started.html
blurb: Learn how to send test payments right from your browser.
cta_text: XRPを送金しよう
embed_xrpl_js: true
filters:
  - interactive_steps
labels:
  - XRP
  - 支払い
top_nav_grouping: 人気ページ
---

# XRPの送金

このチュートリアルでは、xrpl.jsを使用してシンプルなXRP送金を行う方法について説明します。 まずは、XRP Testnetを使用してプロセスを順に進めます。 次に、そのプロセスと、本番で同様の処理を行う場合に発生する追加要件とを比較します。

**Tip:** Check out the [Code Samples](https://github.com/XRPLF/xrpl-dev-portal/tree/master/content/_code-samples) for a complete version of the code used in this tutorial.

## 前提条件

<!-- Source for this specific tutorial's interactive bits: -->
<script type="application/javascript" src="assets/js/tutorials/send-xrp.js"></script>
{% set use_network = "Testnet" %}

To interact with the XRP Ledger, you need to set up a dev environment with the necessary tools. This tutorial provides examples using the following options:

- このページでは、xrpl.jsライブラリーを使用するJavaScriptの例を紹介します。 [xrpl.js入門ガイド](get-started-using-javascript.html)に、xrpl.jsを使用してJavaScriptからXRP Ledgerデータにアクセスする方法の説明があります。
- **Python** with the [`xrpl-py` library](https://xrpl-py.readthedocs.io/). See [Get Started using Python](get-started-using-python.html) for setup steps.
- **Java** with the [xrpl4j library](https://github.com/XRPLF/xrpl4j). See [Get Started Using Java](get-started-using-java.html) for setup steps.


## Testnetでの送金
{% set n = cycler(* range(1,99)) %}

### {{n.next()}}. 検証の待機

XRP Ledgerでトランザクションを送信するには、まずアドレスと秘密鍵、そしていくらかのXRPが必要となります。 The address and secret key look like this:

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/send-xrp/js/send-xrp.js", start_with="// Example credentials", end_before="// Connect", language="js") }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", end_before="# Connect", language="py") }}

_Java_

{{ include_code("_code-samples/send-xrp/java/SendXrp.java", end_before="// Connect", language="java") }}

<!-- MULTICODE_BLOCK_END -->

The secret key shown here is for example only. 次のインターフェイスを使用して、XRP Test NetにあるアドレスとTestnet XRPを入手できます。

{% include '_snippets/interactive-tutorials/connect-step.ja.md' %}

When you're [building production-ready software](production-readiness.html), you should use an existing account, and manage your keys using a [secure signing configuration](set-up-secure-signing.html).


### {{n.next()}}. Testnetサーバーへの接続

First, you must connect to an XRP Ledger server so you can get the current status of your account and the shared ledger. You can use this information to [automatically fill in some required fields of a transaction](transaction-common-fields.html#auto-fillable-fields). ）トランザクションの送信先となるネットワークに接続する必要があります。

以下のサンプルコードでは公開XRP Testnetサーバーに接続します。

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/get-started/js/base.js", language="js") }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", start_with="# Connect", end_before="# Get credentials", language="py") }}

_Java_

{{ include_code("_code-samples/send-xrp/java/SendXrp.java", start_with="// Connect", end_before="// Prepare transaction", language="java") }}

<!-- MULTICODE_BLOCK_END -->

このチュートリアルでは、以下のボタンをクリックすることでブラウザーから直接接続できます。

{% include '_snippets/interactive-tutorials/generate-step.ja.md' %}


### {{n.next()}}. トランザクションの準備

通常は、XRP LedgerトランザクションをオブジェクトとしてJSON[トランザクションフォーマット](transaction-formats.html)で作成します。 以下の例に、必要最小限の送金仕様を示します。

```json
{
  "TransactionType": "Payment",
  "Account": "rPT1Sjq2YGrBMTttX4GZHjKu9dyfzbpAYe",
  "Amount": "2000000",
  "Destination": "rUCzEr6jrEyMpjhs4wSdQdz4g8Y382NxfM"
}
```

XRP送金に対して指定する必要がある必要最小限の指示は次のとおりです。

- An indicator that this is a payment. これが送金であることを示すインディケーター（`"TransactionType": "Payment"`）
- The sending address. 送信元アドレス（`"Account"`）
- XRPを受け取るアドレス（`"Destination"`）。 このアドレスは送信元アドレスと同じものではいけません。
- 送金するXRP額（`"Amount"`）。 通常、XRPの「drop数」を示す整数として指定します。 1,000,000ドロップは1 XRPです。

技術上、一部の追加のフィールドは実行可能なトランザクションに含める必要があり、また、省略可能なフィールドでも、`LastLedgerSequence`などは含めることを強く推奨します。 Some other language-specific notes:

- [`autofill()`メソッド](https://js.xrpl.org/classes/Client.html#autofill)は、トランザクションの残りのフィールドに適切なデフォルトを自動的に入力します。 In TypeScript, you can also use the transaction models like `xrpl.Payment` to enforce the correct fields.
- With `xrpl-py` for Python, you can use the models in `xrpl.models.transactions` to construct transactions as native Python objects.
- With xrpl4j for Java, you can use the model objects in the `xrpl4j-model` module to construct transactions as Java objects.
    - Unlike the other libraries, you must provide the account `sequence` and the `signingPublicKey` of the source account of a `Transaction` at the time of construction, as well as a `fee`.

Here's an example of preparing the above payment:

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/send-xrp/js/send-xrp.js", start_with="// Prepare", end_before="// Sign", language="js" ) }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", start_with="# Prepare", end_before="# Sign", language="py" ) }}

_Java_

{{ include_code("_code-samples/send-xrp/java/SendXrp.java", start_with="// Prepare", end_before="// Sign", language="java") }}

<!-- MULTICODE_BLOCK_END -->

{{ start_step("Prepare") }}
<div class="input-group mb-3">
  <div class="input-group-prepend">
    <span class="input-group-text">送金する額： </span>
  </div>
  <input type="number" class="form-control" value="22" id="xrp-amount"
  aria-label="Amount of XRP, as a decimal" aria-describedby="xrp-amount-label"
  min=".000001" max="100000000000" step="any">
  <div class="input-group-append">
    <span class="input-group-text" id="xrp-amount-label"> XRP</span>
  </div>
</div>
<button id="prepare-button" class="btn btn-primary previous-steps-required">サンプルトランザクションを準備する</button>
<div class="output-area"></div>
{{ end_step() }}


### {{n.next()}}. トランザクションの指示への署名

Signing a transaction uses your credentials to authorize the transaction on your behalf. The input to this step is a completed set of transaction instructions (usually JSON), and the output is a binary blob containing the instructions and a signature from the sender.

- xrpl.jsの[Wallet.sign()メソッド](https://js.xrpl.org/classes/Wallet.html#sign)を使用して、トランザクションに署名します。
- **Python:** Use the [`xrpl.transaction.safe_sign_transaction()` method](https://xrpl-py.readthedocs.io/en/latest/source/xrpl.transaction.html#xrpl.transaction.safe_sign_transaction) with a model and `Wallet` object.
- **Java:** Use a [`SignatureService`](https://javadoc.io/doc/org.xrpl/xrpl4j-crypto-core/latest/org/xrpl/xrpl4j/crypto/signing/SignatureService.html) instance to sign the transaction. For this tutorial, use the [`SingleKeySignatureService`](https://javadoc.io/doc/org.xrpl/xrpl4j-crypto-bouncycastle/latest/org/xrpl/xrpl4j/crypto/signing/SingleKeySignatureService.html).

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/send-xrp/js/send-xrp.js", start_with="// Sign", end_before="// Submit", language="js" ) }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", start_with="# Sign", end_before="# Submit", language="py" ) }}

_Java_

{{ include_code("_code-samples/send-xrp/java/SendXrp.java", start_with="// Sign", end_before="// Submit", language="java" ) }}

<!-- MULTICODE_BLOCK_END -->

署名処理の結果は、署名を含むトランザクションオブジェクトになります。 通常、XRP Ledger APIは、署名済みトランザクションがトランザクションの正規の[バイナリーフォーマット](serialization.html)（「ブロブ」と呼ばれる）の16進数表現になることを想定しています。

- 署名APIは、トランザクションのID、つまり識別用ハッシュを返します。 この識別用ハッシュは、後でトランザクションを検索する際に使用します。 識別用ハッシュは、このトランザクションに固有の64文字の16進文字列です。
- In `xrpl-py`, you can get the transaction's hash in the response to submitting it in the next step.
- In xrpl4j, `SignatureService.sign` returns a `SignedTransaction`, which contains the transaction's hash, which you can use to look up the transaction later.

{{ start_step("Sign") }}
<button id="sign-button" class="btn btn-primary previous-steps-required">サンプルトランザクションに署名する</button>
<div class="output-area"></div>
{{ end_step() }}


### {{n.next()}}. 署名済みブロブの送信

Now that you have a signed transaction, you can submit it to an XRP Ledger server, which relays it through the network. It's also a good idea to take note of the latest validated ledger index before you submit. The earliest ledger version that your transaction could get into as a result of this submission is one higher than the latest validated ledger when you submit it. Of course, if the same transaction was previously submitted, it could already be in a previous ledger. (It can't succeed a second time, but you may not realize it succeeded if you aren't looking in the right ledger versions.)

- **JavaScript:** Use the [`submitAndWait()` method of the Client](https://js.xrpl.org/classes/Client.html#submitAndWait) to submit a signed transaction to the network and wait for the response, or use [`submitSigned()`](https://js.xrpl.org/classes/Client.html#submitSigned) to submit a transaction and get only the preliminary response.
- **Python:** Use the [`xrpl.transaction.send_reliable_submission()` method](https://xrpl-py.readthedocs.io/en/latest/source/xrpl.transaction.html#xrpl.transaction.send_reliable_submission) to submit a transaction to the network and wait for a response.
- **Java:** Use the [`XrplClient.submit(SignedTransaction)` method](https://javadoc.io/doc/org.xrpl/xrpl4j-client/latest/org/xrpl/xrpl4j/client/XrplClient.html#submit(org.xrpl.xrpl4j.crypto.signing.SignedTransaction)) to submit a transaction to the network. Use the [`XrplClient.ledger()`](https://javadoc.io/doc/org.xrpl/xrpl4j-client/latest/org/xrpl/xrpl4j/client/XrplClient.html#ledger(org.xrpl.xrpl4j.model.client.ledger.LedgerRequestParams)) method to get the latest validated ledger index.

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/send-xrp/js/send-xrp.js", start_with="// Submit", end_before="// Wait", language="js" ) }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", start_with="# Submit", end_before="# Wait", language="py") }}

_Java_
{{ include_code("_code-samples/send-xrp/java/SendXrp.java", start_with="// Submit", end_before="// Wait", language="java" ) }}

<!-- MULTICODE_BLOCK_END -->

このメソッドは、ローカルでトランザクションを適用しようと試みたときの**一時的な**結果を返します。 この結果は、トランザクションが検証済みレジャーに含まれた時点で変わる_可能性があります_。 当初は成功していたトランザクションが最終的に失敗となったり、当初失敗していたトランザクションが最終的に成功する場合があります。 しかしながら、一時的な結果はほとんどの場合は最終結果と一致するため、ここで`tesSUCCESS`が表示されたらひとまず安心しても問題ありません。 😁

他の結果が表示された場合は、以下の点を確認します。

- 送信元および送信先の正しいアドレスを使用しているか。
- トランザクションの他のフィールドへの入力漏れ、ステップのスキップ、その他の入力ミスがないか。
- トランザクションの送信に必要なTest Net XRPが十分にあるか。 送金できるXRPの額は、[必要準備金](reserves.html)によって制限されています。 現時点では、20XRPに加えて、レジャー内に保有している各「オブジェクト」につき5XRPずつ追加となります。 （Test Net Faucetを使用して新しいアドレスを生成した場合は、保有するオブジェクトはありません。 ）
- テストネットワークのサーバーに接続しているか。

他の可能性については、[トランザクション結果](transaction-results.html)の完全なリストを参照してください。

{{ start_step("Submit") }}
<button id="submit-button" class="btn btn-primary previous-steps-required" data-tx-blob-from="#signed-tx-blob" data-wait-step-name="Wait">サンプルトランザクションを送信する</button>
<div class="loader collapse"><img class="throbber" src="assets/img/xrp-loader-96.png"> 送信中...</div>
<div class="output-area"></div>
{{ end_step() }}


### {{n.next()}}. トランザクションステータスの確認

ほとんどのトランザクションは送信後の次のレジャーバージョンに承認されます。 つまり、4～7秒でトランザクションの結果が最終的なものになる可能性があります。 XRP Ledgerがビジーになっているか、ネットワーク接続の品質が悪いためにトランザクションをネットワーク内で中継する処理が遅延した場合は、トランザクション確定までにもう少し時間がかかることがあります。 （トランザクションの有効期限を設定する方法については、[信頼できるトランザクションの送信](reliable-transaction-submission.html)を参照してください。 ）

- **JavaScript:**  If you used the [`.submitAndWait()` method](https://js.xrpl.org/classes/Client.html#submitAndWait), you can wait until the returned Promise resolves. Other, more asynchronous approaches are also possible.

- **Python:** If you used the [`xrpl.transaction.send_reliable_submission()` method](https://xrpl-py.readthedocs.io/en/latest/source/xrpl.transaction.html#xrpl.transaction.send_reliable_submission), you can wait for the function to return. Other approaches, including asynchronous ones using the WebSocket client, are also possible.

- **Java** Poll the [`XrplClient.transaction()` method](https://javadoc.io/doc/org.xrpl/xrpl4j-client/latest/org/xrpl/xrpl4j/client/XrplClient.html#transaction(org.xrpl.xrpl4j.model.client.transactions.TransactionRequestParams,java.lang.Class)) to see if your transaction has a final result. Periodically check that the latest validated ledger index has not passed the `LastLedgerIndex` of the transaction using the [`XrplClient.ledger()`](https://javadoc.io/doc/org.xrpl/xrpl4j-client/latest/org/xrpl/xrpl4j/client/XrplClient.html#ledger(org.xrpl.xrpl4j.model.client.ledger.LedgerRequestParams)) method.

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/send-xrp/js/send-xrp.js", start_with="// Wait", end_before="// Check", language="js" ) }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", start_with="# Wait", end_before="# Check", language="py") }}

_Java_

{{ include_code("_code-samples/send-xrp/java/SendXrp.java", start_with="// Wait", end_before="// Check", language="java" ) }}

<!-- MULTICODE_BLOCK_END -->

{{ start_step("Wait") }}
{% include '_snippets/interactive-tutorials/wait-step.ja.md' %}
{{ end_step() }}


### {{n.next()}}. Check Transaction Status

トランザクションが行った内容を正確に把握するために、トランザクションが検証済みレジャーバージョンに記録されたときにトランザクションの結果を調べる必要があります。

- **JavaScript:** Use the response from `submitAndWait()` or call the \[tx method\]\[\] using [`Client.request()`](https://js.xrpl.org/classes/Client.html#request).

    **Tip:** In **TypeScript** you can pass a [`TxRequest`](https://js.xrpl.org/interfaces/TxRequest.html) to the [`Client.request()`](https://js.xrpl.org/classes/Client.html#request) method.

- **Python:** Use the response from `send_reliable_submission()` or call the [`xrpl.transaction.get_transaction_from_hash()` method](https://xrpl-py.readthedocs.io/en/latest/source/xrpl.transaction.html#xrpl.transaction.get_transaction_from_hash). (See the [tx method response format](tx.html#response-format) for a detailed reference of the fields this can contain.)

- **Java:** Use the [`XrplClient.transaction()`](https://javadoc.io/doc/org.xrpl/xrpl4j-client/latest/org/xrpl/xrpl4j/client/XrplClient.html#transaction(org.xrpl.xrpl4j.model.client.transactions.TransactionRequestParams,java.lang.Class)) method to check the status of a transaction.

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

{{ include_code("_code-samples/send-xrp/js/send-xrp.js", start_with="// Check", end_before="// End of", language="js" ) }}

_Python_

{{ include_code("_code-samples/send-xrp/py/send-xrp.py", start_with="# Check", language="py") }}

_Java_

{{ include_code("_code-samples/send-xrp/java/SendXrp.java", start_with="// Check", language="java" ) }}

<!-- MULTICODE_BLOCK_END -->

**注意:** APIは、まだ検証されていないレジャーバージョンからの暫定的な結果を返す場合があります。 例えば、`rippled` APIの\[txメソッド\]\[\]を使用した場合は、応答内の`"validated": true`を探して、データが検証済みレジャーバージョンからのものであることを確認してください。 検証済みレジャーバージョンからのものではないトランザクション結果は、変わる可能性があります。 詳細は、[結果のファイナリティー](finality-of-results.html)を参照してください。

{{ start_step("Check") }}
<button id="get-tx-button" class="btn btn-primary previous-steps-required">トランザクションステータスを確認する</button>
<div class="output-area"></div>
{{ end_step() }}


## Differences for Production

本番XRP LedgerでXRPを送金する場合も、大部分の手順は同じです。 ただし、必要なセットアップでは重要な相違点がいくつかあります。

- [実際のXRPは無料で取得できません。](#getting-a-real-xrp-account)
- [本番XRP Ledgerネットワークと同期されているサーバーに接続する必要があります。](#connecting-to-the-production-xrp-ledger)

### 実際のXRPアカウントの取得

このチュートリアルでは、Test Net XRPがすでに資金供給されているアドレスをボタンで取得しましたが、それが可能だったのはTest Net XRPに何の価値もないからです。 実際のXRPでは、XRPを所有している他者からXRPを入手する必要があります。 （たとえば、取引所で購入する方法など。 ）[xrpl.jsのWallet()クラス](https://js.xrpl.org/classes/Wallet.html)を使用して、本番またはTestnetで機能するアドレスとシークレットを生成できます。

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

```js
const wallet = new xrpl.Wallet()
console.log(wallet.address) // Example: rGCkuB7PBr5tNy68tPEABEtcdno4hE6Y7f
console.log(wallet.seed) // Example: sp6JS7f14BuwFY8Mw6bTtLKWauoUs
```

_Python_

```py
from xrpl.wallet import Wallet
my_wallet = Wallet.create()
print(my_wallet.classic_address) # Example: rGCkuB7PBr5tNy68tPEABEtcdno4hE6Y7f
print(my_wallet.seed)            # Example: sp6JS7f14BuwFY8Mw6bTtLKWauoUs
```

_Java_

```java
WalletFactory walletFactory = DefaultWalletFactory.getInstance();
SeedWalletGenerationResult generationResult = walletFactory.randomWallet(false);
Wallet wallet = generationResult.wallet();
System.out.println(wallet.classicAddress()); // Example: rGCkuB7PBr5tNy68tPEABEtcdno4hE6Y7f
System.out.println(generationResult.seed()); // Example: sp6JS7f14BuwFY8Mw6bTtLKWauoUs
```

<!-- MULTICODE_BLOCK_END -->

**警告:** ローカルマシンで安全な方法で生成したアドレスとシークレットのみを使用してください。 別のコンピューターでアドレスとシークレットを生成して、ネットワーク経由でそれらを自分に送信した場合は、ネットワーク上の他の人がその情報を見ることができる可能性があります。 If they do, they'll have as much control over your XRP as you do. また、Test Netと本番で同じアドレスを使用しないことも推奨します。 指定したパラメーターによっては、一方のネットワークに向けて作成したトランザクションが、もう一方のネットワークでも実行可能になるおそれがあるためです。

アドレスとシークレットを生成しても、直接XRPを入手できるわけではありません。 単に乱数を選択しているだけです。 また、そのアドレスでXRPを受け取って[アカウントに資金供給](accounts.html#アカウントの作成)する必要があります。 XRPを取得する方法として最も一般的なのは、取引所から購入し、所有しているアドレスに入れる方法です。 詳細は、[XRP Overview](xrp-overview.html)を参照してください。

### 本番XRP Ledgerへの接続

XRP Ledgerと同期しているサーバーを指定する必要があります。 多くの場合は公開サーバーを、以下のスニペットなどで使用できます。

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

```js
const xrpl = require('xrpl')
const api = new xrpl.Client('wss://xrplcluster.com')
api.connect()
```

_Python_

```py
from xrpl.clients import JsonRpcClient
client = JsonRpcClient("https://xrplcluster.com")
```

_Java_

```java
final HttpUrl rippledUrl = HttpUrl.get("https://xrplcluster.com");
XrplClient xrplClient = new XrplClient(rippledUrl);
```

<!-- MULTICODE_BLOCK_END -->

If you [install `rippled`](install-rippled.html) yourself, it connects to the production network by default. (You can also [configure it to connect to the test net](connect-your-rippled-to-the-xrp-test-net.html) instead.) After the server has synced (typically within about 15 minutes of starting it up), you can connect to it locally, which has [various benefits](xrpl-servers.html). The following example shows how to connect to a server running the default configuration:

<!-- MULTICODE_BLOCK_START -->

_JavaScript_

```js
const xrpl = require('xrpl')
const api = new xrpl.Client('ws://localhost:6006')
api.connect()
```

_Python_

```py
from xrpl.clients import JsonRpcClient
client = JsonRpcClient("http://localhost:5005")
```

_Java_

```java
final HttpUrl rippledUrl = HttpUrl.get("http://localhost:5005");
XrplClient xrplClient = new XrplClient(rippledUrl);
```

<!-- MULTICODE_BLOCK_END -->

**ヒント:** ローカル接続では、WebSocketプロトコルのTLSで暗号化されたバージョン（`wss`）ではなく、暗号化されていないバージョン（`ws`）を使用します。 この方式は、通信が同じマシンの中だけで行われてマシンの外に出て行かないという点で安全で、TLS証明書が不要であるため設定が簡単です。 外部ネットワークとの接続では、必ず`wss`を使用してください。

## 次のステップ

このチュートリアルを完了後は、以下を試してみてください。

- Test Netを使用してXRPの送金をテストします。
- [Trade in the Decentralized Exchange](trade-in-the-decentralized-exchange.html).
- 本番システム向けに[信頼できるトランザクションの送信](reliable-transaction-submission.html)を構築する
- [xrpl.jsリファレンス](https://js.xrpl.org/)を参照して、XRP Ledgerの全機能を確認する
- [アカウント設定](manage-account-settings.html)をカスタマイズする
- [トランザクションのメタデータ](transaction-metadata.html)にトランザクションの結果の詳細がどのように記述されているかを知る
- escrowやPayment Channelなどの[複雑な支払いタイプ](complex-payment-types.html)について調べる
- [XRP Ledgerビジネス](xrp-ledger-businesses.html)のベストプラクティスを読む



<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
