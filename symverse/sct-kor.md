# SCT KOR

## **SCT(Symverse Contract Template)**

**특징**

* 스마트 컨트랙트 중 현재 가장 많이 사용되고 있는 항목을 내장화 하여 간편하게 생성 및 이용 가능하다.
* 개발 환경의 개선
  * 솔리디티등의 Smart Contract 전용 개발 언어를 사용하지 않는다.
  * 단순 API 호출로 개발 가능하게 하여 개발 시간 단축시킬 수 있다.

![sct\_usage](https://github.com/symverse-lab/Document/wiki/document/image/sct\_usage.png)

* 유지 보수 비용 감소
  * 사용자는 보안 문제 등의 이슈 사항에 대해서 신경 쓰지 않아도 된다.

**SCT 종류**

* SCT20, SCT21 (ERC20 like)
  * 거래 가능한 토큰, 기능성 토큰, 디지털 화폐, ICO, IEO
* SCT30 (ETC721 like)
  * 디지털 또는 물리적 자산에 대한 소유권을 표현
  * 물리적 속성(주택, 독특한 삽화 등)
  * 가상 수집품(새끼 고양이, 게임 아이템 등)
  * 자산(대출, 부담 및 기타 책임 등)
* SCT40 (쿠폰형태의 계약을 처리)
  * 쿠폰 계약 생성 및 관리
  * 개별 쿠폰 생성 및 관리
  * 개발 쿠폰 발급 제약 조건 제어
  * 상품권, 할인 쿠폰, 숙박권 등

**SCT 용어 정리**

* Token : SCT20으로 생성된 코인
* Creator : SCT 계약의 생성자 계정(Master Owner)
* Owner : SCT 계약의 소유자 계정
* Spender(delegator) : 토큰 소유자로부터 일부 토큰의 송금 권한을 위임 받은 계정
* User(Holder) : 모든 계정은 User의 권한을 가진다.

## **권한 위임**

*   Creator

    Creator는 한개의 계정에 Owner를 지정 및 해제할 수 있다.\


    * 지정 : SCT\*\_APPROVE\

    * 해제 : SCT\*\_DECREASE\_APPROVE\
      최초 계약 생성시 Creator는 자신을 Owner로 지정하거나 다른 계정을 Owner로 지정해야 한다.
*   Owner

    Owner는 Creator의 일부 권한을 가진다.
*   Spender (Delegator)

    토큰을 소유하고 있는 모든 계정(User, Owner, Creator)은 다른 계정을 Spender로 지정하여 자신이 보유한 토큰 중 일부에 대해서 송금 권한을 위임할 수 있다.

    Spender는 자신을 Spender로 지정한 계정의 코인을 위임받은 만큼 다른 User에게 송금할 수 있다.

    예를 들어\
    Owner(A), User(B, C) 가 있는 상황에서

    * A가 100개의 토큰을 소유하고 있을 때
    * A는 B를 Spender로 지정하고 50개의 송금 권한을 위임할 수 있다.
    * B는 A의 에게 위임받은 50개중 10개를 C에게 송금할 수 있다. (SCT\*\_TRANSFER\_FROM)

    ※ B는 C를 Spender 로 지정할 수 없다. (위임받은 권한을 다른 계정에 다시 위임할 수 없다.)\
    ※ B는 송금 시 자신의 개인키를 이용해 서명하여 송금 트랜잭션을 발행하지만 토큰은 A에서 C로 송금된다.

    **Spender 지정은 다음 두가지 이유로 사용한다.**

    `첫번째`, 토큰 소유자가 토큰을 혼자 관리하기 힘들 경우 다른 사람에게 권한을 위임하여 관리하고자 할 때 사용한다.\
    `두번째`, 토큰 소유계정의 개인키 분실에 대하여 대응하기 위해 사용한다. Owner가 Spender를 지정하고 Application Server에서는 Spender 계정을 사용하여 운영할 경우 Application Server가 해킹 등의 이유로 개인키를 분실 혹은 유출되었을 시 Owner은 새로운 Spender를 지정하여 대응할 수 있다.

## **SCT 정밀도**

SCT 계약을 생성시 정밀도를 위하여 10^18 hug 단위로생성하기를 권장한다. 이는 SymScan등의 서비스에서 10^18 hug 를 정밀도로 하여 표시 하기 때문이다.

See also [Prepare SCT-Data](https://github.com/symverse-lab/Document/wiki/SCT-Guide#b-prepare-sct-data)

## **SCT API**

[JSON RPC SCT API](https://github.com/symverse-lab/Document/wiki/JSON-RPC-SCT-API)

## **SCT 기능 및 Gas**

**SCT 계약을 생성하고 사용하기 위해서는 SYM Coin이 필요하다. Gas Fee는 해당 SCT 함수 호출시 필요한 SYM Coin의 수이다.**

_Total Gas = SCT Gas + Transaction Gas_

| 종류    | 함수                       | 설명           | SCT Gas | Total Gas |
| ----- | ------------------------ | ------------ | ------: | --------: |
| SCT20 | SCT20\_CREATE            | SCT20 계약 생성  |  49,000 |    70,000 |
|       | SCT20\_TRANSFER          | 토큰 송금        |   7,000 |    28,000 |
|       | SCT20\_MINT              | 토큰 추가 발행     |   7,000 |    28,000 |
|       | SCT20\_BURN              | 토큰 소각        |   7,000 |    28,000 |
|       | SCT20\_PAUSE             | 계약 일시정지      |   4,000 |    23,000 |
|       | SCT20\_UNPAUSE           | 계약 일시정지 해제   |   4,000 |    23,000 |
|       | SCT20\_GRANT\_OWNERSHIP  | owner 변경     |   4,000 |    25,000 |
| SCT21 | SCT21\_CREATE            | SCT21 계약 생성  |  50,000 |    71,000 |
|       | SCT21\_TRANSFER          | 토큰 송금        |   8,000 |    29,000 |
|       | SCT21\_MINT              | 토큰 추가 발행     |   9,000 |    30,000 |
|       | SCT21\_BURN              | 토큰 소각        |   9,000 |    30,000 |
|       | SCT21\_PAUSE             | 계약 일시정지      |   4,000 |    25,000 |
|       | SCT21\_UNPAUSE           | 계약 일시정지 해제   |   4,000 |    25,000 |
|       | SCT21\_GRANT\_OWNERSHIP  | owner 변경     |   4,000 |    25,000 |
|       | SCT21\_TRANSFER\_LOCKED  | 잠금 상태 토큰 송금  |   9,000 |    30,000 |
|       | SCT21\_GRANT\_UNLOCK     | 토큰 잠금 상태 해제  |   9,000 |    30,000 |
|       | SCT21\_RECALL\_LOCKED    | 전송한 잠금 토큰 회수 |   9,000 |    30,000 |
|       | SCT21\_INCREASE\_LOCKED  | 잠금 보유량 추가    |   6,000 |    27,000 |
|       | SCT21\_DECREASE\_LOCKED  | 잠금 보유량 회수    |   6,000 |    27,000 |
|       | SCT21\_ACCOUNT\_INACTIVE | 계정 잠금        |   7,000 |    28,000 |
|       | SCT21\_ACCOUNT\_ACTIVE   | 계정 잠금 해제     |   7,000 |    28,000 |
| SCT30 | SCT30\_CREATE            | SCT30 계약 생성  |  79,000 |   100,000 |
|       | SCT30\_CREATE\_ITEM      | SCT30 아이템 생성 |  30,000 |    51,000 |
|       | SCT30\_TRANSFER          | 아이템 전송       |  14,000 |    35,000 |
|       | SCT30\_ITEM\_PAUSE       | 아이템 일시정지     |   9,000 |    30,000 |
|       | SCT30\_ITEM\_UNPAUSE     | 아이템 일시정지 해제  |   9,000 |    30,000 |
|       | SCT30\_GRANT\_OWNERSHIP  | owner 변경     |   4,000 |    25,000 |
| SCT40 | SCT40\_CREATE            | SCT40 계약 생성  |  79,000 |   100,000 |
|       | SCT40\_CREATE\_COUPON    | SCT40 아이템 생성 |  30,000 |    51,000 |
|       | SCT40\_TRANSFER          | 아이템 전송       |  14,000 |    35,000 |
|       | SCT40\_COUPON\_USE       | 아이템 사용처리     |   9,000 |    30,000 |
|       | SCT40\_COUPON\_PAUSE     | 아이템 일시정지     |   9,000 |    30,000 |
|       | SCT40\_COUPON\_UNPAUSE   | 아이템 일시정지 해제  |   9,000 |    30,000 |
|       | SCT40\_GRANT\_OWNERSHIP  | owner 변경     |   4,000 |    25,000 |
