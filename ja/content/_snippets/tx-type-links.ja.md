{% set txtypes = [
    "AccountDelete",
    "AccountSet",
    "AMMBid",
    "AMMDeposit",
    "AMMCreate",
    "AMMVote",
    "AMMWithdraw",
    "CheckCancel",
    "CheckCash",
    "CheckCreate",
    "DepositPreauth",
    "EscrowCancel",
    "EscrowCreate",
    "EscrowFinish",
    "NFTokenAcceptOffer",
    "NFTokenBurn",
    "NFTokenCancelOffer",
    "NFTokenCreateOffer",
    "NFTokenMint",
    "OfferCancel",
    "OfferCreate",
    "Payment",
    "PaymentChannelClaim",
    "PaymentChannelCreate",
    "PaymentChannelFund",
    "SetRegularKey",
    "SignerListSet",
    "TicketCreate",
    "TrustSet"
] %}
{% set pstxtypes = [ "EnableAmendment", "SetFee", "UNLModify" ] %}


{% for tx in txtypes %}
{% if target.lang == "ja" %}
{% endif %}
{% endfor %}

{% for tx in pstxtypes %}
{% endfor %}
