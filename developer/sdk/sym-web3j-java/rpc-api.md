---
description: "아래는 Symverse Blockchain 의 RPC API 를 \b호출하는 방식에 대한 설명 문서입니다."
---

# RPC API 사용하기

&#x20;블록체인 노드에서 제공하는 JSON RPC API를  JAVA 언어로 변환한 코드입니다. 노드에서 제공하는 상세한 API 가 궁금하시다면 해당 링크를 참고해 주시기 바랍니다.

* [Default JSON RPC API](../../rpc-api/json-rpc-api.md)
* [SCT JSON RPC API](../../rpc-api/json-rpc-sct-api.md)

## **Symverse 노드 접속**

API를 호출하기 위해선 아래 Service 코드를 작성해야합니다.

{% tabs %}
{% tab title="퍼블릭 노드 연결" %}
```java
// 테스트넷 노드 연결 시 Network.TEST_NET 삽입
Network network = Network.MAIN_NET 
// Network network = Network.TEST_NET 

SymHttpService httpService = new SymHttpService(PublicWorkNode.fromNetwork(network)
GSymCitizen gsym = GSymCitizen.build(httpService);
```
{% endtab %}

{% tab title="노드 직접 지정" %}
```java
// 노드의 URL 과 symbase를 입력합니다
GsymNode node = new GsymNode("http://localhost:8545", "0x000000000000001")

SymHttpService httpService = new SymHttpService(node);
GSymCitizen gsym = GSymCitizen.build(httpService);
```
{% endtab %}
{% endtabs %}

메인넷과 테스트넷은 퍼블릭 체인으로써 각 고정으로 사용하는 호스트 정보를 라이브러리 내부에서 관리하고 있습니다.\
따라서 개발자는 실제 호스트 주소의 정보를 알 필요 없이 `PublicWorkNode.fromNetwork`함수를 통해 특정 네트워크에 해당하는 호스트를 랜덤으로 요청할 수 있습니다.



## 블록체인 데이터 호출하기

블록체인에 기록된 정보(Block, Transaction, Citiz, SCT) 를 조회하기 위해서 아래와 같이 작성합니다.

### Block 조회

메인 블록 정보를 조회 합니다

{% tabs %}
{% tab title="Code" %}
```java
// 조회할 블록 number 지정
DefaultBlockParameterNumber number = DefaultBlockParameter.valueOf(SymUtil.big(1001);
// 가장 마지막 블록 조회 시
//DefaultBlockParameterName number = DefaultBlockParameterName.LATEST;

// 두번째 파라미터는 블록에 포함된 트랜잭션 상세 여부
SymBlock block = gsym.symGetBlockByNumber((number), true).send();
```
{% endtab %}

{% tab title="Example Result" %}
```java
SymBlock.Block(
    number=0x1f0cac, 
    hash=0xa37e219f4c6cf49e81ca33c1aa9aa3e25d6cb81ce89cff708e01b95ce49993e2, 
    parentHash=0x18e2c95fb2aa8c01db7a32f0f9a07b72053de3d64c083add59d8dd88ebdfd655, 
    nonce=null, 
    logsBloom=0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000, transactionsRoot=0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421, 
    stateRoot=0x0baa81cd96f3bc95dfcd18eba4eb2e7f9bbbde7d18262325ebd41cc9884df567, 
    sctRoot=0xf211e8b5d8214174eb9474b554e671f68f18049e607deacaa1df92d12a53fa0e, 
    receiptsRoot=0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421, 
    cbHash=0x0000000000000000000000000000000000000000000000000000000000000000, 
    cbNum=0x0, 
    activeCbNum=0x193, 
    wbHash=0x0000000000000000000000000000000000000000000000000000000000000000, 
    wbNum=0x24, 
    activeWbNum=0x24, 
    obHash=0x0000000000000000000000000000000000000000000000000000000000000000, 
    obNum=0x0, 
    activeObNum=0x0, 
    primary=0x0002677b269832ee0002, 
    extraData=0xd883010014846773796d88676f312e31302e37856c696e7578, size=0x399, 
    gasLimit=0xb3c36a0, 
    gasUsed=0x0, 
    tickStamp=0x0, 
    timeStamp=0x5dde3f0a, 
    signInfo=SymBlock.SignInfo(mbNum=0x1f0cac, sign=0x723043762b5e8af310147db34d3fb85963f9edb3f40e174097a42a4cbadf86138b5e28656a6baa62f8b9a114d21f903bea156377e78f01d82bb0ea4779645ef3, extraData=0x), validatorInfos=[0x00021ccfce89f8040002, 0x0002a6cbb88e480a0002, 0x0002025f3094f9740002, 0x0002f5682eda003e0002, 0x000243950812a7240002, 0x000293550d7a23ae0002, 0x0002881ef9e467cb0002, 0x0002810866f538160002, 0x00023831a108c4010002, 0x0002d3cadb945ad80002, 0x0002e363f8d3fc520002, 0x00021e6c845b04750002, 0x0002b1f2ffef5ca20002, 0x0002caaffcbb81660002, 0x000264ca79064b870002, 0x00028a5274ee0a4e0002, 0x00026d97036e06050002], transactions=[]
)
```
{% endtab %}
{% endtabs %}

