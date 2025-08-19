# Gasyard-SDK Integration

The Gasyard SDK provides seamless cross-chain bridging capabilities across multiple blockchain networks. This guide will walk you through the complete integration process.

## Installation

Install the Gasyard SDK via npm:

```bash
npm install gasyard-sdk
```

## Quick Start

```jsx
const { GasyardSDK } = require("gasyard-sdk");

// Initialize the SDK
const sdk = new GasyardSDK({
  apiKey: "your-api-key", // Use 'trial' for testing
  version: 2, // default is 1
});
```

## Supported Networks

The SDK supports bridging across the following networks:

| Network         | Gasyard ChainID | Network Chain ID | Base Token         | Supported Tokens | Token Addresses                                                                                                                                                                          |
| --------------- | --------------- | ---------------- | ------------------ | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Ethereum**    | 1               | 1                | ETH (18 decimals)  | ETH, USDC, USDT  | `0x0000000000000000000000000000000000000000`, `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48`, `0xdAC17F958D2ee523a2206206994597C13D831ec7`                                                 |
| **Base**        | 2               | 8453             | ETH (18 decimals)  | ETH, USDC        | `0x0000000000000000000000000000000000000000`, `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`                                                                                               |
| **BNB Chain**   | 3               | 56               | BNB (18 decimals)  | BNB, BNB-USD     | `0x0000000000000000000000000000000000000000`, `0x55d398326f99059fF775485246999027B3197955`                                                                                               |
| **Arbitrum**    | 4               | 42161            | ETH (18 decimals)  | ETH, USDC        | `0x0000000000000000000000000000000000000000`, `0xaf88d065e77c8cC2239327C5EDb3A432268e5831`                                                                                               |
| **Hyperliquid** | 5               | 42161            | USDC (18 decimals) | USDC             | `0xaf88d065e77c8cC2239327C5EDb3A432268e5831`                                                                                                                                             |
| **Movement**    | 6               | 126              | MOVE (8 decimals)  | MOVE, USDT, USDC | `0x0000000000000000000000000000000000000000`, `0x447721a30109c662dde9c73a0c2c9c9c459fb5e5a9c92f03c50fa69737f5d08d`, `0x83121c9f9b0527d1f056e21a950d6bf3b9e9e2e8353d0e95ccea726713cbea39` |
| **Solana**      | 7               | 1329             | SOL (9 decimals)   | SOL, USDC, USDT  | `0x0000000000000000000000000000000000000000`, `EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`, `Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB`                                             |
| **Bera**        | 8               | 80094            | BERA (18 decimals) | None             | No supported tokens configured                                                                                                                                                           |
| **Sei**         | 9               | 1329             | SEI (18 decimals)  | None             | No supported tokens configured                                                                                                                                                           |
| **Polygon**     | 10              | 137              | POL (18 decimals)  | POL, USDC        | `0x0000000000000000000000000000000000000000`, `0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359`                                                                                               |

**Important Notes:**

- Use **Gasyard Chain ID** in all SDK method calls (not Network Chain ID)
- Native tokens always use address `0x0000000000000000000000000000000000000000`
- Token addresses are listed in the same order as the supported tokens column

## Core Methods

### 1. Get Network Configuration

Retrieve all supported networks and their configurations:

```jsx
async function getNetworkConfig() {
  try {
    const config = await sdk.getConfig();
    console.log("Supported networks:", config);
    return config;
  } catch (error) {
    console.error("Error fetching config:", error);
  }
}
```

### 2. Get Bridge Quote

Get a quote for bridging tokens between networks:

```jsx
async function getBridgeQuote() {
  try {
    const quote = await sdk.getQuote({
      inputNetwork: 2, // Base
      outputNetwork: 5, // Hyperliquid
      inputTokenAmount: "50000000", // Amount in smallest unit
      inputTokenContract: "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", // USDC on Base
      outputTokenContract: "0xaf88d065e77c8cC2239327C5EDb3A432268e5831", // USDC on Arbitrum
      sourceAddress: "0x14fB25f4C1EEf59C07c183df4a8465856Fd4aBc7", // source address for transaction
      destinationAddress: "0x14fB25f4C1EEf59C07c183df4a8465856Fd4aBc7", // destination address for transaction
      slippage: 100, // slippage in BPS -  default is 5
      showBridgeData: true, // default is true
    });

    console.log("Bridge quote:", quote);
    // Returns: inputTokenAmount, outputTokenAmount, outputValueInUSD, feesInToken, feesInUSD

    return quote;
  } catch (error) {
    console.error("Error getting quote:", error);
  }
}
```

### 4. Check Transaction Status

Monitor the status of your bridge transaction:

```jsx
async function checkBridgeStatus(txHash) {
  try {
    const status = await sdk.getStatus({
      sourceHash: txHash,
    });

    console.log("Transaction status:", status);

    // Status can be: "success", "pending", "failed"
    // Also includes outputTxHash, fees, collateral info

    return status;
  } catch (error) {
    console.error("Error checking status:", error);
  }
}
```

### 5. Get Transaction History

Retrieve bridge transaction history for an address:

```jsx
async function getBridgeHistory(address) {
  try {
    const history = await sdk.getHistory({
      inputAddress: address,
      page: 1,
      limit: 10,
    });

    console.log("Bridge history:", history);

    // Returns paginated results with transaction details
    return history;
  } catch (error) {
    console.error("Error fetching history:", error);
  }
}
```

## Complete Bridge Implementation

Here's a complete example showing how to implement a cross-chain bridge:

```jsx
const { GasyardSDK } = require("gasyard-sdk");
async function testSDK() {
  const sdk = new GasyardSDK({
    apiKey: "", // Your API Key
  });

  // Test Config first to get network objects
  console.log("\nTesting getConfig...");
  const configResponse = await sdk.getConfig();
  console.log("Config:", JSON.stringify(configResponse));

  // Test Quote with simple network IDs
  console.log("\nTesting getQuote...");
  try {
    const quote = await sdk.getQuote({
      inputNetwork: 2,
      outputNetwork: 5,
      inputTokenAmount: "50000000",
      inputTokenContract: "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
      outputTokenContract: "0xaf88d065e77c8cC2239327C5EDb3A432268e5831",
      sourceAddress: "0x14fB25f4C1EEf59C07c183df4a8465856Fd4aBc7",
      destinationAddress: "0x14fB25f4C1EEf59C07c183df4a8465856Fd4aBc7",
      slippage: 100,
      showBridgeData: true,
    });
    console.log("Quote:", JSON.stringify(quote));
  } catch (error) {
    console.log(error.toString());
    console.log(error.message);
  }

  // Test Status (using a sample transaction hash)
  console.log("\nTesting getStatus...");
  const status = await sdk.getStatus({
    sourceHash:
      "0x50aa2de06b9a386a6f87ac0c509a088240e129c71503e7c323e4e27f7fcaeec4",
  });
  console.log("Status:", JSON.stringify(status, null, 2));

  // Test History (using a sample address)
  console.log("\nTesting getHistory...");
  const history = await sdk.getHistory({
    inputAddress: "0xc5218F7e68f800c45c4BF49D87BE56b6a2692efB",
    page: 1,
    limit: 10,
  });
  console.log("History:", JSON.stringify(history, null, 2));

  console.log("\nAll tests completed successfully!");
}

// Run the test
testSDK();
```

## API Rate Limits

The trial API key has rate limits. For production use, contact Gasyard for a production API key with higher limits.
