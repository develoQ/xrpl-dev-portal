The prerequisites for cashing a check are the same whether you are cashing it for an exact amount or a flexible amount.

- 現在レジャーに記録されているCheckオブジェクトのIDが必要です。
    - 例えば、以下の例では、あるCheckのIDとして`838766BA2B995C00744175F69A1B11E32C3DBC40E64801A4056FCBD657F57334`を使用していますが、各ステップをご自分で行う際には、異なるIDを使用する必要があります。
- Checkに記載されている受取人の**アドレス**と**秘密鍵**。 このアドレスは、Checkオブジェクトの`Destination`アドレスと一致している必要があります。
- If the Check is for a [token](tokens.html), you (the recipient) must have a [trust line](trust-lines-and-issuing.html) to the issuer. Your limit on that trust line must be high enough to hold your previous balance plus the amount you would receive.
- [トランザクションに安全に署名できる手段](set-up-secure-signing.html)。
- XRP Ledgerに接続できる[クライアントライブラリ](client-libraries.html)か、それとも[HTTPライブラリ、WebSocketライブラリなど](get-started-using-http-websocket-apis.html)。
