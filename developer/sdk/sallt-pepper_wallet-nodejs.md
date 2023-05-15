# @sallt/pepper\_wallet (NodeJS)

## @sallt/pepper\_wallet <a href="#sallt-pepper_wallet" id="sallt-pepper_wallet"></a>

{% hint style="info" %}
**Sallt 프로젝트의 SymKeystoreLoader 프로젝트를 Node.js와 Web에서 사용할 수 있도록 WebAssembly로 개발된 패키지입니다.**
{% endhint %}

**기능 개선이나 성능 향상을 위해 예고없이 구조가 변경될 수 있습니다. 업데이트시 안내를 꼭 확인하시기 바랍니다.**

### &#x20;<a href="#undefined" id="undefined"></a>

Sallt 프로젝트는 Dart/Flutter 프레임워크를 사용하여 크로스플랫폼으로 개발되었습니다.

Dart로 만들어진 SDK 패키지도 훌륭한 성능과 기능을 제공하지만, Dart로 만들어진 컴포넌트 및 서버 애플리케이션, Flutter로 만들어진 모바일 애플리케이션이나 싱글 페이지 애플리케이션(SPA)에서만 사용할 수 있으므로 순수한 웹이나 Node 프로젝트에서는 사용이 제한됩니다.

이를 해결하기 위해 WebAssembly로 개발된 `pepper-wallet`를 사용하면, Node.js와 웹에서도 SymKeystoreLoader 패키지의 기능을 사용할 수 있습니다.

### 설치방법 <a href="#undefined" id="undefined"></a>

`NPM` 설치 패키지 매니저를 통해 설치합니다.

자체 NPM Registry를 사용하고 있으므로, `npm config`를 통해 추가 설정을 해야 합니다.

```javascript
npm config set registry=https://registry.yarnpkg.com/
npm config set @sallt:registry=https://nexus.symverse.com/repository/sallt-npm/
npm config set //nexus.symverse.com/repository/sallt-npm/:_auth=Z3Vlc3Q6Z3Vlc3Q=
npm config set always-auth=true
```

또는,

`.npmrc` 파일을 생성하여 아래 내용을 추가합니다.

```javascript
registry=https://registry.yarnpkg.com/
@sallt:registry=https://nexus.symverse.com/repository/sallt-npm/
//nexus.symverse.com/repository/sallt-npm/:_auth=Z3Vlc3Q6Z3Vlc3Q=
always-auth=true
```

이후 아래 명령어를 통해 패키지를 설치합니다.

`npm install @sallt/pepper_wallet`

### 사용방법 <a href="#undefined" id="undefined"></a>

```javascript
import { SCrypt, Credentials, MsgHashAlgorithm, SymId, UInt256, UInt8, WorkNodes, Data, SymTransaction } from '@sallt/pepper_wallet';

// Parameters of the encrypted private key
let iv = "{16byte hex string}";
let ciphertext = "{32byte hex string}";
let dklen = 32;
let n = 262144;
let p = 1;
let r = 8;
let salt = "{32byte hex string}";

// Recovering the private key
let scrypt = new SCrypt(dklen, n, p, r, salt);
let credentials = new Credentials(scrypt, ciphertext, iv);
await credentials.initialize(_passphrase).then(
        async () => {
            // credentials is initialized
            // public key hash
            let publicKeyHash = credentials.publicKeyHash(MsgHashAlgorithm.Sha3_256);
            let pubKeyHash = Web3Utils.uint8ArrayToHexString(publicKeyHash);
        },
        async (error) => {
            // error
        }
);

// Composing a transaction
let symId = "{10byte hex string}";
let nonce = "{nonce in hex string}";
let gas_price = "{gas price in hex string}";
let gas_limit = "{gas limit in hex string}";
let recipient_sym_id = "{10byte hex string}";
let hex_amount = "{amount in hex string}";
let data = "{data in hex string}";
let transaction_type = "{0x00(original), 0x01(sct) or 0x02(deposit)}";
let work_node_id = "{10byte hex string}";
let extra_data = "{extra data in hex string}";

let symTransaction = new SymTransaction(
    SymId.fromHexString(symId),
    UInt256.fromHexString(nonce),
    UInt256.fromHexString(gas_price),
    UInt256.fromHexString(gas_limit),
    SymId.fromHexString(recipient_sym_id),
    UInt256.fromHexString(hex_amount),
    Data.fromHexString(data),
    UInt8.fromHexString(transaction_type),
    new WorkNodes(SymId.fromHexString(work_node_id)),
    Data.fromHexString(extra_data)
);

// Signing a transaction
let chain_id = 1;
let fork_id = 0;

let sha3Sign = _credentials.sign(MsgHashAlgorithm.Sha3_256, chain_id, fork_id, symTransaction.selfPtr());
let signedMsg = sha3Sign.hexSignedMsg();

// Send a transaction
// use webd3 ...no
```

### 기여하기 <a href="#undefined" id="undefined"></a>

본 프로젝트는 Sallt 서비스 생태계 확대를 위해 개발되었습니다.

기여를 통해 프로젝트를 함께 발전시켜 주세요!

### 라이센스 <a href="#undefined" id="undefined"></a>

[Apache-2.0](https://choosealicense.com/licenses/apache-2.0/)

[MIT](https://choosealicense.com/licenses/mit/)

### 참고 <a href="#undefined" id="undefined"></a>

[Sallt](https://sallt.io/)





{% hint style="info" %}
하단의 내용은 Bundler가 포함된 Web Javascript 용 패키지 및 패키지의 사용 설명이므로 Node.js에서 사용할 경우에는 상단의 내용만 참고하시면 됩니다.
{% endhint %}

## Pepper Wallet (WASM) <a href="#pepper-wallet-wasm" id="pepper-wallet-wasm"></a>

* Pepper Wallet JS의 기능을 WASM으로 구현한 구현체 프로젝트입니다.
  * 구조체 구현이 가능한 구조이므로 Pepper Wallet JS보다 더 다양한 도구가 제공됩니다.
    * SCrypt, Aes128Ctr, Signer
* Wallet 관리 기능(예정)을 제공하며, 브라우저 내에서 합리적이고 효율적인 방법으로 보관하고 이용할 수 있는 기능을 제공합니다.
* SCrypt를 통해 256비트 길이의 Derived Key를 만들 수 있습니다.
* Aes128Ctr을 통해 키스토어의 Crypto(aes-128-ctr)을 디코드할 수 있습니다.
  * SCrypt로 만든 DK의 앞 128비트를 암호화 키로 사용
* Signer를 통해 SymVerse 네트워크 및 Ethereum 네트워크에서 사용되는 서명을 만들 수 있습니다.
* Pepper Wief (WASM)을 같이 사용하여 웹 자체적인 지갑 운용이 가능합니다.
  * Wief 포맷으로 데이터를 브라우저 저장소에 보관
  * Wief 포맷을 디코드하여 얻은 개인키를 사용하여 Signer로 서명
* Pepper Web3 (WASM)를 사용하여 트랜잭션을 직접 전송할 수 있습니다.
  * `let web3 = new Web3("{endpoint_url}");`\
    `let symWeb3 = web3.newSymWeb3();`
  * `let method = SymMethodContainer.{method_func}({params});`\
    `let response = await symWeb3.send(method);`

### 패키지 이름 <a href="#undefined" id="undefined"></a>

> pepper\_wallet\_rust

### 참고 <a href="#.1" id=".1"></a>

* [WebAssembly Demo](https://www.sallt.io/wasm\_test\_2/wasm.html)

### 사용예 <a href="#undefined" id="undefined"></a>

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* SymTransaction 도구를 초기화한다.
  * 데이터 타입은 지정된 타입을 사용한다.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* Credentials 도구의 sign 함수를 사용하여 트랜잭션을 서명한다.
* SymTransaction 타입과 EthTransaction 타입을 같이 지원하기 위해 포인터를 사용한다.
  * 포인터를 사용하지 않을 경우 signSymTx, signEthTx 함수를 사용한다.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* FunctionParameters 도구를 초기화하면서 함수의 이름을 지정한다.
* Function의 입력 파라미터의 순서에 맞게 타입과 값을 넣어준다.
* FunctionCoder 도구의 encode 메소드에 파라미터 객체와 해시 인코딩 규칙을 지정하여 인코딩한다.
* FunctionCoder 도구의 decode 메소드에 인코딩된 데이터와 파라미터를 순서대로 나열한 문자열을 넣어주면 파싱한 결과가 출력된다.

