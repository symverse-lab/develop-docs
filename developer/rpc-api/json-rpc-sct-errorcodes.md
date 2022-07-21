# JSON RPC SCT ErrorCodes

### Sct Error Code

If validation fails when sending an SCT process transaction, it will be receipt `status: 0x0` just like evm processing. However, in the case of SCT, since it is a built-in contract, the user can determine by checking the value of `errorCode` which validation step failed.

**SCT Error Receipt Example**

The following is an example of calling the `sym_getTransactionReceipt` API when an SCT transaction transfer fails. The errCode is included as shown below.

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockHash": "0x19353ad0c8c3fc1de27fe12b1cc6cd51a694ed2b48118df7866afd9342ce9737",
        "blockNumber": "0x36",
        "contractAddress": null,
        "cumulativeGasUsed": "0x58b0",
        "errCode": "0x3e8",
        "from": "0x00021000000000010002",
        "gasUsed": "0x58b0",
        "logs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x0",
        "to": "0x948a6c99fcb6af9d3b4f",
        "transactionHash": "0xa632390f935baf5527c8ab2a281d6843e8fc0573def900aa237737834a2357c2",
        "transactionIndex": "0x0"
    }
}
```

### SCT Error Codes

check the failure status of Receipt with the error code (`errCode`) below.

**Sct Contract Error Code**

1. `1000` ( hex: `0x3e8` ): occurs when a contract does not exist
2. `1001` ( hex: `0x3e9` ): incorrect format when creating contract (Name length, Symbol length, etc.)
3. `1002` ( hex: `0x3ea` ): contract state value is not correct
4. `1003` ( hex: `0x3eb` ): contract owner or creator is, if not

**Sct Balance or item insufficient Error Codes**

1. `2000` ( hex: `0x7d0` ): insufficient or nonexistent amount of contract account
2. `2001` ( hex: `0x7d1` ): insufficient or nonexistent amount of contract allowance

**Sct Contract Item Error Codes**

1. `3000` ( hex: `0xbb8` ): if the state value of the item is incorrect
2. `3001` ( hex: `0xbb9` ): item is not in the correct format
3. `3002` ( hex: `0xbba` ): when exceeding the creation time of item

**Sct SymId or Citizen Info Error Codes**

1. `4000` ( hex: `0xfa0` ): symid is null or does not exist in the parameter.
2. `4001` ( hex: `0xfa1` ): when requesting transmission to the same symid in parameter

**Sct 50/51 Contract Error Codes**

1. `5000` ( hex: `0x1388` ): already registered poll creator
2. `5001` ( hex: `0x1389` ): not any poll creators
3. `5002` ( hex: `0x138a` ): no proper stake balance
4. `5003` ( hex: `0x138b` ): invalid poll state
5. `5004` ( hex: `0x138c` ): expired poll state
6. `5005` ( hex: `0x138d` ): incorrect sct51 format
7. `5006` ( hex: `0x138e` ): insufficient stake to vote
8. `5007` ( hex: `0x138f` ): can not change voting decision
9. `5008` ( hex: `0x1390` ): sct add poll creators format is incorrect
