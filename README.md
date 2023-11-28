# stratum-jsonrpc2.0-ws protocol

This protocol implements the Stratum protocol using JSON-RPC 2.0 over Websockets.

This project is a client implementation of the stratum-jsonrpc2.0-ws protocol.

## Protocol Specification


### Subscription

**Client request:**

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "eth_subscribe",
  "params": ["newWork", "username", "password", "miner_agent"]
}
```

**Server response:**

Return a subscription id:
```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xcd0c3e8af590364c09d0fa6a1210faf5"
}
```

### Job Notification 

After the creation of the subscrition, we can receive incoming notifications related to this subscription:

**Server notification:**

The returned result field is as follows: `jobId, block header pow-hash, seed hash, boundary("target"), block number, extranonce`


```
{
  "jsonrpc": "2.0",
  "method": "eth_subscription",
  "params": {
    "subscription": "0xcd0c3e8af590364c09d0fa6a1210faf5",
    "result": ["4orr4", "0xd9dcd16aec94afca6ccb4e7da9b733946afb9b4d47004215326c5c5ceeab7279", "0x0000000000000000000000000000000000000000000000000000000000000000", "0x0000800000000000000000000000000000000000000000000000000000000000", "0x4d", "86a3"]
  }
}
```

### Submit Solution

**Client request:**

The request params field is as follows: `jobId, nonce, block header pow-hash`

```
{
  "id": 2,
  "jsonrpc": "2.0",
  "method": "eth_submitWork",
  "params": ["4orr4", "0x90989098111111aa", "0x9867fe999999999999999867fe999999999999999867fe999999999999991234"]
}
```

**Server response:**

If nonce is valid, return `true`

```
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": true
}
```

## Other resources

[CCXminer](https://github.com/cyberchain/ccxminer): Cryptonight-GPU-light miner using stratum-jsonrpc2.0-ws.

[CyberChain](https://github.com/cyberchain/ccx): CyberChain client that has this protocol built-in.