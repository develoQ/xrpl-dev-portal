XRP Ledgerでは、金融機関は秘密鍵の漏えいに関連するリスクを最小限に抑えるために、複数のXRP Ledgerアドレスを使用するのが一般的です。 Rippleでは、以下のように役割を分離することを強くお勧めします。

* One **issuing address**, also known as a "cold wallet." このアドレスは、レジャーでの金融機関の会計上の関係の中心となるものですが、トランザクションの送信は可能な限り少なく抑えます。 <!-- STYLE_OVERRIDE: cold wallet, wallet -->
* 1つ以上の**運用アドレス**。 「ホットウォレット」とも呼ばれます。 インターネットに接続した自動システムが、これらのアドレスへの秘密鍵を使用して、顧客やパートナーへの送金といった日常業務を実施します。 <!-- STYLE_OVERRIDE: hot wallet, wallet -->
* Optional **standby addresses**, also known as "warm wallets." 信頼できる人間のオペレーターが、これらのアドレスを使用して運用アドレスに送金します。 <!-- STYLE_OVERRIDE: warm wallet, wallet -->
