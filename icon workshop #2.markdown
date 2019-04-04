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
    - [Score Sample](#score-sample)
        - [Black Jack](#black-jack)
            - [Business flow](#business-flow)
                - [chip 발행용 SCORE 배포](#chip-%EB%B0%9C%ED%96%89%EC%9A%A9-score-%EB%B0%B0%ED%8F%AC)
                - [BlackJack 게임용 SCORE 배포](#blackjack-%EA%B2%8C%EC%9E%84%EC%9A%A9-score-%EB%B0%B0%ED%8F%AC)
                - [공통파일 작성](#%EA%B3%B5%ED%86%B5%ED%8C%8C%EC%9D%BC-%EC%9E%91%EC%84%B1)
                - [ICX로 chip 환전](#icx%EB%A1%9C-chip-%ED%99%98%EC%A0%84)
                - [chip 잔액 확인](#chip-%EC%9E%94%EC%95%A1-%ED%99%95%EC%9D%B8)
                - [방 생성](#%EB%B0%A9-%EC%83%9D%EC%84%B1)
                - [방 목록 확인](#%EB%B0%A9-%EB%AA%A9%EB%A1%9D-%ED%99%95%EC%9D%B8)
                - [방 입장](#%EB%B0%A9-%EC%9E%85%EC%9E%A5)
                - [레디 상태 변환](#%EB%A0%88%EB%94%94-%EC%83%81%ED%83%9C-%EB%B3%80%ED%99%98)
                - [게임 시작](#%EA%B2%8C%EC%9E%84-%EC%8B%9C%EC%9E%91)
                - [패 돌리기](#%ED%8C%A8-%EB%8F%8C%EB%A6%AC%EA%B8%B0)
                - [손패 보기](#%EC%86%90%ED%8C%A8-%EB%B3%B4%EA%B8%B0)
                - [손패 픽스](#%EC%86%90%ED%8C%A8-%ED%94%BD%EC%8A%A4)
                - [게임 결과 확인](#%EA%B2%8C%EC%9E%84-%EA%B2%B0%EA%B3%BC-%ED%99%95%EC%9D%B8)
                - [방 나가기](#%EB%B0%A9-%EB%82%98%EA%B0%80%EA%B8%B0)
                - [ICX로 환전](#icx%EB%A1%9C-%ED%99%98%EC%A0%84)

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

---

## Score Sample
### Black Jack
#### Business flow
1. chip 발행용 SCORE 배포
2. BlackJack 게임용 SCORE 배포
3. ICX로 chip 환전
4. chip 잔액 확인
5. 방 생성
6. 방 목록 확인
7. 방 입장
8. 레디 상태 변환
9. 게임 시작
10. 패 돌리기
11. 손패 보기
12. 손패 픽스
13. 게임 결과 확인
14. 방 나가기
15. ICX로 환전

##### chip 발행용 SCORE 배포
기본적인 토큰 기능과 환전 기능을 갖고 있는 Chip SCORE를 배포한다.
```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider

import icon_util as util

icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))
wallet = util.get_wallet("./keystore_rs.json", "./password.txt")

signed_transaction = util.buildDeployTransaction(wallet, "./chip")
tx_hash = icon_service.send_transaction(signed_transaction)
print(tx_hash)
```

##### BlackJack 게임용 SCORE 배포
블랙잭 게임용 SCORE를 배포한다. 위에서 배포한 chip SCORE 주소를 파라미터로 넘긴다.
```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider

import icon_util as util

icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))
wallet = util.get_wallet("./keystore_rs.json", "./password.txt")

signed_transaction = util.buildDeployTransaction(wallet, "./blackjack", _tokenAddress="cx6c0473858b08a1905e6829d45366cb992f55a35d")
tx_hash = icon_service.send_transaction(signed_transaction)
print(tx_hash)
```

##### 공통파일 작성
play_blackjack.prerequisite.py
```python
import icon_util as util

from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider

icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))
wallets = [util.get_wallet("../keystore_rs.json", "../password.txt"),
           util.get_wallet("../keystore_rslinux.json", "../password.txt")]
score_address = "cx5dfa12d69495b25dccb5ddca80603d6c0f451723"
```

##### ICX로 chip 환전
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    signed_transaction = util.buildCallTransaction(wallet, score_address, "mintChips", 10000000000000000000)
    result = icon_service.send_transaction(signed_transaction)
    print(result)
```

```python
    @external
    @payable
    def mintChips(self):
        chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)
        chip.mint(self.msg.value)
```

##### chip 잔액 확인
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    call = util.buildCall(wallet, score_address, "getChipBalance")
    result = icon_service.call(call)
    print(int(result, 16))
```

```python
    @external(readonly=True)
    def getChipBalance(self) -> int:
        chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)
        return chip.balanceOf(self.msg.sender)
```

##### 방 생성
```python
from play_blackjack.prerequisite import *

signed_transaction = util.buildCallTransaction(wallets[0], score_address, "createRoom", _prizePerGame=1)
result = icon_service.send_transaction(signed_transaction)
print(result)
```

```python
    @external
    def createRoom(self, _prizePerGame: int=10):
        # Check whether 'self.msg.sender' is now participating to game room or not
        if self._DDB_in_game_room[self.msg.sender] is not None:
            revert("You already joined to another room")

        # Check whether the chip balance of 'self.msg.sender' exceeds the prize_per_game or not
        chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)
        if chip.balanceOf(self.msg.sender) < _prizePerGame:
            revert("Set the prize not to exceed your balance")

        # Create the game room & Get in to it & Set the prize_per_game value
        # 방을 만들고 입장. 방정보를 갱신.
        game_room = GameRoom(self.msg.sender, self.msg.sender, self.block.height, _prizePerGame)
        game_room.join(self.msg.sender)
        self._DDB_game_room[self.msg.sender] = str(game_room)

        #방 목록에 방 추가.
        game_room_list = self._get_game_room_list()
        game_room_list.put(str(game_room))
        self._DDB_in_game_room[self.msg.sender] = self.msg.sender

        # Initialize the deck of participant
        # 방 참여자의 덱과 핸드를 초기화
        new_deck = Deck()
        self._DDB_deck[self.msg.sender] = str(new_deck)
        new_hand = Hand()
        self._DDB_hand[self.msg.sender] = str(new_hand)
```

##### 방 목록 확인
```python
from play_blackjack.prerequisite import *

call = util.buildCall(wallets[1], score_address, "showGameRoomList")
result = icon_service.call(call)
print(result)
```

```python
    def _get_game_room_list(self):
    """게임방 리스트 반환 -> ArrayDB"""
    return ArrayDB(self._GAME_ROOM_LIST, self._db, value_type=str)

    @external(readonly=True)
    def showGameRoomList(self) -> list:
        response = []
        game_room_list = self._get_game_room_list()

        for game_room in game_room_list:
            game_room_dict = json_loads(game_room)
            game_room_id = game_room_dict['game_room_id']
            creation_time = game_room_dict['creation_time']
            prize_per_game = game_room_dict['prize_per_game']
            participants = game_room_dict['participants']
            room_has_vacant_seat = "is Full" if len(participants) > 1 else "has a vacant seat"
            response.append(f"{game_room_id} : ({len(participants)} / 2). The room {room_has_vacant_seat}. Prize : {prize_per_game}. Creation time : {creation_time}")

        return response
```
##### 방 입장
```python
from play_blackjack.prerequisite import *

signed_transaction = util.buildCallTransaction(wallets[1], score_address, "joinRoom", _gameRoomId="hxb1c73381e270d401a8f8b3f969f97457cca32d6d")
result = icon_service.send_transaction(signed_transaction)
print(result)
```

```python
    @external
    def joinRoom(self, _gameRoomId: Address):
        # Check whether the game room with game_room_id is existent or not
        if self._DDB_game_room[_gameRoomId] is "":
            revert(f"There is no game room which has equivalent id to {_gameRoomId}")

        # Check the participant is already joined to another game_room
        if self._DDB_in_game_room[self.msg.sender] is not None:
            revert(f"You already joined to another game room : {self._DDB_in_game_room[self.msg.sender]}")

        # 게임룸 정보 load
        game_room_dict = json_loads(self._DDB_game_room[_gameRoomId])
        game_room = GameRoom(Address.from_string(game_room_dict['owner']), Address.from_string(game_room_dict['game_room_id']), game_room_dict['creation_time'],
                             game_room_dict['prize_per_game'], game_room_dict['participants'], game_room_dict['active'])
        game_room_list = self._get_game_room_list()

        # Check the chip balance of 'self.msg.sender' before getting in
        chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)
        if chip.balanceOf(self.msg.sender) < game_room.prize_per_game:
            revert(f"Not enough Chips to join this game room {_gameRoomId}. Require {game_room.prize_per_game} chips")

        # Check the game room's participants. Max : 2
        if len(game_room.participants) > 1:
            revert(f"Full : Can not join to game room {_gameRoomId}")

        # Get in to the game room
        # 방정보 DB에 저장
        game_room.join(self.msg.sender)
        self._DDB_in_game_room[self.msg.sender] = _gameRoomId
        self._DDB_game_room[_gameRoomId] = str(game_room)

        # 게임룸 리스트 갱신.
        game_room_index_gen = (index for index in range(len(game_room_list)) if game_room.game_room_id == Address.from_string(json_loads(game_room_list[index])['game_room_id']))

        try:
            index = next(game_room_index_gen)
            game_room_list[index] = str(game_room)
        except StopIteration:
            pass

        # Initialize the deck & hand of participant
        new_deck = Deck()
        self._DDB_deck[self.msg.sender] = str(new_deck)
        new_hand = Hand()
        self._DDB_hand[self.msg.sender] = str(new_hand)
```
##### 레디 상태 변환
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    signed_transaction = util.buildCallTransaction(wallet, score_address, "toggleReady")
    result = icon_service.send_transaction(signed_transaction)
    print(result)
```

```python
    @external
    def toggleReady(self):
        if self._DDB_in_game_room[self.msg.sender] is None:
            revert("Enter the game room first.")
        if self._DDB_ready[self.msg.sender]:
            self._DDB_ready[self.msg.sender] = False
        else:
            self._DDB_ready[self.msg.sender] = True
```

##### 게임 시작
```python
from play_blackjack.prerequisite import *

signed_transaction = util.buildCallTransaction(wallets[0], score_address, "gameStart")
result = icon_service.send_transaction(signed_transaction)
print(result)
```

```python
    def _bet(self, bet_from: Address, amount: int):
        """
        bet_from의 칩을 베팅함. 베팅한 칩은 blackjack 스코어가 맡게됨.
        """
        chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)
        chip.bet(bet_from, self.address, amount)


    @external
    def gameStart(self):
        #load gameroom info
        game_room_id = self._DDB_in_game_room[self.msg.sender]
        game_room_dict = json_loads(self._DDB_game_room[game_room_id])
        game_room = GameRoom(Address.from_string(game_room_dict['owner']), Address.from_string(game_room_dict['game_room_id']), game_room_dict['creation_time'],
                             game_room_dict['prize_per_game'], game_room_dict['participants'], game_room_dict['active'])
        participants = game_room.participants

        # Check the 'self.msg.sender' == game_room.owner
        if not self.msg.sender == game_room.owner:
            revert("Only owner of game room can start the game")

        if game_room.active:
            revert("The last game is still active and not finalized")

        # Check the number of participants
        if len(participants) < 2:
            revert("Please wait for a challenger to come")

        # Make sure that all the participants are ready
        for participant in participants:
            if not self._DDB_ready[Address.from_string(participant)]:
                revert(f"{participant} is not ready to play game")

        # 베팅
        for participant in participants:
            self._bet(bet_from=Address.from_string(participant), amount=game_room.prize_per_game)

        # Game start
        game_room.game_start()
        self._DDB_game_start_time[game_room_id] = self.block.height
        self._DDB_game_room[game_room_id] = str(game_room)

        # Set ready status of both participants to False after starting the game
        for participant in participants:
            self._DDB_ready[Address.from_string(participant)] = False
```

##### 패 돌리기
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    signed_transaction = util.buildCallTransaction(wallet, score_address, "hit")
    result = icon_service.send_transaction(signed_transaction)
    print(result)
```
```python
    @external
    def hit(self):
        """
        호출자에게 카드를 한장 분배하고 게임 완료 조건 만족시 승패를 계산.
        :return:
        """
        game_room_id = self._DDB_in_game_room[self.msg.sender]
        if game_room_id is None:
            revert("You are not in game")

        game_room_dict = json_loads(self._DDB_game_room[game_room_id])
        game_room = GameRoom(Address.from_string(game_room_dict['owner']), Address.from_string(game_room_dict['game_room_id']), game_room_dict['creation_time'],
                             game_room_dict['prize_per_game'], game_room_dict['participants'], game_room_dict['active'])

        # Check whether the game is in active mode or not
        if not game_room.active:
            revert("The game is now in inactive mode")

        # Hit and adjust the status of deck & hand
        deck_dict = json_loads(self._DDB_deck[self.msg.sender])
        deck = Deck(deck_dict['deck'])
        hand_dict = json_loads(self._DDB_hand[self.msg.sender])
        hand = Hand(hand_dict['cards'], hand_dict['value'], hand_dict['aces'], hand_dict['fix'])

        # Check if the participant has already fixed hands
        if hand.fix:
            revert('You already fixed your hand')

        if len(hand.cards) == 4:
            hand.fix = True

        #카드를 한장 분배하고 DB에 저장.
        hand.add_card(deck.deal(self.block.timestamp, self.msg.sender))
        hand.adjust_for_ace()
        self._DDB_deck[self.msg.sender] = str(deck)
        self._DDB_hand[self.msg.sender] = str(hand)
        self.Hit(self.msg.sender, game_room_id)

        # Check whether the fix status of all participants are True. And, If participant 'hand.value' exceeds 21, finalize the game. & Game must be finalized.
        if self._check_participants_fix(game_room_id) or hand.value > 21 or self.block.height - self._DDB_game_start_time[game_room_id] > 60:
            self.calculate(game_room_id)
```

##### 손패 보기
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    call = util.buildCall(wallet, score_address, "showMine")
    result = icon_service.call(call)
    print(result)
```

```python
    @external(readonly=True)
    def showMine(self) -> str:
        hand = self._DDB_hand[self.msg.sender]
        return hand
```

##### 손패 픽스
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    signed_transaction = util.buildCallTransaction(wallet, score_address, "fix")
    result = icon_service.send_transaction(signed_transaction)
    print(result)
```

```python
    @external
    def fix(self):
        """
        핸드를 픽스한다.(카드를 더 받지 않음) 게임 종료 조건을 만족하면 승패를 계산한다.
        :return:
        """
        game_room_id = self._DDB_in_game_room[self.msg.sender]
        hand_dict = json_loads(self._DDB_hand[self.msg.sender])
        hand = Hand(hand_dict['cards'], hand_dict['value'], hand_dict['aces'], hand_dict['fix'])

        hand.fix = True
        self._DDB_hand[self.msg.sender] = str(hand)
        self.Fix(self.msg.sender, game_room_id)

        if self._check_participants_fix(game_room_id) or self.block.height - self._DDB_game_start_time[game_room_id] > 60:
            self.calculate(game_room_id)
            self.Calculate(game_room_id)


    def calculate(self, game_room_id: Address = None):
    """
    승패를 계산한다.
    :param game_room_id:
    :return:
    """
    self.Calculate(game_room_id)
    chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)

    # Finalize the game
    self._game_stop(game_room_id)

    # Calculate the result
    # 두참여자의 핸드 정보를 가져옴.
    game_room_dict = json_loads(self._DDB_game_room[game_room_id])
    game_room = GameRoom(Address.from_string(game_room_dict['owner']), Address.from_string(game_room_dict['game_room_id']), game_room_dict['creation_time'],
                         game_room_dict['prize_per_game'], game_room_dict['participants'], game_room_dict['active'])
    participants = game_room.participants
    participant_gen = (participant for participant in participants)
    first_participant = next(participant_gen)
    first_hand_dict = json_loads(self._DDB_hand[Address.from_string(first_participant)])
    first_hand = Hand(first_hand_dict['cards'], first_hand_dict['value'], first_hand_dict['aces'], first_hand_dict['fix'])
    second_participant = next(participant_gen)
    second_hand_dict = json_loads(self._DDB_hand[Address.from_string(second_participant)])
    second_hand = Hand(second_hand_dict['cards'], second_hand_dict['value'], second_hand_dict['aces'], second_hand_dict['fix'])

    results = self._get_results()
    loser = ""
    # 승자에게 칩 전송
    if first_hand.value > 21 or second_hand.value > 21:
        chip.transfer(Address.from_string(first_participant), game_room.prize_per_game * 2) if second_hand.value > 21 else chip.transfer(Address.from_string(second_participant), game_room.prize_per_game * 2)
        results.put(f"{first_participant} wins against {second_participant}.") if second_hand.value > 21 else results.put(f"{second_participant} wins against {first_participant}.")
        loser = first_participant if first_hand.value > 21 else second_participant
    elif first_hand.value > second_hand.value:
        chip.transfer(Address.from_string(first_participant), game_room.prize_per_game * 2)
        results.put(f"{first_participant} wins against {second_participant}.")
        loser = second_participant
    elif first_hand.value < second_hand.value:
        chip.transfer(Address.from_string(second_participant), game_room.prize_per_game * 2)
        results.put(f"{second_participant} wins against {first_participant}.")
        loser = first_participant
    else:
        chip.transfer(Address.from_string(first_participant), game_room.prize_per_game)
        chip.transfer(Address.from_string(second_participant), game_room.prize_per_game)
        results.put(f"Draw!! {first_participant}, {second_participant}.")

    #패자가 돈이 없으면 강퇴
    if loser != "" and game_room.prize_per_game > chip.balanceOf(Address.from_string(loser)):
        self._ban(game_room_id, Address.from_string(loser))
```

##### 게임 결과 확인
```python
from play_blackjack.prerequisite import *

call = util.buildCall(wallets[0], score_address, "getResults")
result = icon_service.call(call)
print(result)
```
```python
    @external(readonly=True)
    def getResults(self) -> list:
        return list(self._get_results())
```

##### 방 나가기
```python
from play_blackjack.prerequisite import *

signed_transaction = util.buildCallTransaction(wallets[1], score_address, "escape")
result = icon_service.send_transaction(signed_transaction)
print(result)
```
```python
    @external
    def escape(self):
        # Check whether 'self.msg.sender' is now participating to game room or not
        if self._DDB_in_game_room[self.msg.sender] is None:
            revert(f'No game room to escape')

        # Retrieve the game room ID & Check the game room status
        game_room_id_to_escape = self._DDB_in_game_room[self.msg.sender]
        game_room_to_escape_dict = json_loads(self._DDB_game_room[game_room_id_to_escape])
        game_room_to_escape = GameRoom(Address.from_string(game_room_to_escape_dict['owner']), Address.from_string(game_room_to_escape_dict['game_room_id']),
                                       game_room_to_escape_dict['creation_time'], game_room_to_escape_dict['prize_per_game'],
                                       game_room_to_escape_dict['participants'], game_room_to_escape_dict['active'])

        if game_room_to_escape.active:
            revert("The game is not finalized yet.")

        # Escape from the game room
        if game_room_to_escape.owner == self.msg.sender:
            if len(game_room_to_escape.participants) == 1:
                game_room_to_escape.escape(self.msg.sender)
                self._crash_room(game_room_id_to_escape)
            else:
                revert("Owner can not escape from room which has the other participant")
        else:
            game_room_to_escape.escape(self.msg.sender)
            self._DDB_game_room[game_room_id_to_escape] = str(game_room_to_escape)

        # Set the in_game_room status of 'self.msg.sender' to None
        game_room_list = self._get_game_room_list()
        game_room_index_gen = (index for index in range(len(game_room_list)) if game_room_to_escape.game_room_id == Address.from_string(json_loads(game_room_list[index])['game_room_id']))

        try:
            index = next(game_room_index_gen)
            game_room_list[index] = str(game_room_to_escape)
        except StopIteration:
            pass

        self._DDB_in_game_room.remove(self.msg.sender)
```
##### ICX로 환전
```python
from play_blackjack.prerequisite import *

for wallet in wallets:
    signed_transaction = util.buildCallTransaction(wallet, score_address, "exchange", amount=5000000000000000000)
    result = icon_service.send_transaction(signed_transaction)
    print(result)
```
```python
    @external
    def exchange(self, amount: int):
        """
        토큰용 SCORE를 이용해 호출자의 칩을 소각하고 ICX로 환전
        :param amount:
        :return:
        """
        chip = self.create_interface_score(self._VDB_token_address.get(), ChipInterface)
        chip.burn(amount)
        self.icx.transfer(self.msg.sender, amount)
```
