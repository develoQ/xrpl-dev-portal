---
html: about-canceling-a-transaction.html
parent: consensus-network.html
blurb: 送信済みのトランザクションのキャンセルがいつどのように可能か説明します。
labels:
  - トランザクション送信
---

# トランザクションの取消しについて

XRP Ledgerの重要かつ意図的な機能の1つに、トランザクションが検証済みレジャーに組み込まれると、即時に最終的なトランザクションになるという機能があります。

If a transaction has _not_ yet been included in a validated ledger, it may be possible to effectively cancel it by sending another transaction from the same sending address with the same `Sequence` value. 置換トランザクションが何もしないようにしたい場合は、オプションを指定せずに\[AccountSetトランザクション\]\[\]を送信します。

**Caution:** There is no guaranteed way to cancel a valid transaction after it has been distributed to the network. The process described here may or may not work depending on factors including how busy the network is, the network topology, and the [transaction cost](transaction-cost.html) of the proposed transaction.

If the transaction has already been distributed to the network and proposed as a [candidate transaction](consensus.html#consensus-1) in servers' consensus proposals, it may be too late to cancel. It is more likely that you can successfully cancel a transaction that is [queued](transaction-queue.html) or is stuck "in limbo" because its [transaction cost](transaction-cost.html) is not high enough to meet the network's current requirements. In this case, the replacement transaction can either do nothing, or do the same thing as the transaction to be canceled. The replacement transaction is more likely to succeed if its transaction cost is higher.

たとえば、シーケンス番号が11、12、13の3つのトランザクションを送信しようとしたときに、トランザクション11が何らかの理由で失われるか、またはトランザクション11にネットワークに伝達するのに十分な[トランザクションコスト](transaction-cost.html)がない場合には、オプションを指定せずシーケンス番号11を指定したAccuontSetトランザクションを送信すれば、トランザクション11を取り消せます。 このトランザクションは（新しいトランザクション11のトランザクションコストを消却する以外は）何も行いませんが、トランザクション12と13を有効にできます。

この方法により、異なるシーケンス番号のトランザクションの内容が実質的に重複することを防げるため、トランザクション12と13の番号を変更して再送信するよりも望ましい方法です。

つまり、オプションが指定されていないAccountSetトランザクションは標準的な「[no-op](http://en.wikipedia.org/wiki/NOP)」トランザクションです。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
