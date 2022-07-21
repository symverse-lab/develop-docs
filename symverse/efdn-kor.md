# EFDN KOR

## EFDN (Evolutionary Functional Distributive Network)

EFDN은 동일기능에서 다양한 기능으로 진화하는 미래지향적 분산네트워크이다.

블록체인의 소비자와 공급자가 협력하며 다음과 같은 분산 P2P 네트워크를 구축한다.

* 네트워크 Node의 지속적인 활성화 유지
* 악의적인 Node 및 장애 Node의 탐지 및 관리
* 블록체인 Node의 지속적인 기능 세분화를 통한 거래 속도와 확장성 확보
* 합의과정, load-balancing, 블록생성을 위한 거래모음 처리 등의 네트워크 기능 제공
* 파일공유 및 메시징 처리 등 미래지향형 서비스 도입

## EFDN의 구성

SymVerse 블록체인은 Wallet Node, Work Node, Warrant Node와 CA(Citizen Alliance) Node로 구성된다. 각 Node들은 다음 그림과 같이 상호 연동하여 각각의 역할을 수행함으로써 블록체인 생태계를 유지한다

![efdn1](https://github.com/symverse-lab/Document/wiki/document/image/efdn1.png)

* **CA (Citizen Alliance) Node**는 Client/Server Model로 운영되며 블록체인 참여자의 SymID 및 Account를 관리한다. CA의 운영은 해당국가 정부의 법적 테두리 안에서 운영되며 SymVerse 재단에 의해 선정된다.
* **Wallet Node**는 블록체인을 이용하여 거래하기를 희망하는 참여자 누구든 휴대폰이나 PC에 설치하여 사용할 수 있다. CA와 통신하여 SymID를 발급하는 역할 및 거래 Transaction을 생성하고 Work Node로 전달하는 역할을 주로 한다.
* **Work(light) Node**는 가정이나 단체 등에서 소규모 그룹을 형성하여 블록체인에 참가를 희망하는 경우에 사용된다. 개인의 Wallet Node를 그룹에서 운영하는 Work(light) Node에 등록하여 사용함으로써 Work Node의 기여도 보상을 받을 수 있다.
* **Work(full) Node**는 블록체인을 모니터링 하고자 하는 경우 또는 Warrant(보증) Node에 참여하고자 하는 경우에 사용된다. 예를 들어 많은 회원을 확보하고 있는 쇼핑몰의 경우 쇼핑몰 회원들에게 SymVerse Wallet을 나누어 주고 쇼핑몰에서 운영하는 Work Node에 등록하여 사용하게 함으로써 많은 기여도 보상을 받을 수 있다.
* **Work(scan) Node**는 즉시 거래 내역, 예약 거래 내역, 스마트 계약 내용, 보증 Node 선출 내역, 기여도 보상 내역 등 블록체인상의 모든 데이터를 기록하고 조회할 수 있는 서비스를 제공한다.
* **Warrant Node**는 총 25개 Node로 운영되며 합의를 통한 블록의 생성이 주된 역할이다. 네트웍 서비스 경험이 있는 기업이나 개인은 누구든 Warrant Node에 참여하며, Warrant Node의 기여도 보상을 받을 수 있다.

## PoN(Proof of Network)

Proof of Network(PoN)는 EFDN의 작동원리이다. PoN은

1. 소비자인 Wallet Node들이 Work Node를 통하여 거래를 처리하며 Network를 활성화시키는 작업
2. Work Node들이 Warrant Node중 블록제안 그룹(B)에 참여하기 위한 조건의 심사 (Automatic BMT)
3. Warrant Node중 거부권 그룹(A)에 포함되는 Node 선발과정
4. 블록 제안 그룹(B)과 거부권 그룹(A)이 합의하여 신규블록을 생성하는 과정 (SymSensus)
5. 합의과정이 완료된 후 1일 1회 공급되는 코인분배 (SymReward)

등 세부적인 EFDN의 작동원리를 포함한다.

*   거래 처리(Transaction)

    SymID를 가진 Wallet Node는 Blockchain과 연결된 Work Node를 통하여 거래를 처리한다. Work Node 목록은 수시로 업데이트 되며 Wallet Node는 3개의 Work Node와 접속하여 Transaction처리를 한다. 만약 특정 Work Node로 접속이 이루어지지 않으면 다른 Work Node를 선택하게 된다.

    Work Node는 P2P Protocol을 이용하여 다른 Work Node와 Warrant Node에 Transaction을 전달한다.

    Wallet Node에서 발생한 Transaction은 Warrant Node간의 합의과정의 결과로 새로운 Block에 기록되며 새로운 Block은 기존 Block과 연결되어 영구 보존된다.
*   합의과정 기여를 통한 네트워크 활성화

    Warrant Node는 모든 계정들의 SYM 보유량, 거래수수료, SYM 사용량, 합의과정 참여회수, 합의 참여의사 등의 항목을 구분하여 관리한다.

    신규 블록 생성 합의에 참여하기를 원하는 Work Node들은 Warrant Node에게 참가의사를 전하고 Automatic BMT를 거처서 선발되면 합의에 참여할 수 있다.

    모든 노드는 신규 코인 생성 합의과정에 참여하게 되며 Transaction의 생성, 전달, 블록 생성 등 블록체인 상의 모든 활동이 신규 코인 생성 합의 과정의 요소로 작동하게 된다. Warrant Node는 신규 코인 생성 합의과정에 기여하는 노드들에게 Reward로 신규 코인을 지급한다.
