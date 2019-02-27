---
layout: post
title: ICON Workshop 1일차
date: '2019-02-22 15:26'
---

ICON Workshop 1일
<!-- TOC -->

- [1. T-Bears](#1-t-bears)
    - [T-Bears란?](#t-bears%EB%9E%80)
        - [기능](#%EA%B8%B0%EB%8A%A5)
        - [일반노드와 차이점](#%EC%9D%BC%EB%B0%98%EB%85%B8%EB%93%9C%EC%99%80-%EC%B0%A8%EC%9D%B4%EC%A0%90)
        - [트랜잭션 배포시 고려할 점](#%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EB%B0%B0%ED%8F%AC%EC%8B%9C-%EA%B3%A0%EB%A0%A4%ED%95%A0-%EC%A0%90)
    - [T-Bears 설치](#t-bears-%EC%84%A4%EC%B9%98)
        - [Docker로 설치하는 법(recommended)](#docker%EB%A1%9C-%EC%84%A4%EC%B9%98%ED%95%98%EB%8A%94-%EB%B2%95recommended)
        - [pip로 설치하는 법](#pip%EB%A1%9C-%EC%84%A4%EC%B9%98%ED%95%98%EB%8A%94-%EB%B2%95)
        - [소스로 빌드하는 방법](#%EC%86%8C%EC%8A%A4%EB%A1%9C-%EB%B9%8C%EB%93%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)
    - [T-Bears 사용](#t-bears-%EC%82%AC%EC%9A%A9)
        - [tbears 기본 명령어](#tbears-%EA%B8%B0%EB%B3%B8-%EB%AA%85%EB%A0%B9%EC%96%B4)
- [2. ICON SDK](#2-icon-sdk)
    - [SDK 종류](#sdk-%EC%A2%85%EB%A5%98)
    - [SDK 설치](#sdk-%EC%84%A4%EC%B9%98)
        - [JAVA](#java)
            - [Maven](#maven)
            - [Gradle](#gradle)
        - [Python (Only support 3.6)](#python-only-support-36)
        - [Javascript](#javascript)
            - [CDN](#cdn)
            - [npm](#npm)
- [3. Tracker](#3-tracker)
- [4. ICONex](#4-iconex)
    - [기능](#%EA%B8%B0%EB%8A%A5-1)
    - [Wallet 생성](#wallet-%EC%83%9D%EC%84%B1)
    - [테스트넷 접속](#%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%84%B7-%EC%A0%91%EC%86%8D)
- [5. 샘플실습](#5-%EC%83%98%ED%94%8C%EC%8B%A4%EC%8A%B5)
    - [Prerequisite](#prerequisite)
    - [deploy hello world](#deploy-hello-world)
        - [CLI로 배포](#cli%EB%A1%9C-%EB%B0%B0%ED%8F%AC)
        - [트랜잭션 확인](#%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%ED%99%95%EC%9D%B8)
        - [tbears_cli_config.json을 수정](#tbears_cli_configjson%EC%9D%84-%EC%88%98%EC%A0%95)
        - [수정한 json으로 다시 배포](#%EC%88%98%EC%A0%95%ED%95%9C-json%EC%9C%BC%EB%A1%9C-%EB%8B%A4%EC%8B%9C-%EB%B0%B0%ED%8F%AC)
        - [tbears 테스트](#tbears-%ED%85%8C%EC%8A%A4%ED%8A%B8)
    - [JSON-RPC로 트랜잭션 실행해보기](#json-rpc%EB%A1%9C-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EC%8B%A4%ED%96%89%ED%95%B4%EB%B3%B4%EA%B8%B0)
        - [1 ICX 전송하기](#1-icx-%EC%A0%84%EC%86%A1%ED%95%98%EA%B8%B0)
        - [SCORE의 함수 호출](#score%EC%9D%98-%ED%95%A8%EC%88%98-%ED%98%B8%EC%B6%9C)

<!-- /TOC -->
---
## 1. T-Bears
### T-Bears란?
ICON SCORE를 개발하기 위한 테스트도구 및 환경.
https://github.com/icon-project/t-bears
>**SCORE는** 이더리움의 스마트컨트랙트에 해당

##### 기능
- 테스트노드 환경을 구성
- 합의로직 없이 블록을 자동으로 구성해줌.

##### 일반노드와 차이점
- T-Bears Block Manager가 일반노드의 Loopchain을 대체
- T-Bears CLI가 Client 대체
- T-Bears가 Node 대체

##### 트랜잭션 배포시 고려할 점
로컬개발환경과 달리 테스트넷, 메인넷에 배포할 때 고려해야할 사항.
1. Timestamp(트랜잭션 생성시간)
블록 생성시간과 트랜잭션 타임스탬프는 반드시 5분이내의 갭이어야 함.
> 로컬개발환경에서는 Timestamp를 설정하지 않아도 됨.
2. Signature
사인은 트랜잭션데이터 내에 포함되어 있어야함.
사인은 Keystore파일을 이용해 생성 가능.
3. Steplimit (이더리움의 gasLimit과 비슷)
일반적으로 배포시 최소 10ICX가량 소모됨.
배포시 Steplimit을 낮게 설정하면 배포는 되지 않고 ICX만 차감될 수 있음.

### T-Bears 설치
> 윈도우 환경에선 설치할 수 없음.
#### Docker로 설치하는 법(recommended)
```
$ docker run -it --name local-tbears -p 9000:9000 iconloop/tbears
```
설치 완료시 아래와 같이 컨테이너 쉘에 접속됨.
```
root@07dfee84208e:/tbears#
```

#### pip로 설치하는 법
```bash
# Install levelDB
$ sudo apt-get install libleveldb1 libleveldb-dev
# Install libSecp256k
$ sudo apt-get install libsecp256k1-dev

# install RabbitMQ and start service
$ sudo apt-get install rabbitmq-server

# Create a working directory
$ mkdir work
$ cd work

# Setup the python virtualenv development environment
# virtualenv 대신 conda로 가상환경 구성도 가능
$ virtualenv -p python3 .
$ source bin/activate

# Install the ICON SCORE dev tools
(work) $ pip install tbears
```

#### 소스로 빌드하는 방법
프로젝트를 clone후 가상환경을 구성하고 빌드 스크립트를 실행.
```bash
$ virtualenv -p python3 venv  # Create a virtual environment.
$ source venv/bin/activate    # Enter the virtual environment.
(venv)$ ./build.sh            # run build script
(venv)$ ls dist/              # check result wheel file
tbears-x.y.z-py3-none-any.whl
```

<style>
table thead {
    background-color: #F8F8F8
}
</style>
### T-Bears 사용
##### tbears 기본 명령어
No | Command | Description
---|---------|------------
1  | init    | 새로운 T-bears 프로젝트를 초기화하며 생성
2  | deploy  | SCORE 배포
3  | txresult| 트랜잭션 hash값으로 트랜잭션 결과를 조회
4  | call    | JSON파일로 icx_call 요청을 보냄 (JSON RPC)  
5  | sendtx  | JSON파일로 icx_sendTransaction 요청을 보냄 (JSON RPC)
6  | scoreapi| SCORE 주소로 SCORE API를 가져옴. (스마트컨트랙트의 메서드를 가져오는듯)

## 2. ICON SDK
ICON은 Solidity 같은 전용언어를 사용하지 않고 SDK를 통해 자신에게 익숙한 언어로 SCORE를 개발할 수 있는 장점이 있다.

### SDK 종류
- Java [https://github.com/icon-project/icon-sdk-java]
- Python [https://github.com/icon-project/icon-sdk-python]
- Javascript [https://github.com/icon-project/icon-sdk-js]

### SDK 설치
#### JAVA
###### Maven
```xml
 <dependencies>
     <dependency>
         <groupId>foundation.icon</groupId>
         <artifactId>icon-sdk</artifactId>
         <version>[x.y.z]</version>
     </dependency>
 </dependencies>
```
###### Gradle
```
dependencies { implementation 'foundation.icon:icon-sdk:[x.y.z]' }
```

#### Python (Only support 3.6)
아나콘다로 파이썬 3.6 가상환경 생성 후 사용
```bash
$ conda create --name icon python=3.6
$ conda activate icon
(icon) $ pip install iconsdk
```
> T-bears가 없는 환경에서 pip로 SDK 설치시 libsecp256k1가 없어 오류가 발생한적 있음.
> `$ brew install autoconf automake libtool pkg-config` 로 인스톨해 해결함.
> linux 에서는 `$ sudo apt-get install libsecp256k1-dev pkg-config`

#### Javascript
###### CDN
```html
<script src=”https://cdn.jsdelivr.net/gh/icon-project/icon-sdk-js/build/icon-sdk-js.min.js”></script>
```
###### npm
```bash
$ npm install --save icon-sdk-js
```

## 3. Tracker
트랙커를 통해 블록, 트랜잭션, 컨트랙트를 살펴볼 수 있다. 컨트랙트의 소스코드를 보거나 다운로드가 가능하다.
- 메인넷 트랙커 : https://tracker.icon.foundation
- 테스트넷 트랙커 : https://bicon.tracker.solidwallet.io

## 4. ICONex
ICON 지갑 크롬확장프로그램이다.

#### 기능
- 트랜잭션 전송
- 계정 생성
    - hx로 시작하면 일반 계정, cx로 시작하면 컨트랙트 계정

#### Wallet 생성
1. ICONex 설치후 Create Wallet 클릭
2. ICON (ICX) 선택후 다음
3. 지갑이름과 패스워드 설정 후 다
4. **키스토어파일을 다운로드. (반드시 백업해야 함)**
5. private key 또한 확인 후 백업

#### 테스트넷 접속
1. chrome 개발자 도구 진입
2. Application 탭 클릭
3. Local Storage에서 isDev : true로 설정 후 새로고침
![테스트넷 접속](https://icon-project.github.io/docs/images/iconex-isdev.png)
4. 하단 footer에서 ICX(SERVER)를 여의도로 설정.
![테스트넷접속2](https://icon-project.github.io/docs/images/iconex-network.png)

## 5. 샘플실습
### Prerequisite
1. 도커에 T-bears 컨테이너 배포
```bash
$ docker run -it --name tbears-container -p 9000:9000 iconloop/tbears
```
2. Faucet에 접속하여 테스트 ICX 수령
http://52.88.70.222
3. wallet에서 백업한 키스토어를 복사후 docker에 key라는 파일로 저장함.
```
root@f1831fff63ed:/tbears/test# cat key
{"version":3,"id":"692926c4-62ee-4e90-8729-b631ea2f7d26","address":"hxb1c73381e270d401a8f8b3f969f97457cca32d6d","crypto":{"ciphertext":"d94722ea442011c309935331d4ae8db46949d64dacbe47234e3b286ed43001fe","cipherparams":{"iv":"408b33166344be558deb164ab7cf8a8d"},"cipher":"aes-128-ctr","kdf":"scrypt","kdfparams":{"dklen":32,"salt":"04d3225f7e91e28821eda13de2b5dc2b8b4234d28695739505afe437600cd22d","n":16384,"r":8,"p":1},"mac":"26b7fe177eac64b99054ef2328574a87c1258a2ff1bfaf7af534cec15897830b"},"coinType":"icx"}
```
4. init 명령으로 SCORE 프로젝트 생성함.
usage : ``tbears init <project name> <class name>``
```bash
$ tbears init hello Hello
Initialized hello successfully
```

### deploy hello world
##### CLI로 배포
```
$ tbears deploy hello -k key -u https://bicon.net.solidwallet.io/api/v3
Input your keystore password:
Send deploy request successfully.
If you want to check SCORE deployed successfully, execute txresult command
transaction hash: 0x6ef71c7055a64f817100c6f322312e2e3d010cb5d27c3e0e113d5b319085347f
```
##### 트랜잭션 확인
```
$ tbears txresult 0x6ef71c7055a64f817100c6f322312e2e3d010cb5d27c3e0e113d5b319085347f
Got an error response
Can not get transaction result
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32602,
        "message": "Pending transaction"
    },
    "id": 1
}
```
>**Note:** tbears_cli_config.json 설정파일의 deploy.stepLimit이 값이 너무 작기 때문에 배포에 실패했다. stepLimit은 이더리움에서 gasLimit과 비슷한 개념으로 소비할 ICX한도를 정할 수 있다. 기본적으로 SCORE를 배포할 때는 10 ICX 이상이 소비 되므로 stepLimit 값을 높여줘야한다.
> default 값은 0x10000000으로 268,435,456, 즉 2.68ICX에 해당한다.

##### tbears_cli_config.json을 수정
tbears_cli_config.json을 아래와 같이 수정
<style>
    pre {
        background-color: #F5F5F5;
        font-family: consolas
    }
    yel {
        background-color:#FFFF00;
    }
</style>
<pre>
{
    <yel>"uri": "https://bicon.net.solidwallet.io/api/v3"</yel>,
    "nid": "0x3",
    "keyStore": null,
    "from": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
    "to": "cx0000000000000000000000000000000000000000",
    "deploy": {
        <yel>"stepLimit": "0x59682f00"</yel>,
        "mode": "install",
        "scoreParams": {}
    },
    "txresult": {},
    "transfer": {
        "stepLimit": "0xf4240"
    }
}
</pre>
##### 수정한 json으로 다시 배포
```
$ root@f1831fff63ed:/tbears/test# tbears deploy hello -k key
Input your keystore password:
Send deploy request successfully.
If you want to check SCORE deployed successfully, execute txresult command
transaction hash: 0x0606179afe3037b448bfac1ea7efcca2da15a5689d44529236a02fafa2c92c52

$ root@f1831fff63ed:/tbears/test# tbears txresult 0x0606179afe3037b448bfac1ea7efcca2da15a5689d44529236a02fafa2c92c52
Transaction result: {
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0x0606179afe3037b448bfac1ea7efcca2da15a5689d44529236a02fafa2c92c52",
        "blockHeight": "0x2040b",
        "blockHash": "0xd5e7b6e44ead3f86c2d1d329f686aca8e4e7539cd4e537b3308b3638976dd0fb",
        "txIndex": "0x0",
        "to": "cx0000000000000000000000000000000000000000",
        "scoreAddress": "cx2726c62bab30071ced0af632233c734f5db917ad",
        "stepUsed": "0x3cbc5ed0",
        "stepPrice": "0x2540be400",
        "cumulativeStepUsed": "0x3cbc5ed0",
        "eventLogs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1"
    },
    "id": 1
}
```

##### tbears 테스트
init으로 기본 프로젝트를 생성하면 tests/폴더에 test_projectname.py로 기본 단위테스트도 같이 생성된다.
아래와 같이 테스트코드를 실행할 수 있다.
```
$ tbears test hello
..
----------------------------------------------------------------------
Ran 2 tests in 0.174s

OK
```

### JSON-RPC로 트랜잭션 실행해보기
icx_sendTransaction 스펙 : https://icondev.readme.io/docs/json-rpc-specification#section-icx_sendtransaction
icx_call 스펙 : https://icondev.readme.io/docs/json-rpc-specification#section-icx_call

##### 1 ICX 전송하기
아래와 같이 sendicx.json 파일 작성
```json
{
    "jsonrpc": "2.0",
    "method": "icx_sendTransaction",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxb1c73381e270d401a8f8b3f969f97457cca32d6d",
        "to": "hx9d29b1e7a935de652a753bec4e664a0a1197dcbb",
        "value": "0xde0b6b3a7640000",
        "stepLimit": "0x3b9aca00",
        "nid": "0x3",
        "nonce": "0x1"
    }
}
```
작성한 json파일로 sendtx 명령 실행
```
root@f1831fff63ed:/tbears/test# tbears sendtx -k key sendicx.json
Input your keystore password:
Send transaction request successfully.
transaction hash: 0xc0080c6e4efc9f54dc8c37500c335d052e414566b927ed2b469e26b9d2aab69e
root@f1831fff63ed:/tbears/test# tbears txresult 0xc0080c6e4efc9f54dc8c37500c335d052e414566b927ed2b469e26b9d2aab69e
Transaction result: {
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0xc0080c6e4efc9f54dc8c37500c335d052e414566b927ed2b469e26b9d2aab69e",
        "blockHeight": "0x2043d",
        "blockHash": "0xda3288c14f840f27ac7344d31f46da012ce967c8fc6210e6104949721688495d",
        "txIndex": "0x0",
        "to": "hx9d29b1e7a935de652a753bec4e664a0a1197dcbb",
        "stepUsed": "0x186a0",
        "stepPrice": "0x2540be400",
        "cumulativeStepUsed": "0x186a0",
        "eventLogs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1"
    },
    "id": 1
}
```

##### SCORE의 함수 호출
아래와 같이 functioncall.json 파일 작성
```json
{
    "jsonrpc": "2.0",
    "method": "icx_sendTransaction",
    "id": 1234,
    "params": {
        "version": "0x3",
        "from": "hxb1c73381e270d401a8f8b3f969f97457cca32d6d",
        "to": "cx2726c62bab30071ced0af632233c734f5db917ad",
        "stepLimit": "0x3b9aca00",
        "nid": "0x3",
        "nonce": "0x1",
        "dataType": "call",
        "data" : {
            "method" : "hello"
        }
    }
}
```
작성한 json파일로 sendtx 명령 실행
```
root@f1831fff63ed:/tbears/test# tbears sendtx -k key functioncall.json
Input your keystore password:
Send transaction request successfully.
transaction hash: 0xe4d9a59045c9f6863704c96455103d03ec267c21716ac0f806840ebb4b95923e
root@f1831fff63ed:/tbears/test# tbears txresult 0xe4d9a59045c9f6863704c96455103d03ec267c21716ac0f806840ebb4b95923e
Transaction result: {
    "jsonrpc": "2.0",
    "result": {
        "txHash": "0xe4d9a59045c9f6863704c96455103d03ec267c21716ac0f806840ebb4b95923e",
        "blockHeight": "0x20441",
        "blockHash": "0xcde0c7961d788b7fe1540961debb53e062ed0199b799861919e41c3d81da8c69",
        "txIndex": "0x0",
        "to": "cx2726c62bab30071ced0af632233c734f5db917ad",
        "stepUsed": "0x1ec30",
        "stepPrice": "0x2540be400",
        "cumulativeStepUsed": "0x1ec30",
        "eventLogs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1"
    },
    "id": 1
}
```

### ICON SDK 사용
윈도우에선 SDK 설치가 안되기 때문에 Ubuntu_desktop_18.04에서 진행함
#### Hyper-v로 우분투 설치
##### 1. ISO 다운로드
https://www.ubuntu.com/download/desktop/thank-you?country=KR&version=18.04.2&architecture=amd64

##### 2. Hyper-V로 VM 생성
1. Hyper-V 관리자 실행
2. 새로만들기 -> 가상 컴퓨터 클릭
3. 이름 및 위치 지정
![이름 및 위치 지정](./img/hyper-v1.PNG)
4. 세대 지정
![세대 지정](./img/hyper-v2.PNG)
5. 메모리 할당
![메모리 할당](./img/hyper-v3.PNG)
6. 네트워킹 구성
![네트워킹 구성](img/hyper-v4.PNG)
7. 가상 하드 디스크 연결
![가상 하드 디스크 연결](img/hyper-v5.PNG)
8. 설치 옵션
![설치 옵션](img/hyper-v6.PNG)
9. VM 생성후 부팅설정을 UEFI로 변경
![VM 생성후 부팅설정 변경](img/hyper-v7.PNG)
10. 우분투 인스톨 실행

#### T-bears 인스톨
```bash
$ sudo apt-get update
# 18.04에서는 libleveldb1 대신 libleveldb1v5 사용
$ sudo apt-get install libleveldb1v5 libleveldb-dev
$ sudo apt-get install libsecp256k1-dev
# 문서와 달리 pkg-config 별도로 인스톨 해야함
$ sudo apt-get install pkg-config

# install RabbitMQ
$ sudo apt-get install rabbitmq-server

$ mkdir icon
$ cd icon

$ conda create --name icon python=3.6
$ conda activate icon

(icon) $ pip install tbears
```

#### ICON SDK 설치 (Python)
``` bash
(icon) $ pip install iconsdk
```

#### Pycharm
##### 설치
```bash
$ sudo snap install pycharm-community --classic
```
##### 가상환경 셋팅
pycharm을 ICON 가상환경으로 셋팅한다.
1. 파이참 기존 인터프리터 선택
![파이참 기존 인터프리터 선택](img/pycharm1.png)
2. Conda Environment 선택 후 icon 환경 선택
![Conda Environment 선택 후 icon 환경 선택](img/pycharm2.png)

#### Python SDK로 컨트랙트배포
```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from iconsdk.wallet.wallet import KeyWallet
from iconsdk.builder.transaction_builder import DeployTransactionBuilder
from iconsdk.signed_transaction import SignedTransaction
from iconsdk.libs.in_memory_zip import gen_deploy_data_content

# IconService클래스는 API를 제공한다. JSON-RPC서버와 통신하는 HTTPProvider를 인자로 받는다.
# provider는 IconService가 루프체인에 접속하는 방법을 정의.

#get IconService Instance
icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))

#get wallet instance
keystore_path = "./keystore.json"
keystore_pw = open("./password.txt", "r").readLine()
wallet = KeyWallet.load(keystore_path, keystore_pw)

# build the transaction for deployment
transaction = DeployTransactionBuilder()\
    .from_(wallet.get_address())\
    .to("cx0000000000000000000000000000000000000000") # address 0 means SCORE install\
    .step_limit(1000000)\
    .nid(3)\
    .nonce(100)\
    .content_type("application/zip")\
    .content(gen_deploy_data_content("./helloworld"))\
    .build()

#get signed transaction
signed_transaction = SignedTransaction(transcation, wallet)

#send transaction
tx_hash = icon_service.send_transaction(signed_transaction)
print(tx_hash)
```
**결과 확인**
![python_deploy1](images/2019/02/python-deploy1.png)


#### Python SDK로 컨트랙트 메서드 호출
```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from iconsdk.wallet.wallet import KeyWallet
from iconsdk.builder.call_builder import CallBuilder


hello_world_address = "cx2726c62bab30071ced0af632233c734f5db917ad"
keystore_path = "./keystore.json"
keystore_pw = open("./password.txt", "r").readLine()

#get IconService Instance
icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))

#get wallet instance
wallet = KeyWallet.load(keystore_path, keystore_pw)

# build the icx_call
call = CallBuilder()\
    .from_(wallet.get_address())\
    .to(hello_world_address)\
    .method("hello")\
    .build()

# don't need to sign when you call read-only function

result = icon_service.call(call)
print(result)
```
**결과확인**
```
Hello
```

#### Hello World 분석
```python
from iconservice import *

TAG = 'Helloworld'

class Helloworld(IconScoreBase):

    def __init__(self, db: IconScoreDatabase) -> None:
        super().__init__(db)

    def on_install(self) -> None:
        super().on_install()

    def on_update(self) -> None:
        super().on_update()

    @external(readonly=True)
    def hello(self) -> str:
        Logger.debug(f'Hello, world!', TAG)
        return "Hello"

    @external(readonly=True)
    def name(self) -> str:
        return "HelloWorld"

    @payable
    def fallback(self):
        Logger.info("fallback is called", TAG)

    @external
    def tokenFallback(self, _from: Address, _value: int, _data: bytes):
        Logger.info("tokenFallback is called", TAG)
```
##### IconScoreBase (the highest parent class)
모든 클래스는 반드시 IconScoreBase 클래스를 상속 받아야 함. 그렇지 않으면 deploy되지 않음.

##### `__init__`
초기화 메서드. 생성자와 비슷. 멤버 변수 선언가능. 컨트랙트가 각 노드에서 로드될 때 호출됨.
> 반드시 부모의 init도 호출되어야 함.
> `super().__init__(db)`

##### on_install
컨트렉트가 처음 배포 됐을 떄 한번 실행된다.
state DB 초기화를 위한 콜백

##### external decorator(@external)
@external 데코레이터가 붙으면 컨트랙트 바깥에서 호출이 가능하다.
read-only 파라미터가 붙으면 state DB에 읽기전용으로 접근한다. 즉 상태변화가 발생하지 않는다.

##### @payable
@payable 데코레이터가 있는 함수만 icx 코인을 전송받을 수 있다.

##### fallback
fallback 함수는 data없이 트랜잭션 요청을 받았을 때 호출된다. data가 없다는 것은 메서드 이름을 지정하지 않았다는 뜻이다. 즉 컨트랙트가 단순 ICX 코인만 전송 받겠다는 것이다. 그래서 fallback에 @payable이 없으면 icx코인 전송을 받을 수가 없다.
 @external 데코레이터를 적용할 수 없다.

##### tokenFallback
IRC2 표준을 준수하는 토큰을 받기 위한 메서드
IRC2 표준에선 토큰을 받기 위해 tokenFallback 메서드가 요구된다.

#### 배포한 컨트랙트에 fallback을 이용해서 icx 전송하기
```python
from iconsdk.builder.transaction_builder import TransactionBuilder
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from iconsdk.signed_transaction import SignedTransaction
from iconsdk.wallet.wallet import KeyWallet


network_id = 3 #테스트넷 아이디임.
score_address = "cx0b54a59277bfc49694e435877082cca3043ffab7";

# get IconService instance
icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))
# get wallet instance
keystore_path = "./keystore.json"
keystore_pw = open("./password.txt", "r").readline()
wallet = KeyWallet.load(keystore_path, keystore_pw)

transaction = TransactionBuilder()\
    .from_(wallet.get_address())\
    .to(score_address)\
    .step_limit(0x59682f00)\
    .value(1000000000000000000)\
    .nid(network_id)\
    .nonce(100)\
    .build()

signed_transaction = SignedTransaction(transaction, wallet)

result = icon_service.send_transaction(signed_transaction)
print(result)
```
**실행결과**
![icx 전송결과](images/2019/02/icx-전송결과.png)


#### Sample 토큰 만들기
```python
from iconservice import *

TAG = 'SampleToken'

# IRC2토큰 표준을 정의한 토큰 추상클래스
# IRC2 명세에는 아래 6개의 메서드를 만드시 정의하라고 되어있다.
class TokenStandard(ABC):
    @abstractmethod
    def name(self) -> str:
        pass

    @abstractmethod
    def symbol(self) -> str:
        pass

    @abstractmethod
    def decimals(self) -> int:
        pass

    @abstractmethod
    def totalSupply(self) -> int:
        pass

    @abstractmethod
    def balanceOf(self, _owner: Address) -> int:
        pass

    @abstractmethod
    def transfer(self, _to:Address, _value: int, _data:bytes=None):
        pass


# SCORE가 토큰을 받을 떄 이 인터페이스를 구현한 SCORE는 토콘을 받을 때의 할 작업을 정의할 수 있다.
# 일종의 콜백
class TokenFallbackInterface(InterfaceScore):
    @interface
    def tokenFallback(self, _from:Address, _value:int, _data:bytes):
        pass

# 토큰을 구현할 SCORE
class SampleToken(IconScoreBase, TokenStandard):

    @eventlog(indexed=3)
    def Transfer(self, _from:Address, _to:Address, _value:int, _data:bytes):
        pass

    #초기화 메서드 (상태 저장용 DB를 생성한다.)
    def __init__(self, db: IconScoreDatabase) -> None:
        super().__init__(db)
        self._total_supply = VarDB("total_supply", db, value_type=int)
        self._decimals = VarDB("decimals", db, value_type=int)
        self._balances = DictDB("balances", db, value_type=int)

    # 최초 SCORE 배포시 실행
    # 초기값을 받아 얼마만큼의 토큰을 발행할지 결정하고
    # 발행한 토큰을 SCORE 배포자에게 저장.
    def on_install(self, _initialSupply: int, _decimals: int) -> None:
        super().on_install()

        if _initialSupply < 0:
            revert("Initial supply cannot be less than zero")

        if _decimals < 0:
            revert("Decimals cannot be less than zero")

        total_supply = _initialSupply * 10 ** _decimals
        Logger.debug(f'on_install: _total_supply={total_supply}', TAG)

        self._total_supply.set(total_supply)
        self._decimals.set(_decimals)
        self._balances[self.msg.sender] = total_supply

    def on_update(self) -> None:
        super().on_update()

    @external(readonly=True)
    def name(self) -> str:
        return "Ratherium"

    @external(readonly=True)
    def symbol(self) -> str:
        return "RTH"

    @external(readonly=True)
    def decimals(self) -> int:
        return self._decimals.get()

    @external(readonly=True)
    def totalSupply(self) -> int:
        return self._total_supply.get()

    @external(readonly=True)
    def balanceOf(self, _owner: Address) -> int:
        return self._balances[_owner]

    @external
    def transfer(self, _to: Address, _value: int, _data: bytes = None):
        if _data is None:
            _data = b'None'
        self._transfer(self.msg.sender, _to, _value, _data)

    def _transfer(self, _from:Address, _to:Address, _value: int, _data: bytes):
        if _value < 0:
            revert("Transferring value cannot be less than zero")
        if self._balances[_from] < _value:
            revert("Out of balance")

        self._balances[_from] = self._balances[_from] - _value
        self._balances[_to] = self._balances[_to] + _value

        if _to.is_contract:
            # contract에게 토큰을 전송하는 경우
            # 해당 contract를 TokenFallbackInterface로 캐스팅 후
            # tokenFallback메서드를 호출해서 contract에게 제어권을 넘겨준다..
            recipient_score = self.create_interface_score(_to, TokenFallbackInterface)
            recipient_score.tokenFallback(_from, _value, _data)

        self.Transfer(_from, _to, _value, _data)
        Logger.debug(f'Transfer({_from}, {_to}, {_value}, {_data})')
```

##### IRC2
ICON에서 사용하는 토큰의 인터페이스 표준을 말한다.
https://github.com/icon-project/IIPs/blob/master/IIPS/iip-2.md

##### ABC, @abstractmethod
Python에서 추상클래스, 추상메서드를 정의하기 위한 방법중 하나.

##### @eventlog
트랜잭션 결과를 이벤트로그로 기록한다.
indexed 파라미터를 설정하면 지정한 파라미터 개수만큼 인덱싱되어 저장된다.
트랙커에서 이벤트로그 탭을 클릭하면 확인 가능.

##### VarDB, DictDB, ArrayDB
state DB를 랩핑한 유틸리티 클래스이다.
VarDB는 간단한 키-밸류 데이터를 저장할 수 있다.
```python
VarDB('name', db, value_type=str).set('theloop')
name = VarDB('name', db, value_type=str).get()
print(name) ## 'theloop'
```
DictDB는 DB를 딕셔너리처럼 이용할 수 있게 해준다.
```python
test_dict1 = DictDB('test_dict1', db, value_type=int)
test_dict1['key'] = 1 ## set
print(test_dict1['key']) ## get 1

print(test_dict1['nonexistence_key']) # prints 0 (key does not exist and value_type=int)
```
ArrayDB는 일차원 배열 데이터를 저장한다.
```python
test_array = ArrayDB('test_array', db, value_type=int)
test_array.put(0)
test_array.put(1)
print(len(test_array)) ## prints 2
print(test_array.pop()) ## prints 1
test_array[0] = 0 ## ok
# test_array[100] = 1 ## error
for e in test_array: ## ok
    print(e)
print(test_array[-1]) ## ok
# print(test_array[-100]) ## error
```

#### 샘플 토큰 배포
```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from iconsdk.wallet.wallet import KeyWallet
from iconsdk.builder.transaction_builder import DeployTransactionBuilder
from iconsdk.signed_transaction import SignedTransaction
from iconsdk.libs.in_memory_zip import gen_deploy_data_content

icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))
keystore_path = "./keystore.json"
keystore_pw = open("./password.txt", "r").readline()
wallet = KeyWallet.load(keystore_path, keystore_pw)

# generate instance of transaction
transaction = DeployTransactionBuilder()\
    .from_(wallet.get_address())\
    .to("cx0000000000000000000000000000000000000000")\
    .step_limit(0x59682f00)\
    .nid(3)\
    .nonce(100)\
    .content_type("application/zip")\
    .content(gen_deploy_data_content("./sample_token"))\
    .params({"_initialSupply": 500, "_decimals": 19})\
    .build()

# get signed Transaction from transaction and wallet
signed_transaction = SignedTransaction(transaction, wallet)

#send transaction
tx_hash = icon_service.send_transaction(signed_transaction)
print(tx_hash)
```
**결과확인**
![토큰배포결과확인](images/2019/02/토큰배포결과확인.png)

#### 샘플 토큰 전송
```python
from iconsdk.builder.transaction_builder import CallTransactionBuilder
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from iconsdk.signed_transaction import SignedTransaction
from iconsdk.wallet.wallet import KeyWallet


network_id = 3 #테스트넷 아이디임.
score_address = "cx852677db1e305829865f55bbdf0297713dfba82a";
# get IconService instance
icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/api/v3"))
# get wallet instance
keystore_path = "./keystore.json"
keystore_pw = open("./password.txt", "r").readline()
wallet = KeyWallet.load(keystore_path, keystore_pw)

transaction = CallTransactionBuilder()\
    .from_(wallet.get_address())\
    .to(score_address)\
    .step_limit(0x59682f00)\
    .nid(network_id)\
    .nonce(100)\
    .method("transfer")\
    .params({"_to": "hxb1c73381e270d401a8f8b3f969f97457cca32d6d", "_value": 10000000000000000000})\
    .build()

signed_transaction = SignedTransaction(transaction, wallet)

result = icon_service.send_transaction(signed_transaction)
print(result)
```

**결과확인**
![토큰전송](images/2019/02/토큰전송.png)

#### Hello world Unit Test
##### 테스트 코드
```python
import os

from iconsdk.builder.transaction_builder import DeployTransactionBuilder
from iconsdk.builder.call_builder import CallBuilder
from iconsdk.icon_service import IconService
from iconsdk.libs.in_memory_zip import gen_deploy_data_content
from iconsdk.providers.http_provider import HTTPProvider
from iconsdk.signed_transaction import SignedTransaction

from tbears.libs.icon_integrate_test import IconIntegrateTestBase, SCORE_INSTALL_ADDRESS

DIR_PATH = os.path.abspath(os.path.dirname(__file__))


class TestHelloworld(IconIntegrateTestBase):
    TEST_HTTP_ENDPOINT_URI_V3 = "http://127.0.0.1:9000/api/v3"
    SCORE_PROJECT = os.path.abspath(os.path.join(DIR_PATH, '..'))

    def setUp(self):
        super().setUp()

        self.icon_service = None
        # if you want to send request to network, uncomment next line and set self.TEST_HTTP_ENDPOINT_URI_V3
        # self.icon_service = IconService(HTTPProvider(self.TEST_HTTP_ENDPOINT_URI_V3))

        # install SCORE
        self._score_address = self._deploy_score()['scoreAddress']

    def _deploy_score(self, to: str = SCORE_INSTALL_ADDRESS) -> dict:
        # Generates an instance of transaction for deploying SCORE.
        transaction = DeployTransactionBuilder() \
            .from_(self._test1.get_address()) \
            .to(to) \
            .step_limit(100_000_000_000) \
            .nid(3) \
            .nonce(100) \
            .content_type("application/zip") \
            .content(gen_deploy_data_content(self.SCORE_PROJECT)) \
            .build()

        # Returns the signed transaction object having a signature
        signed_transaction = SignedTransaction(transaction, self._test1)

        # process the transaction in local
        tx_result = self.process_transaction(signed_transaction, self.icon_service)

        self.assertTrue('status' in tx_result)
        self.assertEqual(1, tx_result['status'])
        self.assertTrue('scoreAddress' in tx_result)

        return tx_result

    def test_score_update(self):
        # update SCORE
        tx_result = self._deploy_score(self._score_address)

        self.assertEqual(self._score_address, tx_result['scoreAddress'])

    def test_call_hello(self):
        # Generates a call instance using the CallBuilder
        call = CallBuilder().from_(self._test1.get_address()) \
            .to(self._score_address) \
            .method("hello") \
            .build()

        # Sends the call request
        response = self.process_call(call, self.icon_service)

        self.assertEqual("Hello", response)
```

##### 테스트 실행
```bash
# with T-Bears command
$ tbears test helloworld

# with python
$ python -m unittest discover helloworld
```

##### 코드 분석
###### IconIntegrateTestBase
SCORE 테스트 클래스는 반드시 IconIntegrateTestBase을 상속받아야 한다.
IconIntegrateTestBase 클래스는 아래의 기능을 제공한다.
1. python 단위테스트
    - test_를 접두어로 붙여 테스트메서드를 작성
    - setUp, tearDown 메서드를 오버라이드해서 테스트를 초기화 및 자원반납을 할 수 있다.
2. ICON 서비스 테스트 환경구성
    - 테스트 환경을 초기화하고, 제네시스블록을 셋팅
    - 테스트용 account 제공
        - self._test1 : 코인을 많이 갖고 있는 테스트계정
        - self._wallet_array[]: 테스트용 계정 배열 (기본으로 10개 제공)
3. 통합 테스트용 API 제공
    - self.process_transaction() : 트랜잭션을 실행하고 결과 반환
    - self.process_call() : SCORE의 read-only 전역 메서드를 호출하고 결과반환

###### self.icon_service
self.icon_service가 None이면 Mock환경을 구성하여 단위테스트로 수행
self.icon_service에 IconService를 할당하면 실제 테스트넷이나 로컬테스트넷에 접속하여 통합테스트 수행
