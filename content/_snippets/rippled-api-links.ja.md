<!--{# Links within the dev portal #}-->


<!-- API object types -->
{% set ledger_entries = [ "AccountRoot", "Amendments", "AMM", "Check", "DepositPreauth", "DirectoryNode", "Escrow", "FeeSettings", "LedgerHashes", "NegativeUNL", "NFTokenOffer", "NFTokenPage", "Offer", "PayChannel", "RippleState", "SignerList", "Ticket" ] %}

{% for ledger_entry in ledger_entries %}
{% if ledger_entry in ["amendments", "depositpreauth", "escrow"] %}
  {# objects where the naive URL is already taken #}
  {% if target.lang == "ja" %}
  {% endif %}
{% else %}
  {% if target.lang == "ja" %}
  {% endif %}
{% endif %}
{% endfor %}


<!--{# Links to external sites #}-->


<!--{# rippled API methods #}-->
{% set api_methods = [ "account_channels", "account_currencies", "account_info", "account_lines", "account_nfts", "account_objects", "account_offers", "account_tx", "amm_info", "book_offers", "can_delete", "channel_authorize", "channel_verify", "connect", "consensus_info", "crawl_shards", "deposit_authorized", "download_shard", "feature", "fee", "fetch_info", "gateway_balances", "get_counts", "json", "ledger", "ledger_accept", "ledger_cleaner", "ledger_closed", "ledger_current", "ledger_data", "ledger_entry", "ledger_request", "log_level", "logrotate", "manifest", "nft_buy_offers", "nft_info", "nft_sell_offers", "noripple_check", "path_find", "peer_reservations_add", "peer_reservations_del", "peer_reservations_list", "peers", "ping", "print", "random", "ripple_path_find", "server_info", "server_state", "sign", "sign_for", "stop", "submit", "submit_multisigned", "subscribe", "transaction_entry", "tx", "tx_history", "unsubscribe", "validation_create", "validation_seed", "validator_info", "validator_list_sites", "validators", "wallet_propose" ] %}

{% for method in api_methods %}
{% if target.lang == "ja" %}
{% endif %}
{% endfor %}

<!--{# Amendment links #}-->
{% set amendment_names = [ "Checks", "CheckCashMakesTrustLine", "CryptoConditions", "CryptoConditionsSuite", "DeletableAccounts", "DepositAuth", "DepositPreauth", "DisallowIncoming", "EnforceInvariants", "Escrow", "ExpandedSignerList", "FeeEscalation", "fix1201", "fix1368", "fix1373", "fix1512", "fix1513", "fix1515", "fix1523", "fix1528", "fix1543", "fix1571", "fix1578", "fix1623", "fixCheckThreading", "fixMasterKeyAsRegularKey", "fixNFTokenDirV1", "fixPayChanRecipientOwnerDir", "fixRemoveNFTokenAutoTrustLine", "fixQualityUpperBound", "fixTakerDryOfferRemoval", "fixTrustLinesToSelf", "Flow", "FlowCross", "FlowV2", "ImmediateOfferKilled", "MultiSign", "MultiSignReserve", "NegativeUNL", "NonFungibleTokensV1", "NonFungibleTokensV1_1", "OwnerPaysFee", "PayChan", "RequireFullyCanonicalSig", "SHAMapV2", "SortedDirectories", "SusPay", "TicketBatch", "Tickets", "TickSize", "TrustSetAuth", "XRPFees" ] %}

{% for amd in amendment_names %}
{% endfor %}
