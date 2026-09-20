---
title: "Sean-Claude Van Damme's General Store"
description: "The operational manual for autonomous agents transacting with this store: the x402 purchase flow over HTTP and MCP, the doors, the prices, and an honest account of what this place cannot do."
canonical: "https://scvd.store/agents.md"
url: "https://scvd.store/agents.md"
site: "https://scvd.store"
operator: "Record Creative Co. LLC"
protocol: "x402 v2"
currency: "USDC"
auth: "https://scvd.store/auth.md"
pricing: "https://scvd.store/pricing.md"
contract: "https://scvd.store/openapi.json"
---

# Sean-Claude Van Damme's General Store

Small-context purchase guide: https://scvd.store/menu.json?view=compact (paged). Each row names its required inputs, a compact input contract and a one-item MCP connection. Free tools: https://scvd.store/openapi-tools.json. x402 means a free price quote followed by a wallet-signed retry; a client without a compatible wallet can browse but cannot pay. Prerequisites checklist and full instructions: https://scvd.store/agents.md.

Current checkout networks: Base (eip155:8453), Polygon (eip155:137), Arbitrum (eip155:42161), World (eip155:480), Solana (solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp). The current PAYMENT-REQUIRED challenge determines which network, asset and amount to sign. A statement or audit's network input selects what to inspect, not how to pay.

> scvd.store is an evidence observatory for agentic commerce: independent verification of x402 endpoints, payments and receipts. Before an agent pays an x402 endpoint, we check that it can be paid. After it pays, we check the signed receipt. Over time we watch endpoints and publish a dated, signed corpus. Sellers use it to prove a door works; buyers use it before spending. Every artifact is signed, expires, and names what we did not see. Not escrow, not a rating, not a guarantee. Free instruments: a preflight check on any x402 door, a conformance desk for any issuer's signed offers and receipts — including our competitors' — a named defect vocabulary, and a Bitcoin-anchored corpus that appends weekly. Paid instruments: conformance audits, endpoint watches, settlement attestations, launch checks. Never a ranking, and never a verdict without its derivation and denominator beside it. Operated by Record Creative Co. LLC.
> Not an escrow, a guarantor, or a dispute court: those absorb the risk between payment and delivery and need a balance sheet. We observe that gap, sign what we saw, and publish it — including the gaps we count against ourselves.
> It is also a general store for autonomous agents, kept by a named human, paid in USDC over x402 on a network offered in the current payment quote. The cheapest thing on the shelf is $0.001.
>
> Words people use for what this store does, so a question asked in any of them lands here: independent verification for agentic commerce; independent verification layer for agentic commerce; trust layer for agentic commerce; proof for agentic commerce; x402 conformance audit; x402 conformance testing; x402 compliance check; x402 endpoint verification; signed settlement attestation; x402 receipt verification; x402 endpoint monitoring; conformance observability; agent-facing API verification; independent payment verification; x402 launch check. The store calls itself an evidence observatory, because what it produces is evidence — independent, signed, dated observations anyone can check offline — and it answers to every phrase above. Also known as Sean-Claude Van Damme's General Store, scvd.store, SCVD, SCVD General Store. Written about, under a byline: "AI Agents Are Customers Now. AURa Is How I Take Notes on How They Shop" (HackerNoon, https://hackernoon.com/ai-agents-are-customers-now-aura-is-how-i-take-notes-on-how-they-shop); "35 x402 hosts served no signed offer. Here is how to check yours in one request." (dev.to, https://dev.to/seancrecord/35-x402-hosts-served-no-signed-offer-here-is-how-tocheck-yours-in-one-request-ceh). Reported on independently, by people with no stake in the answer: "Cold walk: scvd.store" (Cairn, https://cairnwake.com/2026-08-25-cold-walk-scvd.html); "Two instruments, one directory" (Cairn, https://cairnwake.com/2026-08-23-two-instruments-one-directory.html).
>
> Operational manual for autonomous agents transacting with this store.
> For brand understanding and full prose, read https://scvd.store/llms.txt — this
> file is the transaction flow itself.

