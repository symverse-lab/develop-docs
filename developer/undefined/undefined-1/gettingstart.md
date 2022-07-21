# GettingStart

### Getting Started with Gsym

1. Download Gsym Binary
2. Running Gsym
3. Access to Gsym Node
4. Creating Account
5. Request Registration to CA
6. Using Gsym

### Download Gsym Binary

To use Gsym, you need to install Gsym first. You can get the Gsym binary from Symverse download page([https://github.com/symverse-lab/Document/tree/master/bin](https://github.com/symverse-lab/Document/tree/master/bin)).

After you download the binary and start a node with some options, then you can use services with JSON-RPC APIs. Please refer to JSON-RPC APIs document( [https://github.com/symverse-lab/Document/wiki/JSON-RPC-API](https://github.com/symverse-lab/Document/wiki/JSON-RPC-API) ).

### Running Gsym

#### Running Gsym Node

When a Gsym node is started, it runs according to Network ID specified. Network ID "1" and "2" are reserved. "1" is for main-net and "2" is for test-net. Any other network ID means a private network.

If an option of "--networkid" or "--testnet" is not specified, the node will be started as a main-net node by default.

You can simply run Gsym as a main-net node as follow.

```
$ gsym
```

For running as a test-net node, run Gsym with "--testnet" or "--networkid 2" option as follow.

```
$ gsym --testnet
```

If you want to customize some environment such as data directory, ip, port, and so on, you can run Gsym with adding some options. An example is as follow.

```
$ gsym --testnet --datadir ./node1 --port 3333  
--rpc --rpcaddr "0.0.0.0" --rpcport 8545  
--rpcapi "admin,sym,debug,net,personal,web3,pon,warrant,citizen,oracle,sct"  
--verbosity 3 console
```

For the description of the options, refer to Command-Line-Options document( [https://github.com/symverse-lab/Document/wiki/Command-Line-Options](https://github.com/symverse-lab/Document/wiki/Command-Line-Options) ).

#### Running RPC Server

In order to access to a node in remote, you need to run a node with RPC server when you start it. You can add "--rpc" option for HTTP server, and "--ws" option for Web Socket server. Then you can access to a node in remote and use JSON-RPC methods with a HTTP or Web Socket client program.

Please refer Command-Line-Options for the detail usage.

#### Access to Gsym Node

**Console attach**

When user want to access a node, user can use the console.

You can enter the console directly when a node is started with "console" command.

```
$ gsym console
```

The other way is to use "attach" command out of a node after the node is started.

```
$ gsym attach rpc:http//127.0.0.1:8545
```

**HTTP**

User can use JSON-RPC command to access to HTTP server of node with remote client program. In detail, please check the manual of that program.

The example of using "Curl" is as follow. You can also use your own program.

```
$ curl -X POST -H "Content-Type: application/json; charset=utf-8" --data '{"jsonrpc":"2.0", "method":"sym_getBalance", "params":["0x000234aa518635e80002","latest"], "id":1}' https://127.0.0.1:8545
```

**Web Socket**

User can also use JSON-RPC command to access to Web Socket server of node with remote client program. In detail, please check the manual of that program.

### Creating Account

#### Making Keystore with Gsym

You can make an account with Gsym, however you need to register your account in the blockchain through CA before you use a service of blockchain. Therefore it is recommended to create an account with Wallet for registering the account through CA.

This section only describes how to create a keystore on a node. If you make up your own private network, you can register your account on a CA node after making keystore with SymID and passphrase.

* Parameters:
* GSYM\_PATH: path to gsym binary file
* DIR\_PATH: path to save the node data
* ACCOUNT\_ADDRESS: 10 byte address(SymID)
* PASSPHRASE: password for generating account
*   Generate Keystore file:

    ```
    $GSYM_PATH/gsym --datadir ./DIR_PATH account new ACCOUNT_ADDRESS PASSPHRASE
    ```
*   Extract public key hash:

    ```
    $GSYM_PATH/gsym --datadir ./DIR_PATH account info ACCOUNT_ADDRESS PASSPHRASE --verbosity 0
    ```

#### Example

```
$GSYM_PATH/gsym --datadir ./test_node1 account new "0x00023627694a7abf0002" "passphrase"  

$GSYM_PATH/gsym --datadir ./test_node1 account info "0x00023627694a7abf0002" "passphrase" --verbosity 0
0x90602765872Bc115e9776b2c1ad78ccbD77160b7 // public-key-hash
```

`Save public key hash for requesting registration to CA.`

### Request Registration to CA

When you register an account to blockchain through CA, you should use CA APIs. Please contact to Symverse for CA APIs.

**Note: When user makes an account in a node, the registration of account should be done manually. However it's not allowed the manual registration of account at this moment. Therefore, if user wants to create an account in Main-net or Test-net, user should use SymWallet or develop a program that it can be processed the creation and registration of account.**

### Using Gsym

#### Unlock Account

In case of some RPC-API methods such as "sym\_sendTransaction", it is needed to unlock account first before using them.

Be careful not to leak the passphrase from your keystore when unlocking your account.

* parameters:
  * Account:byte - 10 byte wallet address to unlock.
  * Passphrase:string - password for the wallet account.
  * Duration:int64 - Duration time to unlock account.

```
$ curl -X POST -H "Content-Type: application/json; charset=utf-8" --data '{"jsonrpc":"2.0", "method":"personal_unlockAccount", "params":["0x000234aa518635e80002","passphrase",0], "id":1}' http://127.0.0.1:8545
```

### Send Transaction

When you want to publish a transaction to blockchain, you can choose two ways.

First way is to send a transaction on the node. In this case, the transaction is signed with private key in the keystore of the node directory.

Second way is to send a RLP encoded raw transaction to a node after making a transaction and signing it. You should manage your own keystore and signing process.

* sendTransaction

```
$ curl -X POST -H "Content-Type: application/json; charset=utf-8" --data
'{"jsonrpc":"2.0", "method":"sym_sendTransaction",
"params":[{"from":"0x000234aa518635e80002","to":"0x00023627694a7abf0002","value":"0x9184e72a"}],
"id":1}' http://127.0.0.1:8545
```

* sendRawTransaction

```
$ curl -X POST -H "Content-Type: application/json; charset=utf-8" --data
'{"jsonrpc":"2.0", "method":"sym_sendRawTransaction",
"params":"0xf8738a0000000000000000000901850430e2340083015f908a00000000000000000009808002cb8a0000000000000000000901a068c19c97383288faa6373c8b058ed386753c767a3e4976937b2afca1515df875a0142de5cf2687167da8d13f51a4767536ea1473b6d46f76edfa644b04aa428901"],
"id":1}' http://127.0.0.1:8545
```

### Appendix

#### Registration of Account Flow

![GettingStart](https://github.com/symverse-lab/Document/wiki/gsym/image/GettingStart1.png)
