# USDT Balance Checker Documentation

## Overview

The USDT Balance Checker is a WebSocket-based service that allows you to query USDT token balances across different blockchain networks (Ethereum and BSC). This service provides real-time balance information for any valid wallet address.

## Prerequisites

- WebSocket client library
- Valid RPC URLs for Ethereum and BSC networks (set in environment variables)
- Socket.IO client

## Environment Variables

```env
ETHEREUM_RPC_URL=<your-ethereum-rpc-url>
BSC_RPC_URL=<your-bsc-rpc-url>
```

## Usage

### Connecting to the WebSocket Server

```javascript
const socket = io('your-server-url');
```

### Checking USDT Balance

To check a wallet's USDT balance, emit a 'check_usdt_balance' event with the following parameters:

```javascript
socket.emit('check_usdt_balance', {
    chain: 'ethereum', // or 'bsc'
    address: '0x742d35Cc6634C0532925a3b844Bc454e4438f44e' // wallet address
});
```

### Response Format

The server will emit a 'usdt_balance_result' event with the following response format:

#### Successful Response

```javascript
{
    success: true,
    data: {
        chain: 'ethereum',
        address: '0x742d35Cc6634C0532925a3b844Bc454e4438f44e',
        balance: '1000.50',
        currency: 'USDT'
    }
}
```

#### Error Response

```javascript
{
    success: false,
    error: 'Error message here',
    details: 'Detailed error message' // optional
}
```

## Example Implementation

```javascript
const socket = io('your-server-url');

// Request balance check
socket.emit('check_usdt_balance', {
    chain: 'ethereum',
    address: '0x742d35Cc6634C0532925a3b844Bc454e4438f44e'
});

// Listen for response
socket.on('usdt_balance_result', (response) => {
    if (response.success) {
        console.log(`USDT Balance: ${response.data.balance}`);
    } else {
        console.error(`Error: ${response.error}`);
    }
});
```

## Supported Networks

| Network  | Chain ID | USDT Contract Address                      |
| -------- | -------- | ------------------------------------------ |
| Ethereum | 1        | 0xdAC17F958D2ee523a2206206994597C13D831ec7 |
| BSC      | 56       | 0x55d398326f99059fF775485246999027B3197955 |

## Error Handling

The service handles several types of errors:

1. Invalid chain selection

   - Error message: "Unsupported chain. Use ethereum or bsc"
2. Invalid wallet address format

   - Error message: "Invalid wallet address format"
3. RPC connection errors

   - Error message: "Failed to fetch balance"

## Rate Limiting

Please note that the service depends on RPC providers which may have their own rate limiting. Implement appropriate error handling and retry logic in your application.

## Security Considerations

- Only use this service over secure WebSocket connections (WSS)
- Validate all input data before sending to the server
- Don't expose sensitive RPC URLs in client-side code

## Contributing

Please submit issues and pull requests to help improve this service.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