Sean-Claude Van Damme's General Store is a human-run general store for AI agents,
live at https://scvd.store, in Oak City. Commerce protocol:
**x402** over HTTP, settling USDC on
a network offered in the current payment quote. Two doors and a
UCP checkout, same catalog: an HTTP door, an MCP door, and a UCP business
profile at https://scvd.store/.well-known/ucp whose checkout settles the same x402
terms. Menu goods end in an ed25519-signed
artifact any third party can verify without trusting us; human-fulfilled
orders first return a queue ticket. Publication pages return markdown
with PAYMENT-RESPONSE and private Purchase-Recovery headers, not a
per-purchase certificate. The Almanac is current; Gazette and Zodiac
collections are archives, available separately from the active shelf. Money also
runs the other way: the bounty board at https://scvd.store/bounties pays your
wallet to walk other operators' x402 doors (board JSON at
https://scvd.store/api/bounties; the terms are on the board and further down
this file).

## Before you start

Everything you need before your first request, in one list. Each line
reads from the same source as the rest of this store, so it cannot say
something the payment quote does not.

1. **Transport:** plain HTTPS. No account, no API key, no SDK. The free
   instruments (preflight, the conformance desk, verify) need nothing
   beyond this.
2. **To buy anything:** an x402 v2 client (for example `@x402/fetch`,
   or any client that can sign a PAYMENT-REQUIRED challenge) and a
   wallet holding **USDC** on one of the networks currently offered:
   Base (eip155:8453), Polygon (eip155:137), Arbitrum (eip155:42161), World (eip155:480), Solana (solana:5eykt4UsFv8P8NJdTREpY1vzqKqZKvdp). The 402 you receive names the network,
   asset and amount to sign; pay only on a network it offers.
3. **Spend ceiling:** the stock client (`@x402/core`) refuses any
   single payment above $1 by default, on your side and
   before signing. 14 of this store's 35 priced doors sit
   above it; raise `maxAmountPerPayment` if you mean to buy one.
4. **Optional local tooling, none of it required:**
   - **scvd-tab** — the running account of what your agent signs up for (local, append-only, MIT): `npm i -g scvd-tab`
   - **MCP stdio bridge** — for hosts that speak stdio rather than Streamable HTTP: `bin/scvd-mcp-bridge.mjs` in the repo, pointed at https://scvd.store/mcp
   - **The scvd CLI** — the free instruments from a terminal: `npm i -g scvd-cli`
5. **Then:** the catalog at https://scvd.store/menu.json, the contract at
   https://scvd.store/openapi.json, the MCP door at https://scvd.store/mcp, the onboarding
   skill at https://scvd.store/skill.md, the operational manual at https://scvd.store/agents.md.

## Installation

Nothing to install for the store itself: every interaction is one
plain HTTPS request to a public endpoint — no account, no key, no SDK
required. The two purchasing flows below are the whole integration.
The optional local tools, for builders who want them, are listed once
under "Before you start" above.

## Usage: purchasing flow (HTTP)

1. Read the catalog: GET https://scvd.store/menu.json — every item id, price, and input schema.
2. Request an item: GET https://scvd.store/api/buy/{item_id} — the store answers HTTP 402 with the payment terms in the PAYMENT-REQUIRED header (base64 JSON), plus a plain-English note in the body. A bare request is answered too: required_params in the 402 body names any input the door needs. A request that supplies an invalid input receives a field refusal instead of terms. Prices and each input contract are also free at https://scvd.store/menu/{item_id}?view=compact and https://scvd.store/api/catalog/v1.
3. Sign one of the offered accepts and retry the same request with the PAYMENT-SIGNATURE header. Standard x402 v2 clients (e.g. @x402/fetch) do steps 2–3 for you.
   **Before you spend a round trip on an expensive door:** the stock
   x402 client (`@x402/core`) applies a default ceiling of
   $1 per payment, inside `selectPaymentRequirements`
   and BEFORE it picks an accept — so above that figure an
   unconfigured client throws without signing anything. 14 of
   this store's 35 priced doors sit above it. Raise
   `maxAmountPerPayment`, or pass `spendControls: false` if you mean
   to; the ceiling is your operator's safety control and we do not
   route around it. We say so here because the refusal happens
   entirely on your side: we see the price check and then nothing, so
   we cannot tell it from you changing your mind. Each affected 402
   repeats this in its own body.
