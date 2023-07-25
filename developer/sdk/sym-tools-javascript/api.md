---
description: SDK를 사용하여 트랜잭션 생성, 해시, 전송등 관련된 함수들을 사용할 수 있습니다.
---

# API 사용하기

## Transaction 함수

### **1. transaction.recover(message,v, r, s)**

트랜잭션의 서명된 값을 기반으로 PublicKey를 복구합니다.

* **Parameters:**
  * hexMessage: The hex message.
  * hexV: The hex V value.
  * hexR: The hex R value.
  * hexS: The hex S value.
* **Returns**: The recovered address.

```javascript
// 예제 코드
transaction.recover("0xf84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280"
, "0x0", "0x0df37aa532758d3e54230335c37b54ae2c39b77635d253d05ae3f62bd25562c3", "0x5aab63b9011624f8863987757b5c318fc38b40b82d1c48f12d4bfd0a3c9ad7cd")

// 결과
{
    "compressed": "0x02e744ad9e5b9a27c921c73e0357efe21ea57701eaf225ee85502539daa9b85a67",
    "uncompressed": "0x04e744ad9e5b9a27c921c73e0357efe21ea57701eaf225ee85502539daa9b85a6766b87a4376da3d40942594aa1d1a3cfa72ae476f48ff70d24dc101deac18bb0e",
    "removePrefix": "0xe744ad9e5b9a27c921c73e0357efe21ea57701eaf225ee85502539daa9b85a6766b87a4376da3d40942594aa1d1a3cfa72ae476f48ff70d24dc101deac18bb0e"
}
```

### **2. transaction.recoverFromRawTx(rawTx)**

RawTransaction 기반으로 PublicKey를 복구합니다.

* **Parameters**:
  * hexRawTx: The hex raw transaction.
* **Returns**: The recovered address.

```javascript
// 예제 코드
transaction.recoverFromRawTx("0xf88d8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d3500028080a00df37aa532758d3e54230335c37b54ae2c39b77635d253d05ae3f62bd25562c3a05aab63b9011624f8863987757b5c318fc38b40b82d1c48f12d4bfd0a3c9ad7cd")

// 결과
{
    "compressed": "0x02e744ad9e5b9a27c921c73e0357efe21ea57701eaf225ee85502539daa9b85a67",
    "uncompressed": "0x04e744ad9e5b9a27c921c73e0357efe21ea57701eaf225ee85502539daa9b85a6766b87a4376da3d40942594aa1d1a3cfa72ae476f48ff70d24dc101deac18bb0e",
    "removePrefix": "0xe744ad9e5b9a27c921c73e0357efe21ea57701eaf225ee85502539daa9b85a6766b87a4376da3d40942594aa1d1a3cfa72ae476f48ff70d24dc101deac18bb0e"
}
```

### 3. transaction.composeSendTransaction(...parameters)

Transaction 메시지를 구성합니다

* **Parameters**:
  * from: The sender address.
  * hexNonce: The hex nonce value.
  * hexGasPrice: The hex gas price value.
  * hexGasLimit: The hex gas limit value.
  * to: The receiver address.
  * hexValue: The hex value of the transaction.
  * hexData: The hex data of the transaction.
  * hexType: The hex type of the transaction.
  * warrantNode: The warrant node of the transaction.
  * hexExtraData: The hex extra data of the transaction.
* **Returns**: The composed send transaction.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong><strong>symTools.transaction.composeSendTransaction(
</strong>    "0x00032dd1fa260e2a0002",
    "0xbd",
    "0x174876e800",
    "0x11e68",
    "0xaca4414a37d7a6dc43e5",
    "0x0",
    "0xd71501d48a000232c721024cdd0002880de0b6b3a7640000",
    "0x1",
    "0x0002537dc9a64d350002",
    "0x"
);

// 결과
{
  params: {
    from: '0x00032dd1fa260e2a0002',
    nonce: '0xbd',
    gasPrice: '0x174876e800',
    gasLimit: '0x11e68',
    to: '0xaca4414a37d7a6dc43e5',
    value: '0x0',
    data: '0xd71501d48a000232c721024cdd0002880de0b6b3a7640000',
    type: '0x1',
    warrantNodes: [ '0x0002537dc9a64d350002' ],
    extra: '0x'
  },
  type: '0x01',
  unsignedTx: '0xf84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280',
  hash: '0xcd88b1e01e538c6250eef50063d5171c45c7e807616e1011b025c156c54b860f'
}
</code></pre>

### 4. transaction.**composeSct(**sctType, methodName, params)

SCT 트랜잭션 정보를 구성합니다

* **Parameters**:
  * sctType: The type of the smart contract transaction.
  * methodName: The name of the method.
  * params: The parameters of the method.
* **Returns**: The composed smart contract transaction.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong>transaction.composeSct(21, "transfer", 
    {"recipient": "0x00032dd1fa260e2a0002", "amount": "0x174876e800"}
)

// 결과
{
    "type": 21,
    "method": "TRANSFER",
    "params": [
        "0x15",
        "0x01",
        "0x00032dd1fa260e2a0002",
        "0x174876e800"
    ],
    "gasPrice": "0xbf68",
    "gasLimit": "0x011670",
    "serialize": "0xd41501d18a00032dd1fa260e2a000285174876e800"
}
</code></pre>

### 5. transaction.**composeSct20Transfer**(recipient, amount)

SCT20 전송 트랜잭션을 구성합니다.

* **Parameters**:
  * recipient: The recipient of the transfer.
  * amount: The amount of the transfer.