### Transaction Nonce 조회

유저의 SymID가 현재까지 발행한 트랜잭션 count를 조회합니다.

{% tabs %}
{% tab title="Code" %}
```java
SymGetTransactionCount count = gSymWeb3j.symGetTransactionCount(SymUtil.appendPrefixHex("0002aea7a09991500002"), DefaultBlockParameterName.LATEST).send();
```
{% endtab %}

{% tab title="Example Result" %}
```
40
```
{% endtab %}
{% endtabs %}

### Transaction 조회

트랜잭션 해쉬(hash) 기반으로 트랜젹션 기록을 조회 합니다

{% tabs %}
{% tab title="Code" %}
```java
SymTransaction transaction = gsym.symGetTransactionByHash("0x10aac13794eae0488dd034dc8a3774714fb9eccafe931a40429c92604ecace0f").send();
```
{% endtab %}

{% tab title="Example Result" %}
```java
Transaction(
    blockHash=0xfe5c04086a35d5e824df734dde4f3b58b92a1778ba4f6fcabeb4b3c4feb3033a, 
    blockNumber=0xb3d64, 
    from=0x0002eb055e4a51cd0002, 
    gas=0xbf68, 
    gasPrice=0x174876e800, 
    hash=0x10aac13794eae0488dd034dc8a3774714fb9eccafe931a40429c92604ecace0f, 
    input=0x, 
    nonce=0x1, 
    to=0x00026ff4874754380002, 
    transactionIndex=0x0, 
    type=0x0, 
    workNodes=[0x0002f9f1eae3dcd40002], 
    value=0x1b1ae4d6e2ef500000, 
    extraData=0x, 
    v=0x0, 
    r=0x5a0932bdf5022e175f0d217219005abec633b777247f9f5de69f66463e838333, 
    s=0x29a3e90070954b3d5eeba651f3bc11fc11353e23c8040459789a808ddcfa4cf5
)
```
{% endtab %}
{% endtabs %}

### Transaction Receipt 조회

트랜잭션 해쉬(hash) 기반으로 트랜젹션 영수증을 조회 합니다

{% tabs %}
{% tab title="Code" %}
```java
SymTransactionReceipt receipt = gsym.symGetTransactionReceipt("0x10aac13794eae0488dd034dc8a3774714fb9eccafe931a40429c92604ecace0f").send();
```
{% endtab %}

{% tab title="Example Result" %}
```java
TransactionReceipt(
    transactionHash=0x10aac13794eae0488dd034dc8a3774714fb9eccafe931a40429c92604ecace0f, 
    transactionIndex=0x0, 
    blockHash=0xfe5c04086a35d5e824df734dde4f3b58b92a1778ba4f6fcabeb4b3c4feb3033a, 
    blockNumber=0xb3d64, 
    cumulativeGasUsed=0xbf68, 
    gasUsed=0xbf68, 
    contractAddress=null, 
    root=null, 
    status=0x1, 
    from=0x0002eb055e4a51cd0002, 
    to=0x00026ff4874754380002, 
    errCode=null, 
    logs=[], 
    logsBloom=0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000, 
    revertReason=null
)
```
{% endtab %}
{% endtabs %}

### DID 조회

SymID 기반으로 DID 정보를 조회 합니다

{% tabs %}
{% tab title="Code" %}
```java
SymGetCitizen citizen = gSymWeb3j.citizenBySymId("0x0002eb055e4a51cd0002", DefaultBlockParameterName.LATEST).send();
```
{% endtab %}

{% tab title="Example Result" %}
```java
Citizen(
    ca=0x00020000000000010002, 
    aKeyPubH=0xa8cca06ac893262a806d99945e05d9c105d5e6ea, 
    nonce=0x4, 
    vFlag=0x0, 
    country=0x0, 
    status=CitizenStatus.ACTIVE(status=0x1, available=true), 
    role=CitizenRole.GENERAL(role=0x1), 
    credit=0x0, 
    refCode=0x0, 
    writeTime=0x323032322d30352d31385430333a35303a34335a
)
```
{% endtab %}
{% endtabs %}

### SCT 조회

SCT API를 통해 생성된 Token의 정보를 조회합니다

