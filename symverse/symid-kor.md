# SymID KOR

## **Introduction**

SymID는 SymVerse Blockchain의 핵심 기술중의 하나로 인증, 검증, 거래, 기록, 자격증명 등의 기반이 되는 자기주권형 디지털 신분증 기반 기술이다.

## **Summary**

SymID는 개인정보를 디지털신분증으로 발행하여 블록체인에 기록함으로써 디지털신분증의 발급자, 사용자간에 암호학적 증명을 기반으로 하여 충분한 보안을 제공하고 기존 ID관리 시스템 대비 높은 신뢰성과 투명성을 확보하고 단일 지점 장애 문제를 해결하며 사용자의 개인정보를 공개하지 않고 신원확인 여부를 검증할 수 있다. 또한 기존 블록체인처럼 public key를 계정으로 하여 저장하지 않기 때문에 저장용량을 대폭 절약할 수 있다.

익명성과 실명제(KYC, AML)를 서비스 제공자가 선택할 수 있게 함으로써 permissioned Block-Chain체인의 특징과 permissionless Block-Chain의 특징을 동시에 수용하고 있다.

개인키를 분실한 SymID의 소유권과 해당 SymID로 기록된 거래내역의 소유권을 증명할 수 있도록 지원한다. (현재 버전 미적용)

기존 ID를 기반으로 하는 서비스 시스템에 Block-Chain의 적용을 쉽게 할 수 있고 향후 Fractal Network 로 발전할 수 있도록 설계되었다.

## **SymID의 특징**

SymID는 DID, SSI, 영지식증명 등 다양 한 주제를 다루 있으며 다음과 같은 특징이 있다.

* 존재증명(Existence): 사용자의 신원을 증명한다.
* 탈중앙화(Decentralization): DID의 특징인 식별자 관리의 탈중앙화를 통해 단일 지점 장애 문제를 해결한다.
* 자기주권성(Self-Sovereignty): 개인정보의 통제권을 사용자가 가질 수 있게 한다.
* 개인정보(Privacy): 개인정보 노출 없는 서비스를 제공하고 노출 시에는 선택적 노출 기능을 제공한다.
* 보안(Security): 소유자, 발행자, 검증자에게 암호학적 증명을 기반으로 하여 충분한 보안을 제공한다.
* 상호운용성(Interoperability): 기존 ID 시스템과의 연동 및 Symverse Blockchain 참여자 간에 상호 연동할 수 있는 기반을 제공한다.
* 휴대성(Portablity): SymWallet을 이용하여 SymID를 지원하는 모든 시스템에 이용이 가능하다.
* 간결성(Simplicity): 10 Bytes로 사용자의 다양한 계좌 정보, 필수 인증 정보 및 거래 내역 등의 자료를 관리할 수 있다.
* 확장성 (Extensibility): Symverse Blockchain의 기반 기술 중 하나인 Fractal Network 기술을 이용하여 다른 네트워크 참여자와 통신이 가능하다.
* 보호(Protection): SymID를 사용하는 참여자 간에 충돌이 있을 때 서로의 권리를 보호한다.
* 지속성(Persistence): 사용자가 원하는 한 영구 사용가능 하다.
* 신뢰성 통신(Trusted Network): SymID 사용자 간에 PKI 기반 신뢰성 네트웍 구성을 지원한다.
* 실명제(KYC, AML): 사용자의 신뢰성을 확보하여 부정 거래를 방지한다.
* 익명성(Anonymity): 참여자(서비스 제공자 및 이용자)가 원할 경우 익명성을 보장한다.
* 자격증명(Credential): 사용자의 신원 증명 및 자격 증명을 다루며 서비스 제공자가 이용자의 자격을 판별하기 위한 정보를 제공 (성인인증, 운전면허, 졸업증명 등)
* 자격확인(Stamp): 자격증명에서 개인정보를 제거하고 자격여부만 판별

## **SymID의 구성**

