---
html: nftoken-auctions.html
parent: non-fungible-tokens.html
blurb: NFTのMintを他のアカウントに代行してもらうことができます。
labels:
  - 非代替性トークン, NFT
---

# NFTオークションの実施

オークションの運営にはいくつかの方法があり、それぞれにメリットとデメリットがあります。

## XRPL外でオークションを行い、XRPL上で取引を成立させる

このフローが最もわかりやすいと思います。 `NFTokenOffer`オブジェクトは常にその作成者によってキャンセルされる可能性があるため、拘束力のあるオファーを実装することはできないことに注意してください。

1. 入札内容を非公開のデータベースに保存します。
2. You take a cut of the winning bid.
3. 買い手/売り手へXRPLトランザクションを送信し、取引を完了させます。

## Run the Auction in Brokered Mode, with a Reserve

Run the auction in brokered mode, as an auction with a reserve.

![Auction in Brokered Mode with a Reserve](img/nft-auction1.png "Auction in Brokered Mode with a Reserve")

1. 売り手はNFTを作成し，`NFTokenCreateOffer`を用い，ブローカーアカウントを宛先に設定して，オークションの最低落札価格を設定します。
1. 入札者は`NFTokenCreateOffer`を使ってオファーを出し、ブローカーアカウントを宛先として設定します。
1. ブローカーは落札した入札を選択し、`NFTokenAcceptOffer`を使用して取引を成立させ、ブローカー手数料を徴収します。 その後、ブローカーは`NFTokenCancelOffer`を使って競り負けた入札をキャンセルします。

**短所:**

- ブローカー手数料を含め、すべてのオークションはXRPLで行われます。
- The seller represents their reserve price on-chain.
- This is _close_ to a binding offer, from the buyside.

**Cons:**

- 売り手とブローカーの間には、ブローカーが事前に合意したレート以上を取らないという暗黙の信頼関係が必要です。 最低落札価格が1XRPで、落札価格が1000XRPだった場合、ブローカーが999XRPを手数料として徴収し、売り手には最低落札価格の利益しか残らないということが起き得ます。 これを防ぐオンチェーンの仕組みは存在しません。

このデメリットの大きな軽減要素は、もしこのような行動が起これば、ブローカーは市場シェアをすべて失うことになるため、売り手はこの点を理解すべきです。

## Run the Auction in Brokered Mode, without a Reserve.

この3つのうち、最も複雑なワークフローとなります。

![Auction in Brokered Mode without a Reserve](img/nft-auction2.png "Auction in Brokered Mode without a Reserve")

1. 売り手は`NFTokenMint`を使用してNFTを作成します。
1. 入札者は`NFTokenCreateOffer`を使って、ブローカーを宛先としてオファーを出します。
1. ブローカーは落札者を選択し、手数料として徴収する金額を差し引いた後、`NFTokenCreateOffer`を介してこの金額で売却のための署名を売り手に要求します。
1. 売り手は要求されたオファーに署名し、宛先としてブローカーを設定します。
1. ブローカーは`NFTokenAcceptOffer`を使って取引を完了させ、ブローカー手数料を受け取ります。
1. ブローカーは`NFTokenCancelOffer`を使って競り負けた入札をキャンセルします。

**短所:**

- このフローは参加者間のトラストが全く必要ないため、ほとんどの人がブロックチェーン上で期待するオプションとなっています。
- 売り手はブローカーが手数料をいくら徴収するか正確に把握でき、チェーン上でそれに同意しなければなりません。

**Cons:**

- オークション終了後は、売り手が最終入札額とブローカー手数料の額に合意することが売却の条件となります。 つまり、売り手はオークション終了後に出品を取り消すことができ、また、売り手は注意散漫や通知を見逃すことで、決済を遅らせてしまうことがありえます。
- オークション終了後、出品者は落札を拒否し、他の出品者に売却することができてしまいます。
