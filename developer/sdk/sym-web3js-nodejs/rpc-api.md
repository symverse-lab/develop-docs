---
description: "아래는 Symverse Blockchain 의 RPC API 를 \b호출하는 방식에 대한 설명 문서입니다."
---

# RPC API 사용하기

블록체인 노드에서 제공하는 JSON RPC API를  Node 환경에서 편리하게 사용하기 위해 자바스크립트 언어로 변환한 코드입니다. 노드에서 제공하는 상세한 API 가 궁금하시다면 해당 링크를 참고해 주시기 바랍니다.

* [Default JSON RPC API](../../rpc-api/json-rpc-api.md)
* [SCT JSON RPC API](../../rpc-api/json-rpc-sct-api.md)

## **Smverse 노드 접속**

&#x20;API를 호출하기 위해선 노드 접속 코드를 작성해야합니다.

### **정의된 테스트넷(Test-net/메인넷(Main-net) 연결**&#x20;

```javascript
const symjs = SymJs();
const symverseNode = symjs.utils.fromNetwork('MAINNET'); // 테스트넷은 "TESTNET"을 넣습니다

symverseNode.network.connect(gsym.url).then(connectedMessage => {
    console.log(connectedMessage, 'connect success...')
}).catch(e => {
    // connected fail...
})
// Promise 객체 리턴
```

### 로컬 혹은 URL 직접 연결

```javascript
const symjs = SymJs();

symverseNode.network.connect('http://localhost:8584').then(connectedMessage => {
    console.log(connectedMessage, 'connect success...')
}).catch(e => {
    // connected fail...
})
```

## 블록체인 데이터 호출하기

블록체인에 기록된 정보(Block, Transaction, Citiz, SCT) 를 조회하기 위해서 아래와 같이 작성합니다.

### Block 조회

메인 블록 정보를 조회합니다.

```javascript
const result = await symjs.network.call.getBlockByHash("0x5223c591adad8b8f9c674e4c5ee03bb2a7584ec048180bef902901d338510660", true)

{
  activeCbNum: '0x22d',
  activeObNum: '0x0',
  activeWbNum: '0x1be',
  cbHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
  cbNum: '0x0',
  extraData: '0xd683010310846773796d86676f312e3230856c696e7578',
  gasLimit: '0xb3c36a0',
  gasUsed: '0x0',
  hash: '0x5223c591adad8b8f9c674e4c5ee03bb2a7584ec048180bef902901d338510660',
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  number: '0x1884f20',
  obHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
  obNum: '0x0',
  parentHash: '0x922692ee85bb751a4828b1d4df2e1fc67d743bb5d14c80017d36b74a5c1f91a1',
  primary: '0x0002231c150e06ff0002',
  receiptsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
  rewardRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
  rewards: [],
  sctRoot: '0x590fcf015c74cfe2f1aba6466122e494ebb81b4f8f47362787ad39220b9c98e1',
  signInfo: {
    mbNum: '0x1884f20',
    sign: '0x79cf6778bb8b9cfc64bfe32e93c0d1b300e092136987b61fdaef566a03ed025313611694486fc90ae50f468a0730acb1ec486955dd4ed87e546f18550c7ad2a9',
    extraData: '0x'
  },
  size: '0x3fc',
  stateRoot: '0x88172652495b8bcebc7eef525abd65873a5fd0e92369da71ffcb7971852fdc2c',
  tickStamp: '0xc3085b',
  timeStamp: '0x64a7a6ea',
  transactions: [],
  transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
  validatorInfos: [
    '0x00027b746c655e570002',
    '0x0002aba49d958e870002',
    '0x0002e4ddd6ce8d860002',
    '0x0002fdf5eee7e0d80002',
    '0x00028078716a635b0002',
    '0x0002a69f989189820002',
    '0x0002d2cbc3bcb5ae0002',
    '0x0002544d463e37300002',
    '0x0002a19a928b847d0002',
    '0x0002a69e979089810002',
    '0x0002fdf6efe8e0d90002',
    '0x0002f8f1e9e2dbd40002',
    '0x0002ccc5beb7afa80002',
    '0x0002100902fbf3b20002',
    '0x0002282119120b040002',
    '0x0002d1cac3bbb4ad0002',
    '0x0002756e2d261f180002',
    '0x000249423b332c250002',
    '0x00027f787069625b0002',
    '0x000229211a130c050002',
    '0x00024f474039322a0002',
    '0x0002534c453e362f0002',
    '0x00027a736c645d560002',
    '0x0002aea7a09991500002'
  ],
  wbHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
  wbNum: '0x1be'
}
```

### Balance 조회

블록에 기록된 symid의 amount 를 조회합니다.

<pre class="language-javascript"><code class="lang-javascript">const result = await symjs.network.call.getBalance(testSymId)
const sym = symjs.utils.toSym(result) // sym 단위로 변환
<strong>11618.746650933996
</strong></code></pre>