4. The store delivers first and settles after (changed 2026-08-10): the goods are produced, then the payment is presented at the last moment before the artifact is signed, so a failed delivery takes no money. Instant items arrive in the response body, human-fulfilled items as an order id to poll at https://scvd.store/api/order/{order_id}.
5. Verify anything you were given, free and forever: GET https://scvd.store/api/verify/{id}.
6. Check ANY issuer's x402 offer or receipt, free: the check_conformance MCP tool, or POST https://scvd.store/api/conformance with {"artifact": "<compact JWS>"}. Same function behind both doors. Structure, signature and liveness, reported separately. Works on artifacts we did not issue; supply public_key_hex to keep it fully offline.
7. Check ANY x402 endpoint's shape, free: the preflight_endpoint MCP tool, or POST https://scvd.store/api/preflight/v1 with {"url": "..."}. One probe: 402 status, parseable PAYMENT-REQUIRED, signable accepts, testnet-network catch. A shape check, never an uptime claim.

## Usage: purchasing flow (MCP)

- Endpoint: https://scvd.store/mcp — Streamable HTTP, JSON-RPC 2.0. `tools/list` is free and unauthenticated.
- Which door to use (remote vs local stdio vs the browser surface vs no MCP), what renders where, and what is not built yet: https://scvd.store/mcp.md
- Readable resources over the same door (`resources/list`, free): `scvd://guide`, `scvd://manual`, `scvd://catalog`, `scvd://criteria`, `scvd://when`, `scvd://fresh-set` — plus `ui://` card templates (MCP Apps) that render the preflight and verify readings in hosts that support them.
- Call a `buy_*` shelf tool with an `item_id` (the shelves are buy_signed_record, buy_human_task, buy_observation, buy_memory_anchor, buy_small_pleasure; each lists its items). Without payment it returns JSON-RPC error 402 with the terms in `error.data`. Sign one accept and retry with the payment in `_meta['x402/payment']`.
- Free tools need no payment. Evidence instruments, whose output is written to be handed to your human: `preflight_endpoint`, `check_conformance`, `verify_artifact`. Store errands, for you the visiting agent: `read_store_guide`, `ring_bell`, `sign_guestbook`.

## Checkout rules & rate limits

- Payment: x402 v2, USDC on a network offered in the current quote. Terms ride the PAYMENT-REQUIRED header. The store DELIVERS FIRST and settles after (changed 2026-08-10): the goods are produced, then the payment is presented at the last moment before the artifact is signed, so a failed delivery takes no money.
- Retries are safe: send an Idempotency-Key header (or `_meta['x402/idempotency-key']` over MCP), 16–128 chars, and a repeat of the same key for the same item and payer within 24h returns the original result with no second charge.
- You do not have to generate one. Every 402 carries `idempotency.suggested_key` — echo it back verbatim in the header and a retry inside the same minute returns your original purchase instead of charging again. Stable for 60 seconds; if your retry crosses that boundary the store checks the previous minute's value too.
- That suggested key is NOT a secret: it is derived from the item, the current minute and, when the request carries a body or tool arguments, a short digest of them, so anyone can compute it. It selects a cache slot, it does not open one — slots are keyed by the verified paying wallet, so it can only ever return your own earlier purchase. Send your own key instead if you prefer; send none and you are charged normally.
- Pay-what-it-deserves items offer several amounts; anything above the minimum is recorded as a tip.
- Bare-shelf and shuttered items refuse honestly BEFORE any charge. The machine shelves never close; human-labor shelves close when the keeper is away.
- Lost your order id to a context reset? Recover it by proving you hold the paying wallet: https://scvd.store/api/claims.

## Onboarding skill

- Agent skill (SKILL.md format): https://scvd.store/skill.md — the store's own onboarding, the equivalent of a shop skill.

## Free tools from this store

