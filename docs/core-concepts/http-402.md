# HTTP 402

For decades, HTTP 402 Payment Required has been reserved for future use. x402 unlocks it, and [absolves the internet of its original sin](https://economyofbits.substack.com/p/marc-andreessens-original-sin).

### What is HTTP 402?

[HTTP 402](https://datatracker.ietf.org/doc/html/rfc7231#section-6.5.2) is a standard, but rarely used, HTTP response status code indicating that payment is required to access a resource.

In x402, this status code is activated to:

* Inform clients (buyers or agents) that payment is required.
* Communicate the details of the payment, such as amount, currency, and destination address.
* Provide the information necessary to complete the payment programmatically.

### Why x402 Uses HTTP 402

The primary purpose of HTTP 402 is to enable frictionless, API-native payments for accessing web resources, especially for:

* Machine-to-machine (M2M) payments (e.g., AI agents).
* Pay-per-use models such as API calls or paywalled content.
* Micropayments without account creation or traditional payment rails.

Using the 402 status code keeps x402 protocol natively web-compatible and easy to integrate into any HTTP-based service.

### Summary

HTTP 402 is the foundation of the x402 protocol, enabling services to declare payment requirements directly within HTTP responses. It:

* Signals payment is required
* Communicates necessary payment details
* Integrates seamlessly with standard HTTP workflows