사용자(User)별로 하나의 Citizen ID를 가지며 하나의 Citizen ID는 여려 개의 Account를 가진다.

즉, `User : Citizen ID : SymID = 1 : 1 : N 이다.`

SymID는 사용자의 Account정보를 포함하고 있으므로 거래 시 계좌번호로 이용될 수 있다. 일반적으로 사용자는 세부정보는 신경 쓰지 않아도 되며 SymID만 관리하면 된다.

SymID는 Account와 함께 발행되며 SymID 와 Account는 다음 표와 같은 항목으로 구성된다.

### SymID (DID)

| Field      | sub-field |    Size | Explanation     |
| ---------- | --------- | ------: | --------------- |
| Version    |           |  2 bits | Version number  |
| Citizen ID | CA ID     | 14 bits | 발급자 구분 코드       |
|            | Random    | 6 Bytes | 사용자 구분 코드       |
| SeqNum     |           | 2 Bytes | Account 발급 일련번호 |

### Account (Document)

| Field             |     Size | Explanation               |
| ----------------- | -------: | ------------------------- |
| PubKeyHash        | 20 Bytes | Public-Key hash           |
| Role              |  2 Bytes | Role and purpose of SymID |
| Verification Flag |  2 Bytes | 사용자 인증 강도(항목)             |
| State             |   1 Byte | SymID Status              |
| Credit            |   1 Byte | Block-Chain 외부 신용도 정보     |
| Country           |   1 Byte | 국가코드                      |
| Ref. code         |  4 Bytes | 발급자 참조 코드                 |
| Issued            |  7 Bytes | 발급 일시                     |

* Citizen ID: 기존 ID처럼 사용자의 구분자로 이용될 수 있다
  * 8 Bytes
  * Citizen ID는 'CA ID' 와 'Random' 로 구성된다.
* CA ID : 발급자(CA) 별로 할당되는 코드이며 Master CA에 의해서 할당된다.
  * 0x0001 : Master CA (SymVerse 재단)
  * 0x0002 \~ 0x0400 : 신뢰형 발급자 (Trusted CA)
  * 0x0401 \~ 0x0800 : 공개형 발급자 (Public CA)
  * 0x0801 \~ 0x1400 : 익명형 발급자 (Anonymous CA)
  * 0x1401 \~ 0xFFFF : Reserved
* Random : CA에서 각 사용자별로 할당하는 Random Number 이다.
  * 0x00...01 : CA
  * 0x00...02 이상 : 일반 사용자
* SeqNum : Account의 발급 일련 번호이다.
  * 1 : Reserved
  * 2 \~ 9999 : Account
  * 10000 이상 : Reserved
* PubKeyHash : 사용자의 Public-Key hash값의 lower 20bytes. 상호인증이나 송금 시 서명 검증용으로 사용되는 정보이다.
* Role : SymID의 역할을 뜻한다. SymID의 특징, 산업적 기능, 조세부담 등에 사용하기 위한 코드이다.
  * 0x0001 : General (default)
  * … : reserved
  * 0xF0F0 : Master CA
  * 0xF0F1 : CA
* Verification Flag : SymID가 어느 정도의 강도로 신원에 대한 검증을 받고 등록되었는지에 대한 정보이다. Bit 각각의 자리마다 의미를 가지고 있다.
  * lsb+5 \~ msb : reserved
  * lsb+4 : 보증금예치 (deposit)
  * lsb+3 : 대면 확인 여부
  * lsb+2 : 국가 신분증 확인 여부
  * lsb+1 : 전화번호 확인 여부
  * lsb+0 : 이메일 확인 여부
* State : SymID의 상태에 대한 정보이다. 상태는 Active(default), Revoked, Locked, Holding, Marked 등이 있다. 각 상태에 대한 의미는 다음과 같다.
  * 0x01 : Active (default)
  * 0x02 : Revoked, 사용자에 의해 폐기 된다.
  * 0x03 : Locked, 사용자에 의해 거래 정지 된다.
  * 0x04 : Holding, (현재 사용하지 않음.)
  * 0x05 : Marked, 재단에 의해서 거래주의 계정으로 설정된다.

