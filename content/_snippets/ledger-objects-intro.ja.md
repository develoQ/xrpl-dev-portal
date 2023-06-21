各[レジャー](ledgers.html)の状態ツリーは**レジャーオブジェクト**のセットで構成されており、それらが総合して共有レジャーのすべての設定、残高、関係を表します。 To store or retrieve an object in the state data, the protocol uses that object's unique **[Ledger Object ID](ledger-object-ids.html)**.

rippledサーバーが互いに通信するために使用する[ピアプロトコル](peer-protocol.html)では、レジャーオブジェクトは生[バイナリーフォーマット](serialization.html)で表されます。 rippled APIでは、レジャーオブジェクトはJSONオブジェクトとして表されます。

A ledger object's data fields depend on the type of object; the XRP Ledger supports the following types:
