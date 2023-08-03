---
description: SDK를 사용하여 트랜잭션 생성, 해시, 전송등 관련된 함수들을 사용할 수 있습니다.
---

# API 사용하기

## Transaction 함수

### **1. transaction.recover(message,V, R, S**, \[`chainId?`] **)**

트랜잭션의 서명된 값을 기반으로 PublicKey를 복구합니다.

* **Parameters:**
  * message: RLP 인코딩 트랜잭션의  값
  * V: ECDSA의 서명 v값
  * R: ECDSA의 서명 r값
  * S: ECDSA의 서명 s값
  * options\[Array]: chainId를 추가합니다.
    * chainId: 체인 ID hex string
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

### **2. transaction.recoverFromRawTx(rawTx**, \[`chainId?`] **)**

RawTransaction 기반으로 PublicKey를 복구합니다.

* **Parameters**:
  * hexRawTx: 서명값이 포함된 RLP 인코딩 트랜잭션 값
  * options\[Array]: chainId와 forkId를 추가합니다.
    * chainId: 체인 ID hex string
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

### 3. transaction.composeSendTransaction(...parameters, \[`chainId?`] )

Transaction 메시지를 구성합니다

* **Parameters**:
  * from:  보내는 symId
  * nonce: 전송하는 symid의 트랜잭션 noce
  * gasPrice: 사용할 가스
  * gasLimit: 사용할 최대 가스
  * to: 받는 symid
  * value:  SYM balance
  * data: 트랜잭션에 담길 메시지 혹은 컨트렉트 인코딩 값&#x20;
  * type:  0: general(default), 1: sct, 2: deposit)
  * workNode\[Array]: 배열, 트랜잭션을 전달하는 작업 노드 목록(개수 고정 == 1)
  * extraData: \[]byte
  * options\[Array]: chainId와 forkId를 추가합니다.
    * chainId: 체인 ID hex string
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
  * sctType: SCT 타입을 의미 (20, 30 등) - **문서 참고:** [sct-kor.md](../../../symverse/sct-kor.md "mention")
  * methodName: SCT 실행 함수를 의미&#x20;
  * params: SCT 실행 함수에 들어갈 파라미터를 의미
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
  * recipient: 토큰을 전달 받을 SymID
  * amount: 토큰 개수
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
  * recipient: 토큰을 전달 받을 SymID
  * amount: 토큰 개수
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
  * recipient: 토큰을 전달 받을 SymID
  * amount: 토큰 개수
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

RLP 인코딩된 RawTransaction 를 트랜잭션 메시지로 추출합니다.

* **Parameters**:
  * rawTx: RLP 인코딩 트랜잭션
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

### **9. transaction.parseSct**(RlpEncodeSct)

SCT RLP 인코딩 된 값을 디코딩합니다.

* **Parameters**:
  * hexSctMethod: RLP 인코딩 된 SCT Type, method, params 값
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

### **1. hasher.hashMessage(**hexMessage, \[`chainId?`] **)**

&#x20;메시지를 Hash 처리

* **Parameters**:
  * hexMessage: 메시지를 SHA3로 해시를 실행합니다.
  * options\[Array]: chainId와 forkId를 추가합니다.
    * chainId: 체인 ID hex string
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

서명값이 없는 RLP 인코딩 트랜잭션 메시지와 ecdsa 서명 v ,r, s 기반으로  RLP 인코딩 값을 추출합니다.

* **Parameters**:
  * hexMessage: 서명값이 없는 RLP 인코딩 트랜잭션 메시지
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

## Utils 함수

### 1. **utils.**toValueString(value, srcRadix, dstRadix)

값을 특정 진수에 맞게 변환합니다.

* **Parameters**:
  * hexValue: 특진 진수로 변환할 값
  * srcRadix: 기존 진수
  * dstRadix: 변환할 진수
* **Returns**: The converted value as a string.

```javascript
// 예제 코드
utils.toValueString("0x14", 16, 10)
utils.toValueString("21", 10, 16)

// 결과
"20"
"0x15"
```

### 2. **utils.**stringToUtf8(str)

문자열을 Hex UTF-8 String으로 변환합니다.&#x20;

* **Parameters**:
  * str: 변환할 문자열&#x20;
* **Returns**: The UTF-8 encoded string.

```javascript
// 예제 코드
utils.stringToUtf8("문자열")

//결과 
"0xebacb8ec9e90ec97b4"
```

### 3. **utils.**utf8ToString(hexStr)

Hex UTF8 String 에서 문자열로 변환합니다.

* **Parameters**:
  * hexStr: 변환할 Hex UTF-8 String
* **Returns**: The converted string.

```javascript
// 예제 코드
utils.utf8ToString("0xebacb8ec9e90ec97b4")

//결과 
"문자열"
```

### 4. **utils.**toRlp(params)

특정 값을 RLP 인코딩합니다.

* **Parameters**:
  * hexParams: RLP 인코딩할 값
* **Returns**: The RLP encoded parameters.

```javascript
// 예제 코드
utils.toRlp(param...)

// 결과 
"0xe18a00032dd1fa260e2a00028aaca4414a37d7a6dc43e58a0002537dc9a64d350002"
```

### 5. pubkeyHash(hexPublicKey)

PublicKey를 SHA3로 해시를 실행합니다.

* **Parameters**:
  * hexPublicKey: PublicKey
* **Returns**: The hash of the public key.

```javascript
// 예제 코드
utils.pubkeyHash("0x12345678901234567890123456789012345678901234567890123456789012341234567890123456789012345678901234567890123456789012345678901234")

// 결과
"0x0249e4711e75daab8781ad36e334d7b16610879a"
```

\


## 참고 문서

* **Github:** [https://github.com/symverse-lab/sym-tools](https://github.com/symverse-lab/sym-tools)
* **API 예제:** [https://download.symverse.com/sym-tools-js/sym\_tools.html](https://download.symverse.com/sym-tools-js/sym\_tools.html)



