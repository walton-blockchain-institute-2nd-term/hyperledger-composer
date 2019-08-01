# hyperledger-composer

### __Linux Ubuntu 환경에서 진행합니다.__

__[Hyperledger Composer 공식 링크](https://hyperledger.github.io/composer/latest/)__

<br>

## __하이퍼레저 컴포즈 사전 필수 요구사항 설치__

[hyperledger.github.io 사전 필수 요구사항 설치](https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html#ubuntu)

```
// 기본 ~ 경로에서 하시면 될 것 같아요.. . . ...

// curl을 사용해 prereqs-ubuntu.sh 다운로드
curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh

// 소유자에게 실행 권한 추가
chmod u+x prereqs-ubuntu.sh

// 다운받은 쉘 스크립트 실행
./prereqs-ubuntu.sh
```

<br>

## __구성 요소 설치__

사전 필수 요구사항이 설치된 후 진행해주세요.

[hyperledger.github.io 구성 요소 설치 및 개발 환경 제어](https://hyperledger.github.io/composer/latest/installing/development-tools.html) 를 이용하셔도 되고 위의 링크에서 계속 다음으로 넘어가셔도 됩니다.

<br>

> ### __1단계 : CLI 도구 설치__

__su나 sudo 명령을 사용하지 말아주세요!__

```
// global로 CLI 도구 설치
npm install -g composer-cli@0.20

npm install -g composer-rest-server@0.20

npm install -g generator-hyperledger-composer@0.20

npm install -g yo
```

<br>

> ### __2단계 : Playground 설치__

```
// global로 Playground 설치
npm install -g composer-playground@0.20
```

<br>

> ### __3단계 : IDE 설정__

[vscode 다운로드](https://code.visualstudio.com/download)

(선택) 설치 후 확장 프로그램에서 `Hyperledger Composer`를 다운받아주세요.

<br>

> ### __4단계 : Hyperledger 패브릭 설치__

```
// fabric-dev-servers 폴더 생성 및 이동
mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers

// fabric-dev-servers.tar.gz 파일 다운로드
curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz

// 압축 풀기
tar -xvf fabric-dev-servers.tar.gz
```

```
// ~/fabric-dev-servers 경로에서 진행
// 환경변수 설정 후 쉘 스크립트 실행
export FABRIC_VERSION=hlfv12
./downloadFabric.sh
```

<br>

## __개발 환경 제어__

```
// ~/fabric-dev-servers 경로에서 진행
// export FABRIC_VERSION=hlfv12

./startFabric.sh

// peerAdmin 카드 생성
./createPeerAdminCard.sh
```

```
// 웹 앱 실행
// localhost:8080/login이 자동으로 실행됩니다.
composer-playground
```
<br>

여기까지 잘 진행되셨다면 성공~

<br>

> ### __부록 : 이전 설정 파괴__

```
docker kill $(docker ps -q)
docker rm $(docker ps -aq)
docker rmi $(docker images dev-* -q)
```

<br>

<hr>

<br>

## __Playground 튜토리얼__

[hyperledger.github.io playground 튜토리얼](https://hyperledger.github.io/composer/latest/tutorials/playground-tutorial.html) 을 참고하시면 됩니다.

### 강의는 로컬 환경(localhost:8080/login)이 아닌 __[온라인 hyperledger playground](https://composer-playground.mybluemix.net/)__ 에서 진행되었습니다.

<br>

> ### __1단계 : Hyperledger Composer Playground 열기__

1. 위의 [온라인 hyperledger playground 링크](https://composer-playground.mybluemix.net/) 로 접속해주세요.

<br>

> ### __2단계 : 새로운 비즈니스 네트워크 만들기__

1. `Deploy a new business network`을 선택합니다.

2. `Basic Information`의 `Give your new Business Network a name` 탭에 이름을 입력해주세요. (강의는 `tutorial-blockchain` 이라는 이름으로 진행되었습니다.)

3. 새 비즈니스에 대한 설명을 추가할 수 있으나 강의에서는 생략했습니다.

4. 처음부터 네트워크를 구축하기 때문에 `empty-business-network`를 선택해주세요.

5. `Deploy` 하시면 새 네트워크가 추가된 것을 확인할 수 있습니다. (배포)

<br>

> ### __3단계 : 비즈니스 네트워크에 연결__

__admin@tutorial-blockchain이라는 새 비즈니스 네트워크 카드가 생성된 것을 확인할 수 있습니다.__

1. 생성된 새 비즈니스 네트워크 카드 밑의 `Connect now`를 선택합니다.

<br>

> ### __4단계 : 모델 파일 추가__

__모델 파일은 비즈니스 네트워크에서 자산(asset), 참가자(participant), 트랜잭션(transaction) 및 이벤트를 정의합니다.__

1. `Define`의 `Files`에서 `Model File`을 선택해주세요.

2. 아래의 코드를 복붙해주세요.

```
/**
 * My commodity trading network
 */
namespace org.example.mynetwork

asset Commodity identified by tradingSymbol {
    o String tradingSymbol
    o String description
    o String mainExchange
    o Double quantity
    --> Trader owner
}
participant Trader identified by tradeId {
    o String tradeId
    o String firstName
    o String lastName
}
transaction Trade {
    --> Commodity commodity
    --> Trader newOwner
}
```

3. `Deploy Changes`로 `models/model.cto`의 변경사항을 업데이트합니다.

<br>

> ### __5단계 : 트랜잭션 프로세서 스크립트 파일 추가__

__도메인 모델이 정의되었으므로 비즈니스 네트워크에 대한 트랜잭션을 정의할 수 있습니다.__

__정의된 함수는 트랜잭션이 `submit`될 때 자동으로 실행됩니다.__

1. `Add a file...`을 선택해주세요.

2. `Script File(.js)`를 선택합니다.

3. 아래의 코드를 복붙해주세요.

``` js
/**
 * Track the trade of a commodity from one trader to another
 * @param {org.example.mynetwork.Trade} trade - the trade to be processed
 * @transaction
 */
async function tradeCommodity(trade) {
    // 매개변수로 들어온 트랜잭션의 newOwner를 commodity의 owner로 설정
    trade.commodity.owner = trade.newOwner;
    // 수정된 내용을 저장
    let assetRegistry = await getAssetRegistry('org.example.mynetwork.Commodity');
    await assetRegistry.update(trade.commodity);
}
```

4. `Deploy Changes`로 `lib/script.js` 파일을 업데이트 해주세요.

<br>

> ### __6단계 : 엑세스 제어__

<br>

> ### __7단계 : 업데이트 된 비즈니스 네트워크 배포__

<br>

> ### __8단계 : 비즈니스 네트워크 정의 테스트__

<br>

> ### __9단계 : 참가자 생성__

<br>

> ### __10단계 : 저작물 만들기__

<br>

> ### __11단계 : 참가자 간 상품 이전__

<br>

<hr>

<br>

## __Hyperledger 개발자 자습서__

[hyperledger.github.io hyperledger 개발자 자습서](https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial)를 참고하세요.

### __Linux Ubuntu 환경에서 진행해주세요.__

__로컬 환경에서 네트워크를 구축하는 실습입니다.__

<br>

> ### __1단계 : 비즈니스 네트워크 구조 만들기__

```
// ~ 경로에서 진행해주세요.

// Yeoman(yo)을 사용해서 네트워크 구축
// Yeoman이란 프로젝트에 필요한 디렉토리 및 파일을 만들어주는 커맨드라인 인터페이스
yo hyperledger-composer:businessnetwork
```

<br>

> ### __2단계 : 비즈니스 네트워크 정의__

<br>

> ### __3단계 : 비즈니스 네트워크 아카이브 생성__

<br>

> ### __4단계 : 비즈니스 네트워크 배포__

<br>

> ### __5단계 : REST 서버 생성__

<br>

> ### __6단계 : 골격 생성 각도 응용 프로그램__