### Transaction 조회

트랜잭션 해시(hash) 기반으로 조회 합니다.

```javascript
const result = await symjs.network.call.getTransactionByHash("0x10e29156518e5015867f85a44ef528b222dcab812d439c77858f5f04dd81e6eb")

{
  blockHash: '0x7e391af4be549b066043b4e601da087a8c2e22a0c83e3ef3a7040896fcce0cd8',
  blockNumber: '0x18d1f09',
  from: '0x0002afc5b2b55a270002',
  gas: '0xbf68',
  gasPrice: '0x174876e800',
  hash: '0x10e29156518e5015867f85a44ef528b222dcab812d439c77858f5f04dd81e6eb',
  input: '0x',
  nonce: '0x65',
  to: '0x00020000000000010002',
  transactionIndex: '0x0',
  type: '0x0',
  workNodes: [ '0x0002cdc6bfb7b0a90002' ],
  value: '0x5',
  extraData: '0x',
  v: '0x0',
  r: '0xeba63613cb5359d58dd768557bb59696f312a52a68397cab7f3aa8e62bd70ded',
  s: '0x52ac995fba5e60dda9a9c0ef703a6b6267244710becc665f1a48a379973b5120'
}

```

### Transaction Receipt 조회

트랜잭션 해시(hash) 기반으로 영수증을 조회 합니다.

```javascript
let result = await symjs.network.call.getTransactionReceipt("0x10e29156518e5015867f85a44ef528b222dcab812d439c77858f5f04dd81e6eb")
{
  blockHash: '0x7e391af4be549b066043b4e601da087a8c2e22a0c83e3ef3a7040896fcce0cd8',
  blockNumber: '0x18d1f09',
  contractAddress: null,
  cumulativeGasUsed: '0xbf68',
  from: '0x0002afc5b2b55a270002',
  gasUsed: '0xbf68',
  logs: [],
  logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
  status: '0x1',
  to: '0x00020000000000010002',
  transactionHash: '0x10e29156518e5015867f85a44ef528b222dcab812d439c77858f5f04dd81e6eb',
  transactionIndex: '0x0'
}

```

### Transaction Nonce 조회

유저의 SymID가 현재까지 발행한 트랜잭션 count를 조회합니다.

```javascript
const result = await symjs.network.call.getTransactionCount(testSymId, PENDING_BLOCK)
2412
```

### Citizen 정보 조회

Sym ID 기반으로 Citizen 정보를 조회 합니다

```javascript
const result = await symjs.network.call.citizen.getCitizenBySymID('0x00024f48413a322b0002');

{
  ca: '0x00020000000000010002',
  aKeyPubH: '0xe254b299769d0cc780d6f685986dedc5f847bca7', // 해당 symid의 public hash key
  nonce: '0x0',
  vFlag: '0x0',
  country: '0x0',
  status: '0x1',
  credit: '0x0',
  role: '0x1',
  refCode: '0x0',
  writeTime: '0x0'
}
```

## SCT 정보 조회

### SCT21 (ERC20 Like Token) 조회

SCT API를 통해 생성된 Token의 정보를 조회합니다

<pre class="language-javascript"><code class="lang-javascript"><strong>const result = await symjs.network.call.sct.getContract('0xffa5f6161966dde56342');
</strong>
{
  creator: '0x0002aea7a09991500002',
  lock: '0x0',
  name: 'SYMVERSE',
  owner: '0x0002aea7a09991500002',
  state: 'active',
  stateCode: '0x0',
  symbol: 'SYM',
  total: '0x3635c9adc5dea00000',
  type: 'sct21'
}
</code></pre>

### SCT30 (ERC721 Like Token) 조회

SCT API를 통해 생성된 Token의 정보를 조회합니다

<pre class="language-javascript"><code class="lang-javascript"><strong>const result = await symjs.network.call.sct.getContract('0xff99634a959fa3347f22');
</strong>
{
  creator: '0x00024f48413a322b0002',
  name: 'COMY_TOKEN_19_0x0002d8aef146424b0002',
  owner: '0x00024f48413a322b0002',
  state: 'active',
  stateCode: '0x0',
  symbol: 'COMY',
  total: '0x4',
  type: 'sct30'
}
</code></pre>

### SCT20 (ERC20 Like Token) Balance 조회

블록에 기록된 SCT기반의 amount를 조회합니다.

<pre class="language-javascript"><code class="lang-javascript"><strong>const result = await symjs.network.call.sct.getContractAccount('0xffa5f6161966dde56342', '0x0002aea7a09991500002');
</strong>const amount symjs.utils.toSym(result.balance)

1000
</code></pre>

### SCT30 (ERC721 Like Token) Items 조회

블록에 기록된 SCT기반의 item을 조회합니다.

```javascript
const result = await symjs.network.call.sct.getContractAccount('0xff99634a959fa3347f22, '0x0002d8aef146424b0002');

[ 1, 2, 3, 4 ]
```

