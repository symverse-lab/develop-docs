# Transaction

**Contents**

* Types of transaction
* Transaction data
* Signing Process
* Type ‘0’ - General transaction
* Type ‘1’ - SCT transaction
* Type ‘2’ - Deposit transaction
* Appendix – Deposit policy

## Types of transaction

* General transaction
  * Remit transaction: Ethereum convention
  * Smart contract transaction: Ethereum convention
* SCT (Symverse Contract Template) transaction
* Deposit transaction

## Transaction data

* parameters

| Field     | Type       | Description                                                         |
| --------- | ---------- | ------------------------------------------------------------------- |
| from      | address    | \[10]bytes, sender address                                          |
| nonce     | int        | the count of transaction publication in the account                 |
| gasPrice  | int        | gas price per gas unit                                              |
| gas       | int        | gas amount for executing transaction                                |
| to        | address    | \[10]bytes, receiver address or contract address or nil             |
| value     | int        | the value sent with this transaction or amount of deposit           |
| input     | data       | \[]byte, rlp encoded data (contract or sct)                         |
| type      | int        | transaction type (0: general(default), 1: sct, 2: deposit)          |
| workNodes | \[]address | array, list of work nodes what deliver the transaction (count == 1) |
| extraData | data       | \[]byte                                                             |

※ Note

* The address is basically SymID
* ‘type’ decides the usage of transaction. If this is not specified, the transaction will be general transaction by default.\
  type ‘0’: General transaction (default)\
  type ‘1’: SCT transaction\
  type ‘2’: Deposit transaction
* ‘workNodes’ should be included in the transaction by wallet before it is signed. If this is not specified, it will be set from symbase of node by default.
* ‘nonce’ will be automatically increased ‘1’ from last state of account, when it is not specified.
* ‘type’, ‘gas’, ‘gasPrice’, ‘value’, and ‘workNodes’ will be default value, when it is not specified.

※ Reference

