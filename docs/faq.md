# FAQ

### General

#### What _is_ x402 in a single sentence?

x402 is an open‑source protocol that turns the dormant HTTP `402 Payment Required` status code into a fully‑featured, on‑chain payment layer for APIs, websites, and autonomous agents.

**Is x402 a CDP Product?**

_No._ While Coinbase Developer Platform provides tooling and are the creators of the standard, it is an open protocol (Apache-2.0 license) and you don't need any Coinbase products to use it. We look forward to further clarifying this distinction and making x402 a credibly neutral payment standard.&#x20;

#### Why not use traditional payment rails or API keys?

Traditional rails require credit‑card networks, user accounts, and multi‑step UI flows.\
x402 removes those dependencies, enabling programmatic, HTTP-native payments (perfect for AI agents) while dropping fees to near‑zero and settling in \~1 second.

#### Is x402 only for crypto‑native projects?

No. Any web API or content provider—crypto or web2—can integrate x402 if it wants a lower‑cost, friction‑free payment path for small or usage‑based transactions.

### Language & Framework Support

#### What languages and frameworks are supported?

TypeScript and Python are **the reference implementations**, but x402 is an **open protocol**.

Nothing prevents you from implementing the spec in Go, Rust, etc. If you're interested in building support for your favorite language, please [open an issue](https://github.com/coinbase/x402/issues) and let us know, we'd be happy to help!

### Facilitators

#### Who runs facilitators today?

Coinbase Developer Platform operates the first production facilitator. The protocol, however, is **permissionless**—anyone can run a facilitator. Expect:

* Community‑run facilitators for other networks or assets.
* Private facilitators for enterprises that need custom KYT / KYC flows.

#### What stops a malicious facilitator from stealing funds or lying about settlement?

Every `x402PaymentPayload` is **signed by the buyer** and settles **directly on‑chain**.\
A facilitator that tampers with the transaction will fail signature checks.

### Pricing & Schemes

#### How should I price my endpoint?

There is no single answer, but common patterns are:

* **Flat per‑call** (e.g., `$0.001` per request)
* **Tiered** (`/basic` vs `/pro` endpoints with different prices)
* **Up‑to** (work in progress): "pay‑up‑to" where the final cost equals usage (tokens, MB, etc.)

#### Can I integrate x402 with a usage / plan manager like Metronome?

Yes. x402 handles the _payment execution_. You can still meter usage, aggregate calls, or issue prepaid credits in Metronome and only charge when limits are exceeded. Example glue code is coming soon.

### Assets, Networks & Fees

#### Which assets and networks are supported today?

| Network        | Asset | Fees\*   | Status      |
| ------------   | ----- | -------- | ----------- |
| Base           | USDC  | fee-free | **Mainnet** |
| Base Sepolia   | USDC  | fee-free | **Testnet** |
| Solana         | SPL Tokens  | fee-free | **Mainnet** |
| Solana Devnet | SPL Tokens  | fee-free | **Testnet** |

* Gas paid on chain; Coinbase's x402 facilitator adds **zero** facilitator fee.

_Support for additional chains and assets is on the roadmap and community‑driven._

#### Does x402 support fiat off‑ramps or credit‑card deposits?

Not natively. However, facilitators or third‑party gateways can wrap x402 flows with on‑ and off‑ramps.&#x20;

### Security

#### Do I have to expose my private key to my backend?

No. The recommended pattern is:

1. **Buyers (clients/agents)** sign locally in their runtime (browser, serverless, agent VM). You can use CDP Wallet API to create a programmatic wallet.
2. **Sellers** never hold the buyer's key; they only verify signatures.

#### How do refunds work?

The current `exact` scheme is a _push payment_—irreversible once executed. Two options:

1. **Business‑logic refunds:** Seller sends a new USDC transfer back to the buyer.
2. **Escrow schemes:** Future spec could add conditional transfers (e.g., HTLCs or hold invoices).

### Usage by AI Agents

#### How does an agent know what to pay?

Agents follow the same flow as humans:

1. Make a request.
2. Parse the `402` JSON (`accepts` array).
3. Choose a suitable requirement and sign a payload via the x402 client SDKs.
4. Retry with `X‑PAYMENT`.

#### Do agents need wallets?

Yes. Programmatic wallets (e.g., **CDP Wallet API**, **viem**, **ethers‑v6** HD wallets) let agents sign `EIP‑712` payloads without exposing seed phrases.

### Governance & Roadmap

#### Is there a formal spec or whitepaper?

* **Spec:** [GitHub Specification](https://github.com/coinbase/x402/tree/main/specs)
* [**Whitepaper**](https://www.x402.org/x402-whitepaper.pdf)

#### How will x402 evolve?

Tracked in public GitHub issues + community RFCs. Major themes:

* Multi‑asset support
* Additional schemes (`upto`, `stream`, `permit2`)
* Discovery layer for service search & reputation

**Why is x402 hosted in the Coinbase GitHub?**

We acknowledge that the repo is primarily under Coinbase ownership today. This is primarily to leverage our best-in-house security and auditing team to ensure the spec is safe and nobody accidentally creates legally ambiguous payment flows. We intend to eventually transfer ownership of the repo to a steering group or open-source committee.

### Troubleshooting

#### I keep getting `402 Payment Required`, even after attaching `X‑PAYMENT`. Why?

1. Signature is invalid (wrong chain ID or payload fields).
2. Payment amount < `maxAmountRequired`.
3. Address has insufficient USDC or was flagged by KYT.\
   Check the `error` field in the server's JSON response for details.

#### My test works on Base Sepolia but fails on Base mainnet—what changed?

* Ensure you set `network: "base"` (not `"base‑sepolia"`).
* Confirm your wallet has _mainnet_ USDC.
* Gas fees are higher on mainnet; fund the wallet with a small amount of ETH for gas.

### Still have questions?

• Reach out in the [Discord channel](https://discord.gg/invite/cdp)\
• Open a GitHub Discussion or Issue in the [x402 repo](https://github.com/coinbase/x402)
