# SymSensus KOR

## SymSensus의 특징

SymSensus는 다음과 같은 특징을 가지고 있다.

* SymSensus의 설계는 게임이론에 기반하고 있다. 특히 사회선택이론의 가장 중요한 정리를 활용하여 구조설계(Mechanism Design)기법을 적용하였다.
* SymSensus는 거부권(Veto)을 포함한 투표방식의 PBFT(Practical Byzantine Fault Tolerant)를 사용한다.
* 합의하여 블록을 생성하는 Warrant Node는 A, B 두개의 그룹으로 구성되며 A그룹은 집단 거부권을 가지고 있다
* B그룹은 Work Node(합의에 참여하지 않는 Full Node)중에서 Autonomous Bench Marking Test를 통하여 선발한다.
* 기존 블록체인의 합의방식과 달리 블록생성 시 코인이 발행되지 않으며, 네트워크 기여도를 측정하여 1일 1회 코인이 분배된다.
* Primary 노드는 매 블록 마다 무작위로 결정된다.
* Multi Block 합의를 지원한다.
* SymSensus 전용 합의용 프로토콜(N2N)을 이용하여 Node간에 통신한다.

## SymSensus 알고리즘

SymSensus는 PBFT를 블록체인에 적용하기 적합한형태로 변형한 프로토콜이다. 다수의 Node로 구성된 Blockchain Network에서 합의하여 블록생성을 담당하는 Node들을 Warrant Node라 정의한다. Warrant Node는 F개의 악의적인 Node를 견디기 위한 3F + 1 로 구성되며 A그룹과 B그룹으로 나뉘어 운영된다.

새로운 블록이 생성되기위해서는 B그룹에서 블록을 제안하고 A그룹의 동의를 얻어야만 한다. 각 그룹의 역할은 다음과 같다.

* A 그룹은 F+1 개의 노드로 구성되며 Verifier Node의 역할 만 수행한다. A 그룹은 SymVerse 재단 운영 규칙에 따라 선출된다.
* B 그룹은 2F 개의 Node로 구성되며 이중 1개의 Primary Node는 블록 생성을 제안하는 역할을 하고 나머지는 Verifier Node의 역할을 한다. B그룹은 실시간 BMT를 통하여 선출한다.

Warrant Node는 역할에 따라 다음과 같이 나뉜다.

* Primary Node : B그룹에 속한 Node중 블록생성을 제안하는 노드이다.
* Verifier Node : Primary Node를 제외 한 모든 Warrant Node는 Verifier Node로서 제안된 블록을 검증한다.

![SymSensus1](https://github.com/symverse-lab/Document/wiki/document/image/SymSensus1.png)

합의 시 A그룹 노드 수의 1/2+1이 넘는 의견은 A그룹 전체의 의견으로 보고 합의한다. 이는 동일한 투표결과를 보여주는 집단적인 거부권(Veto)을 행사할 수 있도록 한 것이다.

*   가결 조건 (합의가 되는 조건)

    A그룹 노드 수의 1/2+1 개 노드가 찬성하고 B그룹 노드 수의 1/2개 이상의 Node가 찬성하면 합의된 것으로 본다.
* 부결 조건 (합의가 안되는 조건)
  * A그룹 노드 수의 1/2+1 개 노드가 반대하는 경우 (F/2+1 개 Node의 반대)
  * B그룹 노드 수의 1/2+1 개 노드가 반대하는 경우 (F+1 개 Node의 반대)

블록 생성은 Propose, Verify, Pre-Commit, Commit 단계를 거처 생성 및 확정된다. 각 단계 별로 가결 조건을 만족하면 다음 단계로 넘어 가고 부결 조건 중 하나이상 만족하면 Propose 단계부터 새로 시작한다.

* Propose 단계는 Primary가 유효한 블록을 생성하여 다른 노드에게 제안하는 단계이다. Primary는 Transaction, Reward등의 집합을 이용하여 블록을 구성하고 이를 포함한 Propose Message를 모든 Warrant Node에 전파한다. (Transaction의 공유는 Block생성 이전에 이루어 진다)
* Verify 단계는 제안받은 블록을 검증하는 단계이다. Verify 단계에서는 블록생성을 제안한 Node가 정당한지 확인 후, 제안된 블록의 유효성을 사전에 공유하고 있던 Transaction의 집합을 통해 검증한다. 검증을 마친 Node는 신규 블록 생성에 대한 찬성 또는 반대 의견을 포함한 Message를 구성하고 다른 노드에 전파한다. 신규 블록 생성을 동의하는 Node는 다음 단계로 진입하기 위해 다른 Node의 Message를 기다린다.
* Pre-Commit 단계에서 Primary는 찬성한 노드의 서명을 취합하고 하나의 서명으로 결합하여 다른 노드들에게 Pre-Commit Message를 보낸다.
* Commit 단계에서는 제안된 블록을 확정 짓는다. 블록을 확정 짓기 위해 다른 노드들의 Commit Message를 기다린다. 가결 조건을 만족하면 블록을 생성하고 전파한다.

## FE-VoW 알고리즘

VoW(View of Warrant)는 B그룹 중에서 Primary Node가 될 수 있는 후보 Node들의 그룹이며 V로 표기한다.

![SymSensus2](https://github.com/symverse-lab/Document/wiki/document/image/SymSensus2.png)

FE-VoW는 SymSensus와 같이 동작하며 Primary Node 선출 및 관리를 지원하는 알고리즘이며, 한번에 다수 Node의 장애 여부를 합의하기 적합한 형태로 설계되었다. FE-VoW는 이벤트 발생시 시작되며 종료 조건을 만족할 때까지 무한 반복된다.

*   VoW 합의 시작

    Warrant Node는 다음 이벤트 중 하나 이상 발행하면 VoW 합의를 진행한다.

    * Primary Node가 일정 시간 이상 Propose 나 Pre-Commit Message를 보내지 않을 시
    * Warrant Node의 연결 상태 변경 시
    * Suggest Message를 2/3+1 이상의 Warrant Node에서 수신 시

    각각의 Warrant Node는 합의 시작 시점이 다를 수 있다.
* VoW 합의 진행

![SymSensus3](https://github.com/symverse-lab/Document/wiki/document/image/SymSensus3.png)

Warrant Node는 VoW 합의가 시작되면 Main-Block을 포함한 모든 Block의 생성을 중지한다. B그룹에서 F를 제외하고 노드ID(SymID)가 담긴 V를 생성한다음 다른 Warrant Node들에게 Suggest Message를 합의 종료시까지 주기적으로 반복하여 Multicasting 한다. 이때 VoW 합의에 참여하지 않는 B그룹의 Node는 F로 간주한다. 또한 합의 완료 전까지 자신이 만든 V를 변경할 수 있다.

*   VoW 합의 종료

    다음 모든 조건이 만족하면 VoW 합의가 종료된다.

    * 2/3+1 이상의 Warrant Node가 VoW 합의에 참여해야 한다.
    * V에 포함된 Node별로 합의가 되어야한다.
    * V에 포함된 전체 Node의 수는 B그룹의 최소 정족 수를 넘어야한다.
    * 일정시간 동안 새로운 V가 없어야 한다. (V에 포함된 Node목록이 같으면 같은 V로 본다.)
*   Primary 선출

    노르웨이의 수학자는 Hylland는 투표방식과 관련해서 다음과 같은 정리를 했다. “시민주권사회에서 확률적인 독재자(비토권)의 투표방식은 어느 누구도 투표결과를 조작하여 이득을 얻을 수 없다.” Hylland의 결과를 BFT에 적용하면 다음과 같은 정리를 도출할 수 있다.

    `“Veto권이 존재하는 BFT 합의과정에서 Primary 노드(확률적 독재자)가 블록을 생성할 때 투표참가자들은 합의과정을 조작하여 이득을 얻을 수 없다”`

    SymSensus는 최종 Main-Block의 Hash값을 Seed값으로 한 Random Number로 VoW 중에서 Primary Node를 선정된다. 따라서 매 블록마다 Random하게 Primary가 변경된다.

## N2N Protocol

N2N(Node To Node) 프로토콜은 Warrant Node간의 합의 시 신뢰성 있는 통신을 지원해주는 프로토콜이다.

N2N 은 TCP상위 프로토콜이며 Warrant Boot Node에 접속하여 현재 활동중인 Warrant Node 목록을 얻어오고 각각의 Warrant Node와 1:1로 연결하여 통신한다. 모든 Warrant Node는 Warrant Boot Node의 기능을 포함하고 있다.

N2N은 다음의 기능을 제공한다.

*   Handshake

    SymID 및 최종 Warrant Block을 기반으로 하여 Warrant Node의 유효성을 판단하고 합의 전용 키를 이용하여 handshake를 진행한다.
*   Security (PKI)

    모든 Packet을 전자 서명하여 전송한다.
*   Node Share

    활동중인 Warrant Node 목록을 서로 공유할 수 있는 기능을 제공한다.
*   BMT

    신규로 Warrant Node로 활동을 희망하는 Work Node를 BMT 할 수 있는 기능을 제공한다.
*   Reputation Management (Yellow list, Black list)

    장애 Node 및 악의 적인 Node를 관리하여 합의 및 Block 생성에 신뢰성과 지속성을 지원한다.
*   Node Management

    Warrant Node중 B그룹은 주기적으로 변경된다. 따라서 각 노드 별로 임기가 있고 이에 대한 관리가 필요하다. 특정 기간에 활동하는 Warrant Node를 ToW(Table of Warrant)로 정의하고 최종 합의된 MainBlock Number를 기준으로 Previous, Active, Next, Candidate로 나뉘어서 관리한다.

    * Previous : 이전에 활동한 Warrant Node
    * Active : 현재 활동 중인 Warrant Node
    * Next : 다음에 활동할 Warrant Node
    * Candidate : BMT 중인 Warrant Node

## Warrant Node 선출 BMT

Warrant Node의 B그룹은 임기를 가지고 활동한다. B그룹은 Work Node중 Warrant Node가 되기를 희망하는 Work Node 중에서 BMT를 거처 선발한다. (Not available in the current version)

![SymSensus4](https://github.com/symverse-lab/Document/wiki/document/image/SymSensus4.png)

Warrant Node 중 B그룹 선출을 위한 BMT는 매일 시행하고 B그룹 Node 수의 1/4+α(장애 노드)개 Node를 선출하며 다음과 같은 활동 주기를 가진다.

1. 인증에 참여하려는 Work Node는 Full Node의 조건을 갖추어야한다.
2. BMT를 통해서 선출된 Node는 4일동안 Warrant Node로 활동한다.
3. 임기가 끝난 Warrant Node는 Work Node로 활동한다.

**후보 노드 평가**

후보 노드가 현재 활동중인 Warrant Node에게 BMT 신청한다. 각 단계별로 평가표 공유 및 합의에 의하여 선출여부를 결정한다. 단계별로 선출 노드 수를 줄여가며 진행하며 현재 단계에서 선출된 노드만 다음 단계 평가의 대상이 된다. Warrant Node로 선출된 Node는 활동 기간 동안 보증금(Deposit)을 인출할 수 없다.

**신규 노드 선출 기준**

1. 평판 평가: 기존에 악의 적인 활동을 한 이력이 있는 노드를 배제한다.
2. 기여도 평가: 보증금(Deposit) 절대 평가
3. 상호운용성 시험(Inter-Operability Test) : Warrant Node로 활동 할 만한 성능을 가지고 있는지 평가한다.
4. 적합성 시험(Conformance test) : 합의에 직접 참여 하여 정상적으로 역할을 수행 하는지 평가 한다.
5. 최종 평가: 랜덤

**제외 노드 선출 기준**

1. 참여한지 오래된 Warrant Node 순으로 정한다. (참여시기가 동일한 노드가 여럿인 경우 랜덤하게 정한다.)
2. 장애 노드(α)

**신규 노드 선출 실패 시 처리**

Warrant Node 선출 BMT에 신청한 Work Node가 부족하거나 BMT를 통과한 Node의 수가 부족할 시 기존 Node들의 임기를 연장하여 운영한다.

## 알려진 블록체인 이슈(공격) 대응 방법

*   Nothing at stake(이기적인 투표 문제)

    포크(블록 체인의 분기)발생 시 지분을 가진 자가 모든 포크(fork)에 투표함으로써 자신은 손해를 보지 않게(nothing at stake)하는 것을 말한다. SymVerse 블록체인에서는 fork가 발생하지 않는다.
*   Bribe Attack(인증 노드 매수 공격)

    블록생성자에게 인센티브를 주고 악의적인 거래가 포함된 거래내역을 생성시키는 것을 말한다. SymVerse는 임기를 가진 보증 노드를 운영하기 때문에 공격자가 2/3+1개의 Warrant Node를 매수하고 매수된 Node가 동일한 시간에 Warrant Node로 활동할 때만 가능하다. 이는 매우 어렵다.
*   Coin Age Accumulation Attack

    코인 보유 기간으로 블록생성자를 선정할 경우 발행한다. SymVerse에서는 발생하지 않는다.
*   Denial of Service (서비스 거부 공격)

    다수의 작은 금액의 거래 발생으로 검증 작업을 지연시키는 공격을 말한다. SymVerse에서는 수수료 정책으로 해결한다.
*   Sybil Attack(불필요한 거래 생성 공격)

    공격자가 가짜 노드를 만들어서 불필요한 거래를 만드는 행위로 SymVerse에서는 SymID를 기반으로 하고 black list, yellow list정책, work node 검증, warrant node 검증, primary node 선출 정책 등 다양한방법으로 방어 가능하다. 악의 적인 거래가 블록 생성자까지 도달하는 것은 거의 불가능하다.
*   이중지불

    SymVerse에서는 특정 시점에 하나의 primary node 만 블록을 생성하므로 이중 지불이 발생하지 않는다.
*   Transaction Malleability (거래 가변성 공격)

    정상적인 거래에 트랜잭션 ID등을 수정하여 허위 거래를 발생시키는 공격방법이다. SymVerse는 모든 거래 내용을 서명 대상 메시지에 포함하므로 원천적으로 불가능하다.
*   51% (66.6%) Attack

    SymVerse는 재단이 전체 보증 노드의 1/3+1개의 보증 노드를 직접 운영하므로 원천적으로 불가능하다.
*   없는 계정으로 송금

    SymVere는 Citizen Block에 모든 이용자가 기록되므로 원천적으로 발생하지 않는다.
