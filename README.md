# hyperledger-composer

### __Linux Ubuntu 환경에서 진행합니다.__

__[Hyperledger Composer 공식 링크](https://hyperledger.github.io/composer/latest/)__

<br>

## __하이퍼레저 컴포즈 사전 필수 요구사항 설치__

[hyperledger.github.io 사전 필수 요구사항 설치](https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html#ubuntu)

```
// curl을 사용해 prereqs-ubuntu.sh 다운로드
// 기본 ~ 경로에서 하시면 될 것 같아요.. . . ...
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

## __Playground 듀토리얼__

[hyperledger.github.io playground 듀토리얼](https://hyperledger.github.io/composer/latest/tutorials/playground-tutorial.html) 을 참고하시면 됩니다.

### 강의는 로컬 환경(localhost:8080/login)이 아닌 __[온라인 hyperledger playground](https://composer-playground.mybluemix.net/)__ 에서 진행되었습니다.

<br>

> ### __1단계 : Hyperledger Composer Playground 열기__

위의 [온라인 hyperledger playground 링크](https://composer-playground.mybluemix.net/) 로 접속해주세요.

<br>

> ### __2단계 : 새로운 비즈니스 네트워크 만들기__