* **Returns**: The composed SCT20 transfer.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong><strong>transaction.composeSct20Transfer("0x00032dd1fa260e2a0002", "0x174876e800")
</strong>
// 결과
{
    "type": 20,
    "method": "TRANSFER",
    "params": [
        "0x14",
        "0x01",
        "0x00032dd1fa260e2a0002",
        "0x174876e800"
    ],
    "gasPrice": "0xbf68",
    "gasLimit": "0x011288",
    "serialize": "0xd41401d18a00032dd1fa260e2a000285174876e800"
}

</code></pre>

### 6. transaction.**composeSct21Transfer**(recipient, amount)

SCT21 전송 트랜잭션을 구성합니다.

* **Parameters**:
  * recipient: The recipient of the transfer.
  * amount: The amount of the transfer.
* **Returns**: The composed SCT21 transfer.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong>transaction.composeSct21Transfer("0x00032dd1fa260e2a0002", "0x174876e800")

// 결과
{
    "type": 21,
    "method": "TRANSFER",
    "params": [
        "0x15",
        "0x01",
        "0x00032dd1fa260e2a0002",
        "0x174876e800"
    ],
    "gasPrice": "0xbf68",
    "gasLimit": "0x011670",
    "serialize": "0xd41501d18a00032dd1fa260e2a000285174876e800"
}
</code></pre>

### 7. transaction.**composeSct22Transfer**(recipient, amount)

SCT22 전송 트랜잭션을 구성합니다.

* **Parameters**:
  * recipient: The recipient of the transfer.
  * amount: The amount of the transfer.
* **Returns**: The composed SCT22 transfer.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong>transaction.composeSct22Transfer("0x00032dd1fa260e2a0002", "0x174876e800")

// 결과
{
    "type": 22,
    "method": "TRANSFER",
    "params": [
        "0x16",
        "0x01",
        "0x00032dd1fa260e2a0002",
        "0x174876e800"
    ],
    "gasPrice": "0xbf68",
    "gasLimit": "0x011288",
    "serialize": "0xd41601d18a00032dd1fa260e2a000285174876e800"
}
</code></pre>

### **8. transaction.parseRawTx(**rawTx**)**

RawTransaction 를 트랜잭션 메시지로 추출합니다.

* **Parameters**:
  * rawTx: The raw transaction to be parsed.
* **Returns**: The parsed transaction.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong>transaction.parseRawTx("0xf84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280")

// 결과
{
    "from": "0x00032dd1fa260e2a0002",
    "nonce": "0xbd",
    "gasPrice": "0x174876e800",
    "gasLimit": "0x11e68",
    "to": "0xaca4414a37d7a6dc43e5",
    "value": "0x0",
    "data": "0xd71501d48a000232c721024cdd0002880de0b6b3a7640000",
    "type": "0x1",
    "warrantNodes": [
        "0x0002537dc9a64d350002"
    ],
    "extra": "0x"
}
</code></pre>

### **9. transaction.parseSct**(hexSctMethod)

SCT Raw Paramter 값을 SCT 메시지로 추출합니다.

* **Parameters**:
  * hexSctMethod: The hex SCT method to be parsed.
* **Returns**: The parsed SCT method.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong>transaction.parseSct("0xd71501d48a000232c721024cdd0002880de0b6b3a7640000")

// 결과
{
    "type": 21,
    "method": "TRANSFER",
    "params": [
        "0x000232c721024cdd0002",
        "0x0de0b6b3a7640000"
    ]
}
</code></pre>

## Hasher 함수

### **1. hasher.hashMessage(**hexMessage**)**

&#x20;메시지를 hash 함수를 통해 추출합니다.

* **Parameters**:
  * hexMessage: The hex message to be hashed.
* **Returns**: The hashed message.

<pre class="language-javascript"><code class="lang-javascript"><strong>// 예제 코드
</strong>hasher.hashMessage("f84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280")

// 결과
{
    "message": "f84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280",
    "hash": "0xcd88b1e01e538c6250eef50063d5171c45c7e807616e1011b025c156c54b860f"
}
</code></pre>

### **2. hasher.combineMessage(**message, v, r, s**)**

&#x20;메시지와 v,r,s 기반으로 서명값과 RawTransaction를 추출합니다.

* **Parameters**:
  * hexMessage: The hex message.
  * hexV: The hex V value.
  * hexR: The hex R value.
  * hexS: The hex S value.
* **Returns**: The combined message.

```javascript
// 예제 코드
hasher.combineMessage("0xf84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280"
, "0x0", "0x0df37aa532758d3e54230335c37b54ae2c39b77635d253d05ae3f62bd25562c3", "0x5aab63b9011624f8863987757b5c318fc38b40b82d1c48f12d4bfd0a3c9ad7cd")

// 결과
{
    "message": "f84a8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d35000280",
    "signature": "0x0000000000000000000000000000000000000000000000000000000000000000001111111111111111111111111111111111111111111111111111111111111111",
    "rawTx": "0xf88d8a00032dd1fa260e2a000281bd85174876e80083011e688aaca4414a37d7a6dc43e58098d71501d48a000232c721024cdd0002880de0b6b3a764000001cb8a0002537dc9a64d3500028080a00000000000000000000000000000000000000000000000000000000000000000a01111111111111111111111111111111111111111111111111111111111111111"
}
```

## 참고 문서

* **Github:** [https://github.com/symverse-lab/sym-tools](https://github.com/symverse-lab/sym-tools)
* **API 예제:** [https://download.symverse.com/sym-tools-js/sym\_tools.html](https://download.symverse.com/sym-tools-js/sym\_tools.html)



