# Multichain KOR

## 소개

다중블록체인 구조는 거래내역을 저장하는 메인 블록체인과 블록체인 운영에 관계되는 여러 서브 블록체인으로 구성된다. 이는 다양한 데이터구조를 단순화시키며 복잡한 비즈니스모델을 체계적인 데이터베이스로 바꾸어 주는 장점을 가지고 있다.

**Contents**

* blockchain의 종류
* 내부 체인 연결
* 블록-검증
* Block Structure
  * Main Block Structure
  * Warrant Block Structure
  * Citizen Block Structure
  * Oracle Block Structure

## Block Chain의 종류

* Main block chain: 단순 거래, 블록체인 기여도에 따른 보상 내역 등을 기록
* Citizen block chain: SymID 별 계정 정보 및 업데이트 정보 기록
* Warrant block chain: 인증 노드 선출 정보 기록
* Oracle block chain: 운영 정책 파라미터, 토큰간 환율 정보 등 기록
* Future block chain: 예약 거래 기록 (개발중)

## 내부 체인 연결 (Inter-connection Of Chains)

Main Block은 거래 내역이나 기록 사항이 없더라도 보안성 강화를 위해 일정 주기마다 생성되며, Sub Block은 기록할 데이터가 있는 경우 Main Block과 함께 생성된다.

Sub Block은 Main Block보다 데이터가 적어 상대적으로 조작의 위험성이 높다. 따라서 Sub Block이 생성될 때 Main Block의 정보(Number)를 블록에 기록하고, 해당 Sub Block 정보(Number, Hash)를 Main Block에 기록함으로써 임의로 Sub Block만 조작할 수 없도록 서로 연결하여 관리된다. (Main Block의 CBHash, WBHash, OBHash, CBNum, WBNum, OBNum 과 각 Sub Block의 RelatedMBNum 참고)

