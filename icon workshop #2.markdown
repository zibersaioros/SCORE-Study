# ICON Workshop 2일
---
<!-- TOC -->
- [ICON Workshop 2일](#icon-workshop-2%EC%9D%BC)
    - [Score](#score)
        - [SCORE란?](#score%EB%9E%80)
        - [SCORE 특징](#score-%ED%8A%B9%EC%A7%95)
    - [Score 구현가이드](#score-%EA%B5%AC%ED%98%84%EA%B0%80%EC%9D%B4%EB%93%9C)
        - [iconservice API](#iconservice-api)
        - [클래스](#%ED%81%B4%EB%9E%98%EC%8A%A4)
            - [Address 클래스](#address-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 메서드](#%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C)
            - [ArrayDB 클래스](#arraydb-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 메서드](#%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C-1)
            - [DictDB 클래스](#dictdb-%ED%81%B4%EB%9E%98%EC%8A%A4)
            - [IconScoreBase 클래스](#iconscorebase-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 메서드](#%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C-2)
                - [주요 프로퍼티](#%EC%A3%BC%EC%9A%94-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0)
            - [IconScoreDatabase 클래스](#iconscoredatabase-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 메서드](#%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C-3)
            - [Icx 클래스](#icx-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 메서드](#%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C-4)
            - [Transaction 클래스](#transaction-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 프로퍼티](#%EC%A3%BC%EC%9A%94-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-1)
            - [VarDB 클래스](#vardb-%ED%81%B4%EB%9E%98%EC%8A%A4)
                - [주요 메서드](#%EC%A3%BC%EC%9A%94-%EB%A9%94%EC%84%9C%EB%93%9C-5)
        - [데코레이터](#%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0)
            - [@eventlog](#eventlog)
            - [@external](#external)
            - [@interface](#interface)
            - [@payable](#payable)
        - [전역함수](#%EC%A0%84%EC%97%AD%ED%95%A8%EC%88%98)
        - [Type hints, Exception handling, Limitations](#type-hints-exception-handling-limitations)
            - [Type hints](#type-hints)
            - [Exception handling](#exception-handling)
            - [제한사항](#%EC%A0%9C%ED%95%9C%EC%82%AC%ED%95%AD)

<!-- /TOC -->
---

## Score
### SCORE란?
ICON 네트워크에서 동작하는 스마트 컨트랙트.
<br />

### SCORE 특징
* SCORE는 파이썬으로 작성
* 블록체인에 압축된 바이너리 형태로 업로드 된다.
* 배포된 SCORE는 업데이트가 가능. 업데이트 이후에도 SCORE 주소는 변하지 않음.
* SCORE 사이즈는 압축후 64KB를 초과할 수 없음.
* SCORE는 샌드박스 정책을 준수해야함 : 파일엑세스, 네트워크 API 호출등은 금지됨.

---
## Score 구현가이드

### iconservice API
> https://iconservice.readthedocs.io/en/latest/
---
### 클래스
#### Address 클래스
##### 주요 메서드
 declaration | description  
-------------|-------------
static **from_bytes**(buf:bytes) -> Address | bytes 로부터 Address 객체를 생성하여 반환
static **from_string**(address:str) -> Address   |  42자 주소 문자열로부터 Address 객체 생성하여 반환
**is_contract** -> bool  | 해당 객체가 SCORE인지 여부 반환

#### ArrayDB 클래스
state DB를 배열형태로 사용할 수 있게 랩핑한 유틸리티 클래스.<br />
length, iterator 지원 및 순서 보장
int, str, Address, bytes 자료형만 지원
##### 주요 메서드
declaration | description  
-------------|-------------
**get**(index:int) -> V | 인덱스 위치의 값을 반환
**pop**() → Optional[V] | 마지막으로 추가된 값을 반환하고 지움.
**put**(value: V)  | 값을 배열의 끝에 삽입.

#### DictDB 클래스
statd DB를 Dictionary 형태로 사용할 수 있게 랩핑한 유틸리티 클래스.
Javascript Object 처럼 사용.
int, str, Address, bytes 자료형만 지원.

#### IconScoreBase 클래스
SCORE의 기반 클래스. SCORE가 실행되는 환경과 기능을 제공.<br />
추상메서드 \_\_init\_\_, on\_install, on\_update 은 구현시 반드시 부모(super())의 메서드를 호출해줘야 함.

##### 주요 메서드
 declaration | description  
-------------|-------------
**\_\_init\_\_**(db: IconScoreDatabase) -> None | python 자체 초기화 함수(생성자). 컨트랙트가 각 노드에 로드 될때 호출됨. 이 메서드에서 상태변경 작업을 진행하면 안 됨. 변수 선언등의 작업을 진행.
**call**(addr_to: Address, func_name: str, kw_dict: dict, amount: int) | 다른 SCORE의 external 함수를 호출함. func_name이 None이면 fallback을 호출.
**create_interface_score**(addr_to: Address, interface_cls: Callable[[Address, callable], T]) → T  | 인터페이스 T의 인스턴스를 만들어 반환. 지정된 SCORE의 external 함수에 접근할 수 있다.
**fallback**() → None  | 컨트랙트가 순수 ICX코인만 받을 때 사용되는 함수. 직접 호출되어선 안되기 때문에 @external 데코레이션을 붙일 수 없다.
**on_install**(\*\*kwargs) → None  | 컨트랙트가 최초 배포될 때 호출되고 다시는 호출되지 않는다. state DB를 초기화하는 곳이다.
**on_update**(\*\*kwargs) → None | 컨트랙트가 업데이트 배포될 때 호출된다. state DB 마이그레이션하는 곳이다.

##### 주요 프로퍼티
 name | description  
 -----|-------
address | `Address`:  현재 SCORE의 주소
block_height | 현재 블록번호
db  |  `IconScoreDatabase`: state DB에 엑세스하는 인스턴스
icx  |  `Icx`:  SCORE의 icx객체
msg  | 현재 SCORE 호출에 대한 정보를 담고있다.
owner  | `Address`: SCORE를 배포한 사람의 주소
tx  |  `Transaction`: 트랜잭션 정보를 담고있다.

#### IconScoreDatabase 클래스
Icon SCORE에서 사용하는 DB객체. IconScoreDatabase를 통해 상태정보에 접근하고 변경한다.
##### 주요 메서드
declaration | description  
-------------|-------------
**delete**(key: bytes) |  특정 key/value 쌍을 삭제
**get**(key: bytes) → bytes |  특정 key/value 쌍을 반환
**put**(key: bytes, value: bytes) |  특정 key/value 쌍을 저장

#### Icx 클래스
ICX 코인 전송관리 클래스.
##### 주요 메서드
declaration | description  
-------------|-------------
**get_balance**(address: Address) → int  |  address의 잔액 조회
**send**(addr_to: Address, amount: int) → bool  |  addr_to에 amount만큼 icx 전송
**transfer**(addr_to: Address, amount: int) → None  |  addr_to에 amount만큼 icx 전송. 실패시 예외발생

#### Transaction 클래스
트랜잭션정보를 담고있는 클래스
##### 주요 프로퍼티
name | description  
-----|------------
hash | 트랜잭션 해시
index | 블록내 트랜잭션 인덱스
nonce | 트랜잭션 요청시의 nonce 값
origin | 트랜잭션을 생성한 계정
timestamp | 트랜잭션 요청 시각 (TimeMillis)

#### VarDB 클래스
state DB를 변수처럼 사용할 수 있게 랩핑한 유틸리티 클래스.<br />
int, str, Address, bytes 자료형만 지원
##### 주요 메서드
declaration | description  
-------------|-------------
**get**() -> Optional[V] | 값을 반환
**remove**() -> None | 값을 삭제
**set**(value: V) -> None  | 값을 저장.
---
### 데코레이터
#### @eventlog
@eventlog를 포함한 함수는 TxResult에 eventLogs라는 이름으로 로그를 남긴다.

#### @external
함수를 외부로 노출시킬 때 사용한다. @external이 있는 함수는 다른 SCORE나 계정이 호출 가능.
readonly 파라미터를 이용해 read-only 설정이 가능

#### @interface
SCORE 인터페이스의 메서드를 정의할 때 사용
@interface를 함수에 정의하면 외부SCORE의 함수를 일반 함수호출과 같은형태로 사용이 가능하다.

#### @payable
외부로 노출된 함수가 ICX를 받을 수 있도록 하는 데코레이터. msg.value로 확인 가능.

---

### 전역함수
declaration | description  
-------------|-------------
**create_address_with_key**(public_key: bytes) → Optional[Address]  |  public key로 주소를 생성. (수수료 지불)
**json_dumps**(obj: Any, \*\*kwargs) → str | 파이썬 object를 JSON 스트링으로 변환
**json_loads**(src: str, \*\*kwargs) → Any | JSON스트링을 파이썬 object로 변환
**recover_key**(msg_hash: bytes, signature: bytes, compressed: bool) → Optional[bytes] | msg_hash와 signature로부터 public key를 반환
**revert**(message: str, code: Union[ExceptionCode, int]) → None  | 현재 트랜잭션을 취소하고 state DB의 모든 변경을 롤백
**sha3_256**(data: bytes) → bytes | 입력 데이터로 해시를 계산.

---

### Type hints, Exception handling, Limitations
#### Type hints
파라미터, 리턴이 어떤 타입일지 힌트를 주는 것. SCORE에서는 Type hints를 반드시 지정해야 한다. SCORE의 API 스펙이 type hints를 기반으로 생성되기 때문.<br />
파라미터엔 int, str, bytes, bool, Address 가 올 수 있다.
리턴엔 int, str, bytes, bool, Address, List, Dict가 올 수 있다.

#### Exception handling
Exception 처리시 IconServiceBaseException을 상속받아 처리하기보단  revert()를 사용하길 권장

#### 제한사항
* 트랜잭션당 한번에 최대 도합 1024번의 call, interface call, Icx 전송만 가능
* 트랜잭션당 외부 SCORE 호출로인해 증가할 수 있는 스택사이즈는 최대 64개이다.
* states에 의해 관리되지 않는 멤버변수는 선언할 수 없다.
