# ICON Workshop 3일

<!-- TOC -->

- [ICON Workshop 3일](#icon-workshop-3%EC%9D%BC)
    - [DApp](#dapp)
        - [DApp의 정의](#dapp%EC%9D%98-%EC%A0%95%EC%9D%98)
        - [DApp의 분류](#dapp%EC%9D%98-%EB%B6%84%EB%A5%98)
        - [DApp의 구현방식](#dapp%EC%9D%98-%EA%B5%AC%ED%98%84%EB%B0%A9%EC%8B%9D)
        - [블록체인의 저주](#%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%EC%9D%98-%EC%A0%80%EC%A3%BC)
        - [불록체인의 저주를 푸는 방법들](#%EB%B6%88%EB%A1%9D%EC%B2%B4%EC%9D%B8%EC%9D%98-%EC%A0%80%EC%A3%BC%EB%A5%BC-%ED%91%B8%EB%8A%94-%EB%B0%A9%EB%B2%95%EB%93%A4)
        - [메시지를 서명 방법](#%EB%A9%94%EC%8B%9C%EC%A7%80%EB%A5%BC-%EC%84%9C%EB%AA%85-%EB%B0%A9%EB%B2%95)
        - [ICON에서 DApp 개발시 장점](#icon%EC%97%90%EC%84%9C-dapp-%EA%B0%9C%EB%B0%9C%EC%8B%9C-%EC%9E%A5%EC%A0%90)
            - [배우기 쉽다](#%EB%B0%B0%EC%9A%B0%EA%B8%B0-%EC%89%BD%EB%8B%A4)
            - [트랜잭션의 처리속도가 빠르다.](#%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98-%EC%B2%98%EB%A6%AC%EC%86%8D%EB%8F%84%EA%B0%80-%EB%B9%A0%EB%A5%B4%EB%8B%A4)
            - [수수료가 더 싸다.](#%EC%88%98%EC%88%98%EB%A3%8C%EA%B0%80-%EB%8D%94-%EC%8B%B8%EB%8B%A4)
            - [인터체인](#%EC%9D%B8%ED%84%B0%EC%B2%B4%EC%9D%B8)
            - [건전한 네트워크 지향](#%EA%B1%B4%EC%A0%84%ED%95%9C-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%A7%80%ED%96%A5)
                - [I-Score 평가항목](#i-score-%ED%8F%89%EA%B0%80%ED%95%AD%EB%AA%A9)
                - [9월 P-Rep 선거](#9%EC%9B%94-p-rep-%EC%84%A0%EA%B1%B0)
        - [Audit](#audit)
            - [Audit Checklist](#audit-checklist)
                - [Critical](#critical)
                    - [Timeout](#timeout)
                    - [Eventlog on Token Transfer](#eventlog-on-token-transfer)
                    - [Eventlog without Token Transfer](#eventlog-without-token-transfer)
                    - [Super Class](#super-class)
                    - [Temporary Limitation](#temporary-limitation)
    - [Javascript SDK](#javascript-sdk)
        - [SDK 사용법](#sdk-%EC%82%AC%EC%9A%A9%EB%B2%95)
            - [load the wallet](#load-the-wallet)
            - [Add EventHandler](#add-eventhandler)
            - [Build transaction payload and request json-rpc](#build-transaction-payload-and-request-json-rpc)

<!-- /TOC -->

## DApp
> [Decentralized Applications White Paper and Spec](https://github.com/DavidJohnstonCEO/DecentralizedApplications)
* 탈중앙화된 어플리케이션

---

### DApp의 정의
1. 어플리케이션은 완전한 오픈소스이며 자율적으로 운영되며, 대부분의 토큰을 제어하는 주체가 없어야한다. 모든 변경사항은 사용자 합의에 의해 결정된다.
2. 어플리케이션 데이터및 작업 기록은 반드시 공개, 분산된 블록체인에 암호화하여 저장해 중앙집중식 실패지점을 피해야 한다.
3. 어플리케이션은 암호화된 토큰을 사용해야하며 토큰은 어플리케이션에 접근하고 마이너에게 리워드로 제공되기 위해 사용된다.
4. 어플리케이션은 노드가 기여도 값을 증명하는 표준 암호 알고리즘에 따라 토큰을 생성해야 한다.

---

### DApp의 분류
1. Type1
    - 블록 체인을 가지고 있는 탈중앙화된 어플리케이션
    - ex) 비트코인
2. Type2
    - Type1 DApp의 블록체인을 이용한 탈중앙화된 어플리케이션.
    - 앱의 기능을 실행하기 위한 토큰과 프로토콜을 가지고 있다.
    - ex) [Omni Layer](https://www.omnilayer.org/)
3. Type3
    - Type2 DApp의 프로토콜을 사용하는 탈중앙화된 어플리케이션.
    - 앱의 기능을 실행하기 위한 토큰과 프로토콜을 가지고 있다.
    - ex) [SAFE Network](https://safenetwork.org/)

> **비유하자면** Type1은 OS(Windows, Linux), Type2는 범용소프트웨어(워드, 엑셀, Dropbox), Type3는 특화된 소프트웨어(워드를 사용한 메일머지툴, 스프레드시트의 매크로, 드랍박스 기반의 블로그서비스).

---

### DApp의 구현방식
1. Contract-Only
    - 비즈니스 로직이 스마트 컨트랙트에만 존재하는 형태
    - EX) ICO
2. Hybrid
    - 비즈니스 로직이 스마트컨트랙트 + Offchain 서비스 투트랙으로 구현된 형태
3. Save Data Only
    - OffChain 서비스 + 블록체인을 데이터 저장소로만 사용하는 형태

---

### 블록체인의 저주
1. **과다한 수수료**: 어떤 트랜잭션을 발생시키든 수수료가 있으며 트랜잭션이 실패해도 수수료가 지불되는 수수료 지옥.
![hell_of_fee](images/2019/04/hell-of-fee.png)
2. **처리의 신속성**: 트랜잭션을 블록에 넣고 채굴을 통해 등록시키고 6번의 검증과정을 거쳐야 함.
3. **서명**: 모든 트랜잭션은 서명이 되어야 한다.
4. **지갑**: 지갑은 일종의 private key 개념인데 잃어버리면 코인을 찾을 수 없다.

---

### 불록체인의 저주를 푸는 방법들
1. 탈중앙화된 ID: 식별자를 관리하는 블록체인
2. hybrid: 인증에 OAuth방식 도입 (AccessToken).
3. 블록체인을 데이터 저장용도로만 사용.

---

### 메시지를 서명 방법
1. 지갑앱을 이용하여 서명
2. keystore 파일을 직접 로드하여 서명 (취약함)


---

### ICON에서 DApp 개발시 장점
1. 배우기 쉽다.
2. 트랜잭션의 처리속도가 빠르다.
3. 수수료가 더 싸다.
4. 인터체인
5. 건전한 네트워크 지향


#### 배우기 쉽다
![python-learning-curve](images/2019/04/python-learning-curve.png)
- 스마트컨트랙트를 Python으로 개발하기 때문에 새로운 언어를 배워야 하는 부담이 없다.
- 파이썬의 러닝커브는 완만한 편이고, Solidity에 비해 접근성이 높다.
- 개발자 포털, 공식 Guide, ICON Workshop등 공식가이드가 비교적 잘 되어 있다.

#### 트랜잭션의 처리속도가 빠르다.
- 이더리움의 TPS는 약 20, 아이콘은 1000
- 이더리움은 PoW 특성상 최소 6컨펌은 받아야 하지만 아이콘은 1컨펌만 받아도 됨.
- 이더리움은 2019/03/10 기준으로 약 20초마다 블록이 생성됨. 즉 트랜잭션이 처리되기까지 20 * 6초 만큼의 시간이 소요됨

#### 수수료가 더 싸다.
- 이더리움의 약 3.3% 수준
- DApp 개발자가 트랜잭션의 수수료를 유저가 지불할지 개발자가 지불할지 결정할 수 있고 그 비율도 결정 가능하다.
- Virtual Step 기능으로 개발자의 수수료 부담을 덜 수 있다.
> **Virtual Step** 은 개발자가 일정량의 ICX를 거치하면 ICON이 거치한 양에 비례하여 가상의 수수료를 지원해주는 기능이다.

#### 인터체인
- 타플랫폼, 블록체인을 이을 수 있는 인터체인을 지향함.
- 인터체인이란 다른 블록체인들을 서로 연결하기 위한 체인이다.
- A 암호화폐로만 살 수 있는 것들을 인터체인으로 연결된 B 암호화폐로도 구매가 가능하다.
- 단순 가치의 교환뿐만 아니라 서비스의 확장 가능성까지 부여한다.
- 이러한 인터체인을 이용하면 Oracle Problem을 상당부분 해소할 수 있다.
>
> **Oracle Problem** 이란 블록체인 밖에 있는 데이터를 블록체인 안으로 가져올 때 발생하는 문제를 말한다. 블록체인 안에 있는 데이터는 위변조가 거의 불가능하지만 데이터가 블록체인 안으로 들어오는 과정에서 위변조가 발생한다면 그 데이터가 블록체인 안에 있을지라도 신뢰할 수 없게된다.
>
#### 건전한 네트워크 지향
- 해당 네트워크에 기여한 사람에게 보상을 줄 수 있도록 설계. (IISS)
- IISS는 DPoC 방식을 지향함.
- DApp 개발도 기여(Contribution)에 해당함.
- 기여도에 따라 I-Score라는 점수를 부여 받는다.

> **IISS(ICON Incentives Scoring System)**: ICON 네트워크 내에서 수많은 구성원들의 기여도를 정확하게 측정하고, 보상하기 위한 AI기반 평가 시스템이다.

> **DPoC(Deligate Proof Of Contribution)**: 네트워크 참여자가 네트워크에 기여한 정도를 평가하여 점수로 환산하고, 이러한 점수를 각각의 대표자에게 위임(투표)하여 위임받은 점수에 따라 네트워크의 대표자들을 선출하는 방식을 말한다. 이 대표자들(노드)이 블록 생성을 담당하게 된다.

> **I-Score** : 네트워크 활성화에 기여한 정도에 따라 받는 점수로, **ICX를 청구할 수 있는 권리** 를 의미한다. 모든 참여자들은 ICON Network에 기여하는 활동을 직접적으로 수행하거나,
특정 대상에게 지분을 위임함으로써 I-Score를 획득할 수 있다. I-Score는 사용자 간 전송 할 수 없으며, 사용자 계정을 통해 Public Treasury의 ICX를 청구할 수 있다.

##### I-Score 평가항목
항목 | 설명
------|--------
대표자 보상 ( β1 ) |  대표자가 블록을 생성 및 검증할 때, 블록 생성 및 검증에 대한 보상
거래 수수료 보상 ( β2 ) | 대표자가 블록을 생성 및 검증할 때, 블록 내 존재하는 거래에서 발생한 수수료에 대한 보상
대표자 위임 보상 ( β3 ) | 대표자에게 지분을 위임하고 있을 때, 지분의 위임에 대한 보상
EEP 보상 ( β4 ) |  EEP가 지분을 위임 받았을 때, 지분에 대한 보상
EEP 위임 보상 ( β5 ) |  EEP에게 지분을 위임하고 있을 때, 지분의 위임에 대한 보상
DApp 보상 ( β6 ) |  DApp이 지분을 위임 받았을 때, 지분에 대한 보상
DApp 위임 보상 ( β7 ) | DApp에게 지분을 위임하고 있을 때, 지분의 위임에 대한 보상


##### 9월 P-Rep 선거
- 블록체인에서 블록의 생성과 검증을 담당하는 대표자(노드)를 선출하는 선거
- 네트워크의 참여자는 보상받은 I-SCORE를 다른 사용자에게 위임할 수 있고
- 네트워크의 참여자는 일정량의 I-SCORE 조건을 충족하게 된다면 자신이 속한 네트워크의 대표자가 될 수 있는 투표에 후보자로 등록할 수 있다.

---

### Audit
SCORE가 배포될 때 SCORE 코드가 네트워크에 해가 되지 않는지 검증하는 과정.

#### Audit Checklist
https://www.icondev.io/docs/audit-checklist

##### Critical
- Timeout
- Unfinishing Loop
- Package import
- System Call
- Outbound Network Call
- iconservice Internal API
- Randomness
- Fixed SCORE Infomation
- IRC2 Token Standard Compliance
- IRC2 Token Parameter Name
- Eventlog on Token Transfer
- Eventlog without Token Transfer
- ICXTransfer Eventlog
- Super Class
- Keyword Arguments
- Big Number Operation
- Instance Variable
- StateDB Operation
- StateDB Write Operation
- Temporary Limitation

###### Timeout
장시간 동작하는 함수는 사용할 수 없다.
```python
# Bad
@external
def airdrop_token(self, _value: int, _data: bytes = None):
    for target in self._very_large_targets:
        self._transfer(self.msg.sender, target, _value, _data)

# Good
@external
def airdrop_token(self, _to: Address, _value: int, _data: bytes = None):
    if self._airdrop_sent_address[_to]:
        self.revert(f"Token was dropped already: {_to}")
    self._airdrop_sent_address[_to] = True
    self._transfer(self.msg.sender, _to, _value, _data)
```
###### Eventlog on Token Transfer
토큰의 전송은 반드시 EventLog를 발생시켜야 한다.
```python
# Good
@eventlog(indexed=3)
def Transfer(self, _from: Address, _to: Address, _value: int, _data: bytes):
    pass

@external
def transfer(self, _to: Address, _value: int, _data: bytes = None):
    self._balances[self.msg.sender] -= _value
    self._balances[_to] += _value
    self.Transfer(self.msg.sender, _to, _value, _data)
```
###### Eventlog without Token Transfer
토큰 전송 없이 Eventlog를 발생시켜선 안된다.
```python
# Bad
@eventlog(indexed=3)
def Transfer(self, _from: Address, _to: Address, _value: int, _data: bytes):
    pass

@external
def doSomething(self, _to: Address, _value: int):
    #  no token transfer occurred
    self.Transfer(self.msg.sender, _to, _value, None)
```

###### Super Class
IconScoreBase를 상속받은 클래스는 \_\_init\_\_, on_install, on_update 에서 super의 메서드를 호출해야 한다.
```python
# Bad
class MyClass(IconScoreBase):
    def __init__(self, db: IconScoreDatabase) -> None:
        self._context__name = VarDB('context.name', db, str)
        self._context__cap = VarDB('context.cap', db, int)

    def on_install(self, name: str, cap: str) -> None:
        # doSomething

    def on_update(self) -> None:
        # doSomething

# Good
class MyClass(IconScoreBase):
    def __init__(self, db: IconScoreDatabase) -> None:
        super().__init__(db)
        self._context__name = VarDB('context.name', db, str)
        self._context__cap = VarDB('context.cap', db, int)

    def on_install(self, name: str, cap: str) -> None:
        super().on_install()
        # doSomething

    def on_update(self) -> None:
        super().on_update()
        # doSomething
```

###### Temporary Limitation
ArrayDB 이슈로 인해, init()에서 ArrayDB를 멤버변수로 선언할 수 없음. ArrayDB 인스턴스를 사용시 매번 초기화 하는 방식으로 사용
```python
# Problematic (Original Usage)
def __init__(self, db: IconScoreDatabase) -> None:
    super().__init__(db)
    self.test_array = ArrayDB('test_array', db, value_type=int)

def func(self) -> None:
    self.test_array.put(0)


# Good (Temporary)
@property
def test_array(self) -> ArrayDB:
    return ArrayDB('test_array', db, value_type=int)

def __init__(self, db: IconScoreDatabase) -> None:
    super().__init__(db)
    # no declaration

def func(self) -> None:
    self.test_array.put(0)
```

---

## Javascript SDK
ICON Javascript SDK에서는 ICONex 지갑을 사용하여 서명하고 JSON-RPC 요청을 함.

### SDK 사용법
- load the wallet
- Add EventHandler
- Build transaction payload and request json-rpc

#### load the wallet
dispatchEvent 메서드로 ICONEX_RELAY_REQUEST라는 지갑관련 이벤트를 발생시키고 이벤트의 type 파라미터를 통해 이벤트 종류를 구분한다.
```javascript
requestaddress.onclick = function() {
    window.dispatchEvent(new CustomEvent('ICONEX_RELAY_REQUEST', {
        detail: {
            type: 'REQUEST_ADDRESS'
        }
    }));
}
```

#### Add EventHandler
지갑으로부터 받은 응답을 처리할 이벤트핸들러를 등록한다.
```javascript
window.addEventListener("ICONEX_RELAY_RESPONSE", eventHandler, false);

function eventHandler(event) {
    var type = event.detail.type;
    var payload = event.detail.payload;
    switch (type) {
        case "RESPONSE_ADDRESS":
            fromAddress = payload;
            break;
        case "RESPONSE_JSON-RPC":
            setTimeout(function (hash) {
                iconService.getTransactionResult(payload.result).execute().then(function(result){
                    scoreresponse.innerHTML = JSON.stringify(result);
                }, function(error) {
                    console.log(error)
                });
            }, 4000);
            break;
        default:
    }
}
```


#### Build transaction payload and request json-rpc
트랜잭션 데이터를 빌드하고 json-rpc 통신을 한다
```javascript
function joinbtnclick() {
    // 트랜잭션 데이터를 빌드한다.
    var callTransactionBuilder = new IconBuilder.CallTransactionBuilder;
    var callTransferData = callTransactionBuilder
        .from(fromAddress)
        .to("cx89245b4a663f2062a9fe52a219c44c281e1d6c36")
        .nid(IconConverter.toBigNumber(3))
        .timestamp((new Date()).getTime() * 1000)
        .version(IconConverter.toBigNumber(3))
        .stepLimit(IconConverter.toBigNumber(10000000))
        .method("joinRoom")
        .params({
            "_gameRoomId": joinbtn.value
        })
        .build();

    //JSON-RPC 통신용 데이터를 조립
    scoreData = JSON.stringify({
        "jsonrpc": "2.0",
        "method": "icx_sendTransaction",
        "params": IconConverter.toRawTransaction(callTransferData),
        "id": 8015
    });

    //ICX 지갑 존재여부 확인
    var parsed = JSON.parse(scoreData);
    if (parsed.method === "icx_sendTransaction" && !fromAddress) {
        alert('Select the ICX Address');
        return;
    }

    //ICONEX_RELAY_REQUEST 이벤트를 발생시키고 데이터를 넘긴다.
    window.dispatchEvent(new CustomEvent('ICONEX_RELAY_REQUEST', {
        detail: {
            type: 'REQUEST_JSON-RPC',
            payload: parsed
        }
    }))
}
```