![multichain1](https://github.com/symverse-lab/Document/wiki/document/image/multichain1.png)

여러 개의 블록들은 다수의 내부 체인으로 연결되며 DAG([Directed Acyclic Graph](https://en.wikipedia.org/wiki/Directed\_acyclic\_graph))를 그리며 성장해 감으로 인해 비가역성을 확보한다. Sub Block Chain은 넝쿨 식물처럼 Main Chain을 지주삼아 같이 자라는 방식이다. 따라서 특정 체인의 특정 블록을 조작 하기위해서는 메인 블록 체인의 최초블록부터 시작해서 주기적으로 변경되는 불특정 다수의 보증 노드들의 전자서명을 모두 위조해야 한다.

![multichain2](https://github.com/symverse-lab/Document/wiki/document/image/multichain2.png)

또한 Main Block이 생성될 때 유효한 Sub Block의 정보(Number)를 기록함으로써 Main Block 생성 시점에 활동 중인 Sub Block의 검색이 가능하다.

모든 Sub Block은 생성시기와 적용 시기가 다를 수 있다.

아래 그림에서 Main Block M2 생성시 최종 Citizen Block은 C1이다. 즉 C1에 기록된 내용은 Main Block M2부터 반영된다는 의미이다.

아래 그림에서 Main Block M3부터 활동한 Warrant Node들은 W1에 기록되어 있다는 것을 알 수 있다. Warrant Block은 해당 Warrant Block을 적용하기 몇 블록 이전에 생성된다. (예를 들어 Main Block Number가 100에서 200까지 적용할 Warrant Block은 Main Block Number가 90일때 생성된다.) 이를 관리하기위해서 Warrant Block에는 각 Warrant Block의 적용 주기와 Warrant Node 임기가 포함되어 있다.

> 참고 : Main Block의 ActiveCBNum, ActiveWBNum, ActiveOBNum

![multichain3](https://github.com/symverse-lab/Document/wiki/document/image/multichain3.png)

## 블록 검증

블록의 검증은 블록 내에 포함된 서명 값을 검증함으로써 이루어진다. 블록 내의 서명은 결합 서명 방식을 사용하며, 이는 블록 합의에 찬성한 보증 노드들이 생성한 서명을 결합하여 생성된다.

![multichain4](https://github.com/symverse-lab/Document/wiki/document/image/multichain4.png)

이렇게 생성된 서명을 검증하기 위해서는 블록 합의에 찬성한 전체 보증 노드들의 신원을 확인할 수 있어야만 하며, 이는 블록을 조작하기 위해서는 결합 서명 생성에 사용된 전체 보증 노드들의 전자서명을 모두 위조해야 한다는 것을 의미한다.

![multichain5](https://github.com/symverse-lab/Document/wiki/document/image/multichain5.png)

## Block Structure

* Data size

| **Type**          | **Size(Bytes)** |
| ----------------- | --------------: |
| uint64            |               8 |
| common.Address    |              10 |
| Common.Hash       |              32 |
| common.PubKeyHash |              20 |
| \*big.Int         |              가변 |
| BKeyPub           |              65 |
| BKeyPubSigned     |             128 |
| Sign              |              64 |
| Bloom             |             256 |
| Extra             |             256 |
| Payload           |           32000 |
| <p><br></p>       |                 |

### Main Block Structure

| **Name**       | **Type**      | **Description**        |
| -------------- | ------------- | ---------------------- |
| header         | \*Header      | 헤더                     |
| transactions   | Transactions  | 블록 내의 트랜잭션 리스트         |
| rewards        | Rewards       | 블록 내의 보상 내역            |
| validatorInfos | ValidatorInfo | 블록 합의에 찬성한 보증 노드 정보 내역 |
| signInfo       | \*SignInfo    | 블록 검증을 위한 서명 정보        |

* Header Structure in Main Block

| **Name**    | **Type**       | **Json Name**    | **Description**                                                     |
| ----------- | -------------- | ---------------- | ------------------------------------------------------------------- |
| ParentHash  | Common.Hash    | parentHash       | 부모 블록 헤더의 해시값                                                       |
| Primary     | Common.Address | primary          | 블록 제안자의 SymID                                                       |
| Root        | Common.Hash    | stateRoot        | 상태 트리 루트의 해시값                                                       |
| SctRoot     | Common.Hash    | sctStateRoot     | SCT 트리 루트의 해시값                                                      |
| TxHash      | Common.Hash    | transactionsRoot | 트랜잭션 트리 루트의 해시값                                                     |
| RewardHash  | Common.Hash    | rewardRoot       | 보상 트리 루트의 해시값                                                       |
| ReceiptHash | Common.Hash    | receiptsRoot     | 영수증 트리 루트의 해시값                                                      |
| CBHash      | Common.Hash    | cbHash           | 블록과 연결된 시티즌 블록의 해시값                                                 |
| WBHash      | Common.Hash    | wbHash           | 블록과 연결된 워런트 블록의 해시값                                                 |
| OBHash      | Common.Hash    | obHash           | 블록과 연결된 오라클 블록의 해시값                                                 |
| Number      | \*big.Int      | number           | 블록의 높이 (블록 생성 주기와 무관하게 트랜잭션이 존재하거나, 블록 생성 주기에 맞춰 블록 생성시 마다 카운트되는 값) |
| CBNum       | \*big.Int      | cbNum            | 블록과 연결된 시티즌 블록의 높이                                                  |
| WBNum       | \*big.Int      | wbNum            | 블록과 연결된 워런트 블록의 높이                                                  |
| OBNum       | \*big.Int      | obNum            | 블록과 연결된 오라클 블록의 높이                                                  |
| ActiveCBNum | \*big.Int      | activeCbNum      | 최종 생성된 시티즌 블록의 높이 번호                                                |
| ActiveWBNum | \*big.Int      | activeWbNum      | 블록을 검증하기 위해 필요한 워런트 블록의 높이 번호                                       |
| ActiveOBNum | \*big.Int      | activeObNum      | 현재 적용 중인 오라클 블록의 높이 번호                                              |
| Bloom       | Bloom          | logsBloom        | 트랜잭션들의 로그들을 블룸필터로 표현                                                |
| TickStamp   | \*big.Int      | tickStamp        | 블록 생성 주기(BlockGenTick) 마다 카운트 되는 값                                  |
| Time        | \*big.Int      | timeStamp        | 타임스탬프 epoch값 (UTC)                                                  |
| GasLimit    | uint64         | gasLimit         | 블록당 현재 지급 가능한 최대 가스 총합                                              |
| GasUsed     | uint64         | gasUsed          | 메인 블록 내에 트랜잭션에 의해 사용된 가스의 총합                                        |
| Extra       | \[]byte        | extraData        | 추가 정보 (optional)                                                    |

* Transactions in Main Block

| **Name**     | **Type**          | **Json Name** | **Description**                          |
| ------------ | ----------------- | ------------- | ---------------------------------------- |
| Publisher    | common.Address    | from          | 발신자 주소                                   |
| AccountNonce | uint64            | nonce         | 트랜잭션 일련번호                                |
| Price        | \*big.Int         | gasPrice      | 발신자가 거래를 처리하기 위하여 지급하는 가스가격              |
| GasLimit     | uint64            | gas           | 거래를 처리하는데 허용되는 최대 가스량                    |
| Recipient    | common.Address    | to            | 수신자 주소 (계정 또는 컨트랙트 주소)                   |
| Amount       | \*big.Int         | value         | 수신자에게 보내는 코인 양                           |
| Payload      | \[]byte           | input         | 임의의 메시지 or 계약에 대한 함수 호출 or 계약을 생성하는 코드   |
| Type         | uint64            | type          | 트랜잭션 종류 ( 0: original, 1:sct, 2:deposit) |
| WorkNode     | \[]common.Address | workNodes     | 트랜잭션을 중개하는 워크노드의 SymID                   |
| Extra        | \[]byte           | extraData     | 추가 정보 (optional)                         |
| V            | \*big.Int         | v             | ECDSA 서명 구성 요소                           |
| R            | \*big.Int         | r             | ECDSA 서명 구성 요소                           |
| S            | \*big.Int         | s             | ECDSA 서명 구성 요소                           |

* Rewards in Main Block

| **Name** | **Type**       | **Json Name** | **Description**   |
| -------- | -------------- | ------------- | ----------------- |
| SymId    | Common.Address | symId         | 보상을 받을 계정의 Sym ID |
| Value    | \*big.Int      | value         | 보상을 받을 양          |
| Type     | uint           | type          | 보상 종류 (Reserved)  |
| Extra    | \[]byte        | extraData     | 추가 정보 (optional)  |

* ValidatorInfos in Main Block

| **Name** | **Type**       | **Json Name** | **Description**          |
| -------- | -------------- | ------------- | ------------------------ |
| SymId    | Common.Address | symId         | 블록 합의에 찬성한 보증 노드의 Sym ID |
| Extra    | \[]byte        | extraData     | 추가 정보 (optional)         |

* SignInfos in Main Block

| **Name** | **Type**  | **Json Name** | **Description**                           |
| -------- | --------- | ------------- | ----------------------------------------- |
| MBNum    | \*big.Int | mbNum         | 서명 정보와 관련 된 메인 블록의 높이 번호 (블록 동기화시 사용)     |
| Sign     | \[]byte   | sign          | 결합 서명 값(블록 검증자가 생성한 서명 + 프라이머리 서명을 결합한 값) |
| Extra    | \[]byte   | extraData     | 추가 정보 (optional)                          |

```
-   검증자 서명 범위 (Header, Transactions, Rewards)
-   프라이머리 서명 범위 (Header, Transactions, Rewards, ValidatorInfos)
```

### Warrant Block Structure

| **Name** | **Type**        | **Description**    |
| -------- | --------------- | ------------------ |
| header   | \*WarrantHeader | 헤더                 |
| warrants | \[]\*Warrant    | 블록 내의 보증 노드 정보 리스트 |

* Header in Warrant Block

| **Name**     | **Type**    | **Json Name** | **Description**                                |
| ------------ | ----------- | ------------- | ---------------------------------------------- |
| ParentHash   | Common.Hash | parentHash    | 부모 블록 헤더의 해시값                                  |
| RelatedMBNum | \*big.Int   | relatedMbNum  | 블록과 관련 된 메인 블록 높이 번호                           |
| WarrantHash  | Common.Hash | warrantsRoot  | 보증노드정보 트리 루트의 해시값                              |
| Number       | \*big.Int   | number        | 현재 블록의 높이                                      |
| BeginMBNum   | \*big.Int   | beginMbNum    | 블록내에 포함 된 보증 노드 정보를 이용하여 검증 할 수 있는 메인 블록 시작 번호 |
| EndMBNum     | \*big.Int   | endMbNum      | 블록내에 포함 된 보증 노드 정보를 이용하여 검증 할 수 있는 메인 블록 끝 번호  |
| Extra        | \[]byte     | extraData     | 추가 정보 (optional)                               |

* Warrant Data in Warrant Block

| **Name**      | **Type**         | **Json Name** | **Description**                     |
| ------------- | ---------------- | ------------- | ----------------------------------- |
| SymId         | \*Common.Address | symId         | 보증 노드로 활동 할 노드의 Sym ID              |
| BKeyPub       | \[]byte          | bKeyPub       | 블록 검증용 공개키 값                        |
| BKeyPubSigned | \[]byte          | bKeyPubSigned | 블록 검증용 공개키를 어카운트 키로 서명한 값           |
| Group         | \*uint64         | group         | 보증 노드 그룹 (A or B)                   |
| Elected       | \*big.Int        | elected       | 해당 노드가 보증 노드로 활동 할 수 있는 메인 블록 시작 번호 |
| Finished      | \*big.Int        | finished      | 해당 노드가 보증 노드로 활동 할 수 있는 메인 블록 끝 번호  |
| Extra         | \[]byte          | extraData     | 추가 정보 (optional)                    |

### Citizen Block Structure

| **Name**     | **Type**            | **Description**       |
| ------------ | ------------------- | --------------------- |
| **header**   | **\*CitizenHeader** | **헤더**                |
| **citizens** | **\[]\*Citizen**    | **블록 내의 시티즌 데이터 리스트** |

* Header in Citizen Block

| **Name**     | **Type**    | **Json Name** | **Description**     |
| ------------ | ----------- | ------------- | ------------------- |
| ParentHash   | Common.Hash | parentHash    | 부모 블록 헤더의 해시값       |
| Root         | Common.Hash | stateRoot     | 시티즌 상태 트리 루트의 해시값   |
| RelatedMBNum | \*big.Int   | relatedMbNum  | 블록과 관련 된 메인블록 높이 번호 |
| CitizenHash  | Common.Hash | citizensRoot  | 시티즌 정보 트리 루트의 해시값   |
| Number       | \*big.Int   | number        | 블록의 높이              |
| Extra        | \[]byte     | extraData     | 추가 정보 (optional)    |

* Citizen Data in Citizen Block

| **Name**     | **Type**          | **Json Name** | **Description**                                                                    |
| ------------ | ----------------- | ------------- | ---------------------------------------------------------------------------------- |
| Publisher    | common.Address    | from          | 시티즌 정보를 발행한 CA의 Sym ID                                                             |
| Recipient    | \*common.Address  | to            | Reserved                                                                           |
| AccountNonce | uint64            | nonce         | 시티즌 일련번호                                                                           |
| SymId        | common.Address    | symId         | 신규 등록 또는 업데이트할 Citizen의 Sym ID                                                     |
| AKeyPubH     | common.PubKeyHash | aKeyPubH      | 20bytes, Account 공개키 hash (20bytes cut)                                            |
| VFlag        | \*uint64          | vFlag         | 검증 레벨                                                                              |
| Country      | \*uint64          | country       | 국가 코드                                                                              |
| Status       | \*uint64          | status        | 상태 코드 ( 0x01: Active, 0x02 :Revoked, 0x03 : Locked, 0x04 : Marked                  |
| Credit       | \*uint64          | credit        | 신용도                                                                                |
| Role         | \*uint64          | role          | 역할 ( 0xf0f0 : Master CA, 0xf0f1 : CA, 0xf0f2 : Oracle, 0x1 : general, 0x02 : Dapp) |
| RefCode      | \*uint64          | refCode       | 발급자 참조 코드                                                                          |
| WriteTime    | \*big.Int         | writeTime     | 신규 등록 또는 업데이트 요청 시간                                                                |
| Extra        | \[]byte           | extraData     | 추가 정보 (optional)                                                                   |
| V            | \*big.Int         | v             | ECDSA 서명 구성 요소                                                                     |
| R            | \*big.Int         | r             | ECDSA 서명 구성 요소                                                                     |
| S            | \*big.Int         | s             | ECDSA 서명 구성 요소                                                                     |

### Oracle Block Structure

| **Name** | **Type**       | **Description**   |
| -------- | -------------- | ----------------- |
| header   | \*OracleHeader | 헤더                |
| citizens | \[]\*Oracle    | 블록 내의 오라클 데이터 리스트 |

* Header in Oracle Block

| **Name**     | **Type**    | **Json Name** | **Description**     |
| ------------ | ----------- | ------------- | ------------------- |
| ParentHash   | Common.Hash | parentHash    | 부모 블록 헤더의 해시값       |
| Root         | Common.Hash | stateRoot     | 오라클 상태 트리 루트의 해시값   |
| RelatedMBNum | \*big.Int   | relatedMbNum  | 블록과 관련 된 메인블록 높이 번호 |
| OracleHash   | Common.Hash | citizensRoot  | 오라클 정보 트리 루트의 해시값   |
| Number       | \*big.Int   | number        | 블록의 높이              |
| Extra        | \[]byte     | extraData     | 추가 정보 (optional)    |

* Oracle Data in Oracle Block

| **Name**     | **Type**         | **Json Name** | **Description**  |
| ------------ | ---------------- | ------------- | ---------------- |
| Publisher    | common.Address   | from          | 오라클라이저의 Sym ID   |
| Recipient    | \*common.Address | to            | Reserved         |
| AccountNonce | uint64           | nonce         | 오라클 일련번호         |
| Payload      | \[]byte          | payload       | 오라클 파라미터 정보      |
| Extra        | \[]byte          | extraData     | 추가 정보 (optional) |
| V            | \*big.Int        | v             | ECDSA 서명 구성 요소   |
| R            | \*big.Int        | r             | ECDSA 서명 구성 요소   |
| S            | \*big.Int        | s             | ECDSA 서명 구성 요소   |