![symid1](https://github.com/symverse-lab/Document/wiki/document/image/symid1.png)

* Credit : SymID의 Block-Chain 외부 신용 등급에 대한 평가지표이다. (Oracle 기술 적용)
* Ref. code : CA가 SymID발급시 그 SymID에 대한 부가정보를 입력 할 수 있도록 할당된 공간이다.
* Issued : SymID의 생성일시이며 YY YY MM DD HH MM SS 순으로 숫자로 표기 한다.
* SymID 구성 예

| SymID Example                   |                          |
| ------------------------------- | ------------------------ |
| Master CA’s 1st Account         | 0x0001 000000000001 0002 |
| Master CA’s 2nd Account         | 0x0001 000000000001 0003 |
| Oracle                          | 0x0001 000000000002 0002 |
| Reward                          | 0x0001 000000000003 0002 |
| 1st CA’s 1st Account            | 0x0002 000000000001 0002 |
| 1st CA’s 2nd Account            | 0x0002 000000000001 0003 |
| 1st CA’s 1st User’s 1st Account | 0x0002 XXXXXXXXXXXX 0002 |
| 1st CA’s 1st User’s 2nd Account | 0x0002 XXXXXXXXXXXX 0003 |
| 1st CA’s 2nd User’s 1st Account | 0x0002 YYYYYYYYYYYY 0002 |

## **시스템 구성**

SymVerse Block-Chain을 이용하는 모든 참여자는 SymID가 있어야 하며 SymID를 활용한 시스템은 다음 그림 과 같이 발급자(CA), 사용자(User), SymID 원장, 개인정보 저장소로 구분되어 운영 된다.

![symid2](https://github.com/symverse-lab/Document/wiki/document/image/symid2.png)

* 사용자 : Block-Chain에 접속하기 위해 발급자에게 SymID발급 받아야 하며 사용자는 역할에 따라 다음과 같이 분류 할 수 있다.
* 서비스 이용자 : SymID를 이용하여 서비스 제공자에게 서비스를 요청하는 사용자
* 서비스 제공자 : 제출된 서비스 이용자의 SymID를 검증하여 서비스 제공 여부를 결정 하고, 검증이 완료되면 서비스를 제공
* 발급자 : 발급자는 ‘최상위 발급자’(Master CA)와 ‘일반 발급자’(CA)로 구분된다. ‘최상위 발급자’는 일반 발급자의 자격여부를 심사하여 일반 발급자의 SymID를 ‘SymID 원장’에 기록하는 역할을 한다. ‘일반 발급자’는 사용자 신원인증 강도와 익명성 보장여부에 따라 신뢰형, 공개형, 익명형 발급자로 나뉘어 진다.
  * 신뢰형 발급자(Trusted Issuer) : 특정 국가의 국민을 대상으로 하거나 성인 여부, 탈세 등의 우려가 있는 서비스를 제공할 때 사용할 수 있는 SymID 발급
  * 공개형 발급자(Public issuer) : 개인을 특정하기 어려운 경우의 서비스를 제공할 때 사용할 수 있는 SymID 발급
  * 익명형 발급자(Anonymous issuer) : 익명성을 보장할 수 있는 서비스를 제공할 때 사용할 수 있는 SymID 발급

![symid3](https://github.com/symverse-lab/Document/wiki/document/image/symid3.png)

위 그림은 다수의 발급자가 참여하여 신원확인 강도 및 익명성 보장 여부에 따라 다양한 종류의 SymID가 발급되는 구성도이다.

* SymID 원장 : 사용자 또는 발급자의 SymID 정보를 기록 및 검증하기 위해 블록체인에 추가한 Citizen-Block을 말한다. Main-Block은 거래 내역을 저장하는 Block이며 모든 거래내역의 사용자 구분은 SymID로 작성된다.

![symid4](https://github.com/symverse-lab/Document/wiki/document/image/symid4.png)

* 개인정보 저장소 : 발급자가 SymID 발급 시 사용된 사용자로부터 제공받은 개인정보를 저장하는 분산네트워크와 분리된 별도의 저장소이다.

## **SymID를 활용한 서비스**

* 신뢰형 서비스

특정 국가의 국민을 대상으로 하거나 성인 여부, 탈세 등의 우려가 있는 서비스의 경우 신뢰성 발급자(200a)이 발급한 SymID을 소지한 서비스 이용 사용자(100a)에게만 제공할 수 있다. SymID로 기록된 Block-Chain의 이용내역의 추적 및 검증을 할 수 있다. 참여자 간 충돌이 있을 시 서로의 권리를 보다 투명하게 보호할 수 있다.

* 공개형 서비스

공개형 발급자(200b)는 이메일과 같이 개인을 특정하기 어려운 정보만 확인하며 SymID로 기록된 Block-Chain의 이용내역의 추적 및 검증을 할 수 있다.

* 익명형 서비스

익명형 발급자(200c)은 사용자에게 어떠한 개인정보도 요구하지 않고 SymID를 발급한다. 익명성을 보장하여 참여자가 보다 자유로운 경재활동 및 의사 표현을 할 수 있다는 효과를 가진다

## **SymID를 활용한 서비스의 이용**

* SymID 발급

![symid5](https://github.com/symverse-lab/Document/wiki/document/image/symid5.png)

위 그림은 발급자, 사용자, Block-Chain, 개인정보 저장소의 역할에 따른 SymID의 발급 절차를 보여주는 흐름도이며 다음 단계를 거처 발급된다.

1. 사용자(100a)가 공개키와 개인키를 생성하는 단계
   * 사용자(100a)의 개인키는 암호화하여 키스토어에 안전하게 보관한다.
2. 사용자(100a)가 Block-Chain에서 획득한 발급자의 SymID를 이용하여 발급자와 상호인증 및 보안 연결하는 단계
3. 사용자(100a)가 발급자에게 공개키 및 개인정보를 제공하고 SymID 발급 요청하는 단계
   * 발급자는 사용자로부터 제공받은 개인정보를 Block-Chain과 분리된 별도의 개인정보 저장소에 안전하게 보관한다
4. 발급자가 Block-Chain에 SymID를 기록하여 Block-Chain상의 다른 사용자와 공유하고 사용자에게 SymID를 발급한다.
   * 발급자는 사용자의 SymID 발급 시 자신의 개인키로 서명하여 발급한다.
5. 사용자(100a)는 Block-Chain에 자신의 SymID가 올바르게 기록되어 있는지 확인할 수 있다.

발급자가 발행한 SymID는 발급자가 참여하고 있는 모든 블록체인 네트워크에서 유일성을 보장받으며 사용할 수 있다

* **SymID 검증**

![symid6](https://github.com/symverse-lab/Document/wiki/document/image/symid6.png)

위 그림은 사용자(서비스 제공자, 100b)가 다른 사용자(서비스 이용자, 100a)의 SymID를 검사하는 절차를 보여주는 흐름도이며 다음 단계를 거처서 검증한다.

1. 서비스를 이용하고자 하는 사용자(100a)가 Block-Chain에 있는 발급자의 SymID와 통신 상대방인 서비스 제공자(100b)의 SymID를 이용하여 상호인증하는 단계
   * 통신을 시도하기 이전에 Block-Chain에 기록된 공개정보를 기반으로 사용자를 검색할 수 있다. (현재 미구현)
   * 사용자는 상호인증 이전에 상대방 사용자의 SymID의 공개된 정보를 보고 상호인증을 거부할 수 있다.
2. 서비스 제공자(100b)가 서비스 이용자(100a)의 SymID의 신원확인 여부에 따라 자신이 보유한 서비스를 제공할지 여부를 결정하는 단계
3. 서비스 제공자(100b)가 서비스 이용자(100a)에게 서비스를 제공하는 단계
