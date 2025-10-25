# Facilitator

This page explains the role of the **facilitator** in the x402 protocol.

The facilitator is an optional but recommended service that simplifies the process of verifying and settling payments between clients (buyers) and servers (sellers).

### What is a Facilitator?

The facilitator is a service that:

* Verifies payment payloads submitted by clients.
* Settles payments on the blockchain on behalf of servers.

By using a facilitator, servers do not need to maintain direct blockchain connectivity or implement payment verification logic themselves. This reduces operational complexity and ensures accurate, real-time validation of transactions.

### Facilitator Responsibilities

* **Verify payments:** Confirm that the client's payment payload meets the server's declared payment requirements.
* **Settle payments:** Submit validated payments to the blockchain and monitor for confirmation.
* **Provide responses:** Return verification and settlement results to the server, allowing the server to decide whether to fulfill the client's request.

The facilitator does not hold funds or act as a custodian - it performs verification and execution of onchain transactions based on signed payloads provided by clients.

### Why Use a Facilitator?

Using a facilitator provides:

* **Reduced operational complexity:** Servers do not need to interact directly with blockchain nodes.
* **Protocol consistency:** Standardized verification and settlement flows across services.
* **Faster integration:** Services can start accepting payments with minimal blockchain-specific development.

While it is possible to implement verification and settlement locally, using a facilitator accelerates adoption and ensures correct protocol behavior.

### Live Facilitators

1. Currently, CDP hosts a facilitator live on Base mainnet. For more information about getting started, see the [CDP Docs](https://docs.cdp.coinbase.com/x402/docs/welcome).

* CDP's facilitator offers fee-free USDC settlement on Base mainnet

2. PayAI [hosts a facilitator](https://facilitator.payai.network) on Solana, Base, Polygon, and more. More info & docs at https://docs.payai.network/x402.


### Interaction Flow

1. `Client` makes an HTTP request to a `resource server`
2. `Resource server` responds with a `402 Payment Required` status and a `Payment Required Response` JSON object in the response body.
3. `Client` selects one of the `paymentDetails` returned by the `accepts` field of the server response and creates a `Payment Payload` based on the `scheme` of the `paymentDetails` they have selected.
4. `Client` sends the HTTP request with the `X-PAYMENT` header containing the `Payment Payload` to the `resource server`
5. `Resource server` verifies the `Payment Payload` is valid either via local verification or by POSTing the `Payment Payload` and `Payment Details` to the `/verify` endpoint of the `facilitator server`.
6. `Facilitator server` performs verification of the object based on the `scheme` and `networkId` of the `Payment Payload` and returns a `Verification Response`
7. If the `Verification Response` is valid, the resource server performs the work to fulfill the request. If the `Verification Response` is invalid, the resource server returns a `402 Payment Required` status and a `Payment Required Response` JSON object in the response body.
8. `Resource server` either settles the payment by interacting with a blockchain directly, or by POSTing the `Payment Payload` and `Payment Details` to the `/settle` endpoint of the `facilitator server`.
9. `Facilitator server` submits the payment to the blockchain based on the `scheme` and `networkId` of the `Payment Payload`.
10. `Facilitator server` waits for the payment to be confirmed on the blockchain.
11. `Facilitator server` returns a `Payment Execution Response` to the resource server.
12. `Resource server` returns a `200 OK` response to the `Client` with the resource they requested as the body of the HTTP response, and a `X-PAYMENT-RESPONSE` header containing the `Settlement Response` as Base64 encoded JSON if the payment was executed successfully.

### Summary

The facilitator acts as an independent verification and settlement layer within the x402 protocol. It helps servers confirm payments and submit transactions onchain without requiring direct blockchain infrastructure.

Next, explore:

* [Client / Server ](client-server.md)— understand the roles and responsibilities of clients and servers
* [HTTP 402](http-402.md) — understand how payment requirements are communicated to clients