* [Ethereum RLP](https://github.com/ethereum/wiki/wiki/RLP)

## Gas

The fixed value of ‘gasPrice’ is 100 GHug (0x174876e800)

```
gas = base_gas
    + (number of none-zero-byte)*680 
    + (number of zero-byte)*40 
    + contract_operation_gas
```

* base\_gas
  * Smart contract or SCT creation('to' is nil): 8000000(0x7a1200)
  * Otherwise: 49000(0xbf68)
* contract\_operation\_gas
  * The gas paid for creating or running Smart contract or SCT. This follows gas table.
  * This is zero when the transaction is not for contract.

※ Reference

* [SCT Function and Gas](https://github.com/symverse-lab/Document/wiki/SCT#sct-function-and-gas)

## Signing Process

*   Signing process is as follow.

    _step 1._ Tx = tx\_data + {chain\_id, "", ""}\
    _step 2._ encoded\_Tx = RLP\_encode(Tx)\
    _step 3._ Tx\_hash = SHA3(encoded\_Tx)\
    _step 4._ V, R, S = SIGN(Tx\_hash)\
    _step 5._ signed\_Tx = (tx\_data , V, R, S)\
    _step 6._ RLP\_encode(signed\_Tx) // Optional, It is only necessary for rawTransaction\
    _step 7._ send message
* `The Tx is [from, nonce, gasPrice , gas, to, value, input, type, workNodes, extraData, {chain_id, "", ""} ], and the order should be observed.` **Last 3 components are needed for chain segmentation.**
* After getting a hash from the encoded\_Tx, the signing process signs for that hash, and makes signed transaction with the sign. The algorithm is SHA3-256 for hash, and ECDSA Sign Algorithm for signing.
* The signature value V, R, S are returned from sign function after signing. The V value is used as recovery identifier, it should be 0 or 1. The R value indicates lower 32bytes of Sign, and the S value indicates upper 32bytes of Sign. These sign values should be attached to the transaction before sending it to blockchain.
* In case of sendTransaction method, the values are set as default if data fields are not filled.
* In case of sendRawTransaction method, all fields are should be given even though default value. The default value means initial value in given data type. The default value of array such as "input", "extra" is empty array.
* chain id : main net = 1, test net = 2

## Type ‘0’ - General transaction

When ‘type’ is zero(0), it indicates remit transaction compatible with Ethereum. If ‘type’ is not specified, the transaction will be general transaction by default.

If ‘to’ is nil, it is a smart contract creation. Otherwise it is general transaction or contract call.

The ‘input’ is mandatory for contract creation or contract call.

In case of contract call, ‘to’ should be contract address.

* Remit transaction parameters

| Fiend     | Type       | Description                                                         |
| --------- | ---------- | ------------------------------------------------------------------- |
| from      | address    | \[10]bytes, sender address                                          |
| nonce     | int        | the count of transaction publication in the account                 |
| gasPrice  | int        | gas price per gas unit                                              |
| gas       | int        | gas amount for executing transaction                                |
| to        | address    | \[10]bytes, `receiver address`                                      |
| value     | int        | the value sent with this transaction                                |
| input     | data       | \[]byte, rlp encoded data                                           |
| type      | int        | transaction type (`0: general(default)`, 1: sct, 2: deposit)        |
| workNodes | \[]address | array, list of work nodes what deliver the transaction (count == 1) |
| extraData | data       | \[]byte                                                             |

* Contract transaction parameters

| Fiend     | Type       | Description                                                         |
| --------- | ---------- | ------------------------------------------------------------------- |
| from      | address    | \[10]bytes, sender address                                          |
| nonce     | int        | the count of transaction publication in the account                 |
| gasPrice  | int        | gas price per gas unit                                              |
| gas       | int        | gas amount for executing transaction                                |
| to        | address    | \[10]bytes, `contract address or nil`                               |
| value     | int        | the value sent with this transaction                                |
| input     | data       | \[]byte, rlp encoded data (contract)                                |
| type      | int        | transaction type (`0: general(default)`, 1: sct, 2: deposit)        |
| workNodes | \[]address | array, list of work nodes what deliver the transaction (count == 1) |
| extraData | data       | \[]byte                                                             |

* Example

```javascript
    // Remit transaction
    sym.sendTransaction({from: "0x00021000000000010002", to: "0x00021000000000070002", workNodes: ["0x00021000000000010002"], value: web3.toHug(100, "sym")})

    // Contract creation
    sym.sendTransaction({from: " 0x00021000000000010002 ", workNodes: ["0x00021000000000010002"], input:"0xf8418080f83d9a30783533373936643736363537323733363534333666363936658830783533353934648b39313834653732613030308c307830303030303030303031",  gas: 9000000})

    // Contract call
    sym.sendTransaction({from: "0x00021000000000010002", to:"0x0CED1024EEd02B234df2", workNodes: ["0x00021000000000010002"], input:"0xf8418080f83d9a30783533373936643736363537323733363534333666363936658830783533353934648b39313834653732613030308c307830303030303030303031", gas: 150000})

    // Raw transaction
    sym.sendRawTransaction("0xf8738a0000000000000000000901850430e2340083015f908a00000000000000000009808002cb8a0000000000000000000901a068c19c97383288faa6373c8b058ed386753c767a3e4976937b2afca1515df875a0142de5cf2687167da8d13f51a4767536ea1473b6d46f76edfa644b04aa428901")

    // Check balance
    sym.getBalance("0x00021000000000010002")
```

## Type ‘1’ - SCT transaction

When ‘type’ is 1, it indicates SCT operation.

If ‘to’ is nil, it is SCT creation. Otherwise it is SCT call(‘to’ should be contract address).

‘input’ is mandatory for SCT creation or SCT call.

| Fiend     | Type       | Description                                                         |
| --------- | ---------- | ------------------------------------------------------------------- |
| from      | address    | \[10]bytes, sender address                                          |
| nonce     | int        | the count of transaction publication in the account                 |
| gasPrice  | int        | gas price per gas unit                                              |
| gas       | int        | gas amount for executing transaction                                |
| to        | address    | \[10]bytes, `contract address or nil`                               |
| value     | int        | not used                                                            |
| input     | data       | \[]byte, `rlp encoded data (sct)`                                   |
| type      | int        | transaction type (0: general(default), `1: sct`, 2: deposit)        |
| workNodes | \[]address | array, list of work nodes what deliver the transaction (count == 1) |
| extraData | data       | \[]byte                                                             |

In SCT processing, `input` fomart data has the following format. Invokes the [JSON RPC SCT API](https://github.com/symverse-lab/Document/wiki/JSON-RPC-SCT-API) through a set of Type, Method, and Parameter. Please check the contents of each SCT format below.

`[ ${Type}, ${Method}, ${Parameter} ]`

* _Type_ - It means Type to be processed among SCT 20/21/30/40.
* _Method_ - Method corresponding to SCT Type.
* _Parameter_ - Parameter corresponding to SCT Type.

After creating the format, input `RLP encoding` value into `input`.

`input = rlp.encode[ ${Type}, ${Method}, ${Parameter} ]`

**Example**

[SCT20\_CREATE](https://github.com/symverse-lab/Document/wiki/JSON-RPC-SCT-API#example)

## Type ‘2’ - Deposit transaction

When ‘type’ is 2, it indicates deposit operation.

If ‘to’ is nil, it is deposit set. Otherwise it is deposit restoration.

The ‘value’ is amount of deposit for deposit set transaction.

The ‘from’ and the ‘to’ should be same for deposit restoration transaction.

In case of deposit set, deposit will be added to amount of the account state.

In case of deposit restoration, the deposit amount of the account state will be cleared, except for that the account is acting as Warrant node, or the role of account is CA or Oraclizer.

* Parameters

| Fiend     | Type       | Description                                                         |
| --------- | ---------- | ------------------------------------------------------------------- |
| from      | address    | \[10]bytes, sender address                                          |
| nonce     | int        | the count of transaction publication in the account                 |
| gasPrice  | int        | gas price per gas unit                                              |
| gas       | int        | gas amount for executing transaction                                |
| to        | address    | \[10]bytes, `deposit owner address or nil`                          |
| value     | int        | `amount of deposit`                                                 |
| input     | data       | not used                                                            |
| type      | int        | transaction type (0: general(default), 1: sct, `2: deposit`)        |
| workNodes | \[]address | array, list of work nodes what deliver the transaction (count == 1) |
| extraData | data       | \[]byte                                                             |

### Example

```javascript
    // Deposit set
    sym.sendTransaction({from: "0x0002A000000000010002", type: "0x2", value:web3.toHug(20, "sym")})
    sym.setDeposit("0x0002A000000000010002", web3.toHug(20, "sym")) //optional method: gas is set as default and gas price is set as suggested

    // Deposit restoration
    sym.sendTransaction({from: "0x0002A000000000010002", to:"0x0002A000000000010002", type: "0x2"})
    sym.restoreDeposit("0x0002A000000000010002") //optional method: gas is set as default and gas price is set as suggested

    // Raw transaction
    sym.sendRawTransaction("0xf8758a0000000000000000000901850430e2340083015f90808901158e460913d00000838207d002cb8a0000000000000000000980a0dceb7d07d0ba181f8d918a2b61cf9e55f8cac1b19dc94729f56dd7210e0a4b9aa0511c581b649ce6748586f176be60042b6b020fbc22615c5248c14ef54c97fc05")

    // Check deposit
    sym.getDeposit("0x0002A000000000010002")
```

## Appendix – Deposit policy

* Deposit set
  * User can set a deposit only for their own account at any time.
  * If the deposit set transaction is issued, the balance is deducted and converted to a deposit at account state.
  * The deposit set transaction can be issued several times even if the balance is enough.
* Deposit restoration
  * User can restore a deposit only for their own account at any time.
  * However, deposit restoration transaction is failed if account is in activity as Warrant node or CA or Oraclizer.
  * If deposit restoration transaction is succeeded, all deposit is cleared and converted to a balance at account state.
* Restriction of restoring deposit
  * When an account is acting as Warrant node, in other words, SymID is included in current/active warrant block.
  * When the role of account is CA.
  * When the role of account is Oraclizer.
