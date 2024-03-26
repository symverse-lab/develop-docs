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

_Total Gas =  Transaction Gas + Create Gas + Data Gas + SCT Gas_

* _Transaction Gas_

> if block >= 4,745,000 { Transaction Gas : 49,000 }\
> else { Transaction Gas : 21,000 }

* _Create Gas_

> if To != nil { Create Gas : 800,000 }&#x20;

* _Data Gas_

> if block >= 4,745,000 { _Data Gas : NonZeroGas(680) \* NonZeroSize + ZeroGas(40) \* ZeroSize_ }
>
> else { _Data Gas : NonZeroGas(68) \* NonZeroSize + ZeroGas(4) \* ZeroSize_ }

* Sct Gas

<table><thead><tr><th width="100">종류</th><th width="365">함수</th><th width="240">설명</th><th width="100" align="right">SCT Gas</th></tr></thead><tbody><tr><td>SCT20</td><td>SCT20_CREATE</td><td>SCT20 계약 생성</td><td align="right">49,000</td></tr><tr><td></td><td>SCT20_TRANSFER</td><td>토큰 송금</td><td align="right">7,000</td></tr><tr><td></td><td>SCT20_TRANSFER_FROM</td><td>토큰 위임 전송</td><td align="right">9,000</td></tr><tr><td></td><td>SCT20_APPROVE</td><td>토큰 위임</td><td align="right">7,000</td></tr><tr><td></td><td>SCT20_DECREASE_APPROVE</td><td>토큰 위임 취소</td><td align="right">7,000</td></tr><tr><td></td><td>SCT20_MINT</td><td>토큰 추가 발행</td><td align="right">7,000</td></tr><tr><td></td><td>SCT20_BURN</td><td>토큰 소각</td><td align="right">7,000</td></tr><tr><td></td><td>SCT20_PAUSE</td><td>계약 일시정지</td><td align="right">4,000</td></tr><tr><td></td><td>SCT20_UNPAUSE</td><td>계약 일시정지 해제</td><td align="right">4,000</td></tr><tr><td></td><td>SCT20_TRANSFER_OWNER</td><td>owner 변경</td><td align="right">4,000</td></tr><tr><td>SCT21</td><td>SCT21_CREATE</td><td>SCT21 계약 생성</td><td align="right">50,000</td></tr><tr><td></td><td>SCT21_TRANSFER</td><td>토큰 송금</td><td align="right">8,000</td></tr><tr><td></td><td>SCT21_TRANSFER_FROM</td><td>토큰 위임 전송</td><td align="right">10,000</td></tr><tr><td></td><td>SCT21_APPROVE</td><td>토큰 위임</td><td align="right">8,000</td></tr><tr><td></td><td>SCT21_DECREASE_APPROVE</td><td>토큰 위임 취소</td><td align="right">8,000</td></tr><tr><td></td><td>SCT21_MINT</td><td>토큰 추가 발행</td><td align="right">9,000</td></tr><tr><td></td><td>SCT21_BURN</td><td>토큰 소각</td><td align="right">9,000</td></tr><tr><td></td><td>SCT21_PAUSE</td><td>계약 일시정지</td><td align="right">4,000</td></tr><tr><td></td><td>SCT21_UNPAUSE</td><td>계약 일시정지 해제</td><td align="right">4,000</td></tr><tr><td></td><td>SCT21_TRANSFER_OWNER</td><td>owner 변경</td><td align="right">4,000</td></tr><tr><td></td><td>SCT21_LOCK_TRANSFER</td><td>잠금 상태인 토큰 전송</td><td align="right">9,000</td></tr><tr><td></td><td>SCT21_UNLOCK_AMOUNT</td><td>토큰 잠금 상태 해제</td><td align="right">9,000</td></tr><tr><td></td><td>SCT21_RESTORE_LOCK_AMOUNT</td><td>잠금 상태인 토큰 회수</td><td align="right">9,000</td></tr><tr><td></td><td>SCT21_ADD_LOCK_AMOUNT</td><td>잠금 상태인 토큰보유량 추가</td><td align="right">6,000</td></tr><tr><td></td><td>SCT21_SUB_LOCK_AMOUNT</td><td>잠금 상태인 토큰유량 회수</td><td align="right">6,000</td></tr><tr><td></td><td>SCT21_ACCOUNT_LOCK</td><td>계정 잠금</td><td align="right">7,000</td></tr><tr><td></td><td>SCT21_ACCOUNT_UNLOCK</td><td>계정 잠금 해제</td><td align="right">7,000</td></tr><tr><td>SCT22</td><td>SCT22_CREATE</td><td>SCT22 계약 생성</td><td align="right">49,000</td></tr><tr><td></td><td>SCT22_TRANSFER</td><td>토큰 송금</td><td align="right">7,000</td></tr><tr><td></td><td>SCT22_MINT</td><td>토큰 추가 발행</td><td align="right">7,000</td></tr><tr><td></td><td>SCT22_BURN</td><td>토큰 소각</td><td align="right">7,000</td></tr><tr><td></td><td>SCT22_PAUSE</td><td>계약 일시정지</td><td align="right">4,000</td></tr><tr><td></td><td>SCT22_UNPAUSE</td><td>계약 일시정지 해제</td><td align="right">4,000</td></tr><tr><td></td><td>SCT22_TRANSFER_OWNER</td><td>owner 변경</td><td align="right">4,000</td></tr><tr><td></td><td>SCT22_SET_AUTHORITY</td><td>권한 설정</td><td align="right">4,000</td></tr><tr><td>SCT30</td><td>SCT30_CREATE</td><td>SCT30 계약 생성</td><td align="right">79,000</td></tr><tr><td></td><td>SCT30_CREATE_ITEM</td><td>SCT30 아이템 생성</td><td align="right">30,000</td></tr><tr><td></td><td>SCT30_TRANSFER</td><td>아이템 전송</td><td align="right">14,000</td></tr><tr><td></td><td>SCT30_TRANSFER_FROM</td><td>토큰 위임 전송</td><td align="right">16,000</td></tr><tr><td></td><td>SCT30_APPROVE</td><td>토큰 위임</td><td align="right">14,000</td></tr><tr><td></td><td>SCT30_ITEM_PAUSE</td><td>아이템 일시정지</td><td align="right">9,000</td></tr><tr><td></td><td>SCT30_ITEM_UNPAUSE</td><td>아이템 일시정지 해제</td><td align="right">9,000</td></tr><tr><td></td><td>SCT30_TRANSFER_OWNER</td><td>owner 변경</td><td align="right">4,000</td></tr><tr><td>SCT40</td><td>SCT40_CREATE</td><td>SCT40 계약 생성</td><td align="right">79,000</td></tr><tr><td></td><td>SCT40_CREATE_COUPON</td><td>SCT40 아이템 생성</td><td align="right">30,000</td></tr><tr><td></td><td>SCT40_TRANSFER</td><td>아이템 전송</td><td align="right">14,000</td></tr><tr><td></td><td>SCT40_TRANSFER_FROM</td><td>토큰 위임 전송</td><td align="right">16,000</td></tr><tr><td></td><td>SCT40_APPROVE</td><td>토큰 위임</td><td align="right">14,000</td></tr><tr><td></td><td>SCT40_COUPON_USE</td><td>아이템 사용처리</td><td align="right">9,000</td></tr><tr><td></td><td>SCT40_COUPON_PAUSE</td><td>아이템 일시정지</td><td align="right">9,000</td></tr><tr><td></td><td>SCT40_COUPON_UNPAUSE</td><td>아이템 일시정지 해제</td><td align="right">9,000</td></tr><tr><td></td><td>SCT40_GRANT_OWNERSHIP</td><td>owner 변경</td><td align="right">4,000</td></tr><tr><td>SCT50</td><td>SCT50_CREATE</td><td>SCT50 설문조사 계약 생성</td><td align="right">49,000</td></tr><tr><td></td><td>SCT50_ADD_POLL_CREATORS</td><td>설문조사 생성 권한 추가</td><td align="right">4,000</td></tr><tr><td></td><td>SCT50_REMOVE_POLL_CREATORS</td><td>설문조사 생성 권한 삭제</td><td align="right">4,000</td></tr><tr><td>SCT51</td><td>SCT51_CREATE_POLL</td><td>SCT51 설문조사 계약 생성</td><td align="right">49,000</td></tr><tr><td></td><td>SCT51_VOTE_IN_POLL</td><td>사용자 설문조사 참여</td><td align="right">4,000</td></tr><tr><td></td><td>SCT51_UNSTAKE_TOKENS</td><td>토큰 예치 해제</td><td align="right">4,000</td></tr><tr><td></td><td>SCT51_EMERGENCY_STOP_POLL</td><td>설문조사 비활성화</td><td align="right">4,000</td></tr><tr><td></td><td>SCT51_FINISH_POLL</td><td>설문조사 만료 처리</td><td align="right">4,000</td></tr><tr><td></td><td>SCT51_WRITE_POLL_RESULTS</td><td>설문조사 결과 쓰기</td><td align="right">4,000</td></tr></tbody></table>