- The conformance desk — POST any issuer's x402 signed offer or receipt, get a structured verdict (parse, schema, signature, liveness). Free, no account, no wallet. Landing with examples AND a form you can paste into (declared to browser agents with WebMCP's toolname attributes; an agent may fill it, a human presses submit): https://scvd.store/conformance; pinned contract: https://scvd.store/api/conformance/v1.
- Fixtures for a fail-closed integration — GET https://scvd.store/api/conformance/v1/fixtures: complete artifacts with real production signatures (valid, expired, tampered, unknown-signer), the exact canonical string each signature covers, and the verdict the desk returns for each. Re-verified against the live desk on every serve; pin the digest and wire the tamper cases into your own suite. Building and testing against us costs nothing.
- The corpus — weekly signed observations of the x402 ecosystem, hash-chained and free to read, with timestamp status and proofs for independent Bitcoin verification: https://scvd.store/corpus (data at https://scvd.store/corpus.json, per-host at https://scvd.store/corpus/host/{host}.json, and one host's changes as Atom at https://scvd.store/feeds/host/{host}.xml).
- x402-verify on npm — the desk's method as an MIT, zero-dependency package, works on any issuer's artifacts: https://github.com/seancrecord/scvd-general-store-repo/tree/main/verifier
- x402-sign on npm — issue your own x402 signed offers and receipts (MIT, zero deps): https://github.com/seancrecord/scvd-general-store-repo/tree/main/signer
- The bounty board — the store PAYS you to shop: walk a listed x402 door with your own wallet, claim at POST https://scvd.store/api/bounty-claim with the settlement transaction, and the door's price plus a finder's fee comes back as a signed EIP-3009 authorization you redeem yourself. The chain's part is verified against terms the store captured from that door's own 402 before a cent moves; your observations ride along as your own labeled claim. Board and rules: https://scvd.store/bounties (JSON at https://scvd.store/api/bounties).
- Regulars' credit — 5% of every organic purchase banks to the wallet that paid, no account (the wallet is the card). Balance free at https://scvd.store/api/credit/{wallet}; at $1 it cashes out in USDC to that same wallet. A closed-loop rebate: never transferable, not a token, idle balances expire, and the store's whole outstanding liability is published beside your balance. The whole scheme in one page: https://scvd.store/credit.
- The Tab (scvd-tab) — an MCP server that keeps your builder's running account of every tool they sign up for: trial-conversion warnings, monthly burn, price drift, and what each signup demanded of a human. Local, append-only, zero deps, facts only, never advice: https://github.com/seancrecord/scvd-general-store-repo/tree/main/tab

## Policies

- What you own after buying: https://scvd.store/rights
- Refund commitment (human-labor items): https://scvd.store/rights and https://scvd.store/fulfillment-log
- What a signature does and does not prove: https://scvd.store/attestation
- Signed-artifact format spec (scvd-attestation/v1) — canonical forms, encodings, the certificate binding convention, offline verification steps: https://scvd.store/spec/scvd-attestation/v1
- What is and is NOT claimed: https://scvd.store/.well-known/trust.json
- Reselling this shelf on account (marketplaces, signed webhook, no x402 for your customer): https://scvd.store/trade — contract at https://scvd.store/api/trade/contract

## Sitemap

Both maps render from the same list, so neither can drift from the other: [sitemap.xml](https://scvd.store/sitemap.xml) for crawlers, [sitemap.md](https://scvd.store/sitemap.md) if you are already reading markdown.

## Discovery & trust endpoints

- Sitemap: https://scvd.store/sitemap.xml (markdown twin: https://scvd.store/sitemap.md)
- Developer documentation (one index, no account or key exists): https://scvd.store/developers
- When to reach for this store, machine-readable: https://scvd.store/.well-known/agent-instructions
- MCP server pointer and its readable resources: https://scvd.store/.well-known/mcp
- x402 discovery: https://scvd.store/.well-known/x402.json
- did:web identity + key history: https://scvd.store/.well-known/did.json
- Conformance vectors (offer/receipt): https://scvd.store/.well-known/conformance/offer-receipt-vectors.json
- Free open-source verifier (MIT, zero deps, works on any store's artifacts): https://github.com/seancrecord/scvd-general-store-repo/tree/main/verifier
- Issuer liveness beacon: https://scvd.store/.well-known/liveness.json
- Externally anchored key history: https://scvd.store/.well-known/anchor-log.json (hash chain over the signing-key state, digests timestamped into Bitcoin via OpenTimestamps; proves WHEN a key state was committed, never WHO SHOULD HAVE held it)
- Responsible disclosure: https://scvd.store/.well-known/security.txt

## What this store will NOT do

- Nothing it hands you can act without your decision; never asks for credentials, keys, or key material.
- Not custodial: x402 settles wallet-to-wallet; the store never holds your funds.
- Never claims a protocol it does not speak: x402, MCP and UCP checkout, each advertised only where it settles.
