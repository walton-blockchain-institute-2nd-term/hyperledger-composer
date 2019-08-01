# hyperledger-composer

### __Linux Ubuntu 환경에서 진행합니다.__

__[Hyperledger Composer 공식 링크](https://hyperledger.github.io/composer/latest/)__

<br>

## __하이퍼레저 컴포즈 사전 필수 요구사항 설치__

[hyperledger.github.io 사전 필수 요구사항 설치](https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html#ubuntu)

```
// curl을 사용해 prereqs-ubuntu.sh 다운로드
curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh

// 소유자에게 실행 권한 추가
chmod u+x prereqs-ubuntu.sh

// 다운받은 쉘 스크립트 실행
// 실행 중 sudo를 사용
./prereqs-ubuntu.sh
```

<br>

## __구성 요소 설치__

사전 필수 요구사항이 설치된 후 진행해주세요.

[hyperledger.github.io 구성 요소 설치](https://hyperledger.github.io/composer/latest/installing/development-tools.html) 를 이용하셔도 되고 위의 링크에서 계속 다음으로 넘어가셔도 됩니다.

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