{% tabs %}
{% tab title="Code" %}
```java
// SCT 주소
String testSctAddress = "0xff89fc8f8050e05fff64";

SymSCTGetContract contract = gSymWeb3j.sctGetContract(testSctAddress).send();
```
{% endtab %}

{% tab title="Example Result" %}
```java
SymSCTGetContract.SCTContract(
    cpoint=null, 
    creator=0x0002eb055e4a51cd0002, 
    name=Symverse, 
    owner=0x0002eb055e4a51cd0002, 
    state=active, 
    stateCode=0x0, 
    symbol=BTC, 
    total=0x33b2e3c9fd0803ce8000000, 
    lock=0x56bc75e2d63100000, 
    type=sct21
)
```
{% endtab %}
{% endtabs %}

## 트랜잭션 전송하기

&#x20;트랜잭션을 전송하기 위해서는 반드시 서명에 필요한 Keystore가 필요합니다. 때문에 API를 사용하기 위해서는 지갑에서  Keystore 를 Export 한 뒤 사용해주시기 바랍니다.



### 기본 트랜잭션 전송

트랜잭션을 전송하기 위해서는 `symGetTransactionCount` 함수를 통해 반드시 트랜잭션의 `Nonce`값을 작성해야합니다. ( 이중 지불 문제 방지 )

{% tabs %}
{% tab title="Code" %}
```java
// symid nonce 값 구하기
SymGetTransactionCount nonce = gSymWeb3j.symGetTransactionCount(walletFile.getSymId(), DefaultBlockParameterName.PENDING).send();

// 노드 선택
GsymNode workNode = PublicWorkNode.fromNetwork(Network.TEST_NET);

// 트랜잭션 메시지 서명
RLPRawTransaction rawTransaction = new RLPRawTransaction.Builder()
        .setFrom(walletFile.getSymId()) // 보내는 SymID
        .setTo(SymUtil.appendPrefixHex("00024f48413a322b0002")) // 받는 SymID
        .setNonce(nonce.getTransactionCount()) 
        .setValue(SymUtil.toSym(100)) // SYM 개수
        .setWarrantNode(workNode.getNodeBaseSymId()) 
        .build(credentials, Network.TEST_NET.getChainId());

// 트랜잭션 전송
SymSendRawTransaction symSendRawTransaction = gSymWeb3j.symSendRawTransaction(rawTransaction.hexSignMessage()).send();

if ( symSendRawTransaction.hasError()){
    //에러처리
}

System.out.println(symSendRawTransaction.getTransactionHash());
```
{% endtab %}

{% tab title="Example Result" %}
```java
// 트랜잭션 Hash
0x19603080e8d179aadf9a60c5b9c191d98da0fa235f4db10f9980026edf09ca3a
```
{% endtab %}
{% endtabs %}

### SCT 트랜잭션 전송

SCT의 경우 Sct.Maker 가 존재합니다. 해당 클래스를 통해서 손 쉽게 파라미터를 작성할 수 있습니다.

아래 예제는 `SCT21`에 대한 생성 코드 입니다.

{% tabs %}
{% tab title="Code" %}
```java
// symid nonce 값 구하기
SymGetTransactionCount nonce = gSymWeb3j.symGetTransactionCount(walletFile.getSymId(), DefaultBlockParameterName.PENDING).send();

// 노드 선택
GsymNode workNode = PublicWorkNode.fromNetwork(Network.TEST_NET);

// "SCT21 생성" 파라미터 생성
Sct21Maker sct21Create = new Sct.Sct21Maker()
        .create(
        "SYMVERSE", 
        "SYM", 
        SymUtil.toSym(1000), 
        BigInteger.ZERO, 
        walletFile.getSymId()
)

// "SCT21 토큰 전송" 파라미터 생성
//new Sct.Sct21Maker().transfer("00024f48413a322b0002", SymUtil.toSym(5));


// SCT 메시지 서명
RLPRawTransaction rawTransaction = new RLPRawTransaction.Builder()
        .setFrom(walletFile.getSymId()) // 보내는 SymID
        .setSct(sct21Token)
        .setContractAddress(null)
        .setNonce(nonce.getTransactionCount()) 
        .setWarrantNode(workNode.getNodeBaseSymId()) 
        .build(credentials, Network.TEST_NET.getChainId());

// 트랜잭션 전송
SymSendRawTransaction sctSendRawTransaction = gSymWeb3j.symSendRawTransaction(rawTransaction.hexSignMessage()).send();

if ( symSendRawTransaction.hasError()){
    //에러처리
}

System.out.println(sctSendRawTransaction.getTransactionHash());
```
{% endtab %}

{% tab title="Example Result" %}
```java
// 트랜잭션 Hash
0x19603080e8d179aadf9a60c5b9c191d98da0fa235f4db10f9980026edf09ca3a
```
{% endtab %}
{% endtabs %}



