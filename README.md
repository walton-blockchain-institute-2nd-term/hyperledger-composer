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

// tradingSymbol을 기본키로 하는 Commodity 정의
asset Commodity identified by tradingSymbol {
    o String tradingSymbol
    o String description
    o String mainExchange
    // Trader의 기본키(tradeId)와 연결
    // 여기서는 owner라는 이름으로 연결....
    o Double quantity
    --> Trader owner
}

// tradeId를 기본키로 하는 Trader 정의
participant Trader identified by tradeId {
    o String tradeId
    o String firstName
    o String lastName
}

// 트랜잭션 정의
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
    // 수정된 Commodity를 자산 레지스트리에 저장
    let assetRegistry = await getAssetRegistry('org.example.mynetwork.Commodity');
    await assetRegistry.update(trade.commodity);
}
```

4. `Deploy Changes`로 `lib/script.js` 파일을 업데이트 해주세요.

<br>

> ### __6단계 : 엑세스 제어__

__엑세스 제어 파일은 비즈니스 네트워크에 대한 엑세스 제어 규칙을 정의합니다.__

__기본적인 엑세스 제어 파일은 `networkAdmin`에게 비즈니스 네트워크 및 시스템 수준 작업에 대한 모든 엑세스 권한을 제공합니다.__

__비즈니스 네트워크에서 모델 파일이나 스크립트 파일의 경우에는 여러개가 있을 수 있지만, 엑세스 제어 파일의 경우에는 하나만 있어야합니다.__

### __현재 실습에서는 간단한 비즈니스 네트워크를 사용하므로 엑세스 제어 파일을 편집하지 않습니다.__

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

1. 네트워크 이름(Business network name)은 `carauction-network`으로 지정해주세요.

2. 설명(Description)은 자유롭게 입력하셔도 됩니다. (강의는 `carauction` 으로 진행하였습니다.)

3. 개발자(Author name)와 이메일(Author email)도 자유롭게 입력하시면 됩니다.

4. License는 `Apache-2.0`으로 작성해주세요.

5. Namespace는 `org.example.mynetwork`로 작성해주세요.

6. 기존에 존재하는 템플릿을 사용할 예정이기 때문에 `Yes`를 선택해주세요.

<br>

에러 없이 진행되셨다면 완료~

<br>

> ### __2단계 : 비즈니스 네트워크 정의__

__모델, 트랜잭션 로직, 엑세스 제어를 정의합니다.__

1. 모델 파일 수정

```
// ~/carauction-network 경로에서 진행합니다.

vi models/org.example.mynetwork.cto
```

아래의 코드로 수정해주세요!

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

2. 트랜잭션 로직 파일 수정

```
vi lib/logic.js
```

아래의 코드로 수정해주세요!

``` js
/*
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/* global getAssetRegistry getParticipantRegistry */

/**
 * Close the bidding for a vehicle listing and choose the
 * highest bid that is over the asking price
 * @param {org.acme.vehicle.auction.CloseBidding} closeBidding - the closeBidding transaction
 * @transaction
 */
async function closeBidding(closeBidding) {  // eslint-disable-line no-unused-vars
    const listing = closeBidding.listing;
    if (listing.state !== 'FOR_SALE') {
        throw new Error('Listing is not FOR SALE');
    }
    // by default we mark the listing as RESERVE_NOT_MET
    listing.state = 'RESERVE_NOT_MET';
    let highestOffer = null;
    let buyer = null;
    let seller = null;
    if (listing.offers && listing.offers.length > 0) {
        // sort the bids by bidPrice
        listing.offers.sort(function(a, b) {
            return (b.bidPrice - a.bidPrice);
        });
        highestOffer = listing.offers[0];
        if (highestOffer.bidPrice >= listing.reservePrice) {
            // mark the listing as SOLD
            listing.state = 'SOLD';
            buyer = highestOffer.member;
            seller = listing.vehicle.owner;
            // update the balance of the seller
            console.log('#### seller balance before: ' + seller.balance);
            seller.balance += highestOffer.bidPrice;
            console.log('#### seller balance after: ' + seller.balance);
            // update the balance of the buyer
            console.log('#### buyer balance before: ' + buyer.balance);
            buyer.balance -= highestOffer.bidPrice;
            console.log('#### buyer balance after: ' + buyer.balance);
            // transfer the vehicle to the buyer
            listing.vehicle.owner = buyer;
            // clear the offers
            listing.offers = null;
        }
    }

    if (highestOffer) {
        // save the vehicle
        const vehicleRegistry = await getAssetRegistry('org.acme.vehicle.auction.Vehicle');
        await vehicleRegistry.update(listing.vehicle);
    }

    // save the vehicle listing
    const vehicleListingRegistry = await getAssetRegistry('org.acme.vehicle.auction.VehicleListing');
    await vehicleListingRegistry.update(listing);

    if (listing.state === 'SOLD') {
        // save the buyer
        const userRegistry = await getParticipantRegistry('org.acme.vehicle.auction.Member');
        await userRegistry.updateAll([buyer, seller]);
    }
}

/**
 * Make an Offer for a VehicleListing
 * @param {org.acme.vehicle.auction.Offer} offer - the offer
 * @transaction
 */
async function makeOffer(offer) {  // eslint-disable-line no-unused-vars
    let listing = offer.listing;
    if (listing.state !== 'FOR_SALE') {
        throw new Error('Listing is not FOR SALE');
    }
    if (!listing.offers) {
        listing.offers = [];
    }
    listing.offers.push(offer);

    // save the vehicle listing
    const vehicleListingRegistry = await getAssetRegistry('org.acme.vehicle.auction.VehicleListing');
    await vehicleListingRegistry.update(listing);
}
```

3. 엑세스 제어 파일 수정

```
vi permissions.acl
```

마찬가지로 아래의 코드로 수정해주시면 됩니다.

```
/*
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * Access Control List for the auction network.
 */
rule Auctioneer {
    description: "Allow the auctioneer full access"
    participant: "org.acme.vehicle.auction.Auctioneer"
    operation: ALL
    resource: "org.acme.vehicle.auction.*"
    action: ALLOW
}

rule Member {
    description: "Allow the member read access"
    participant: "org.acme.vehicle.auction.Member"
    operation: READ
    resource: "org.acme.vehicle.auction.*"
    action: ALLOW
}

rule VehicleOwner {
    description: "Allow the owner of a vehicle total access"
    participant(m): "org.acme.vehicle.auction.Member"
    operation: ALL
    resource(v): "org.acme.vehicle.auction.Vehicle"
    condition: (v.owner.getIdentifier() == m.getIdentifier())
    action: ALLOW
}

rule VehicleListingOwner {
    description: "Allow the owner of a vehicle total access to their vehicle listing"
    participant(m): "org.acme.vehicle.auction.Member"
    operation: ALL
    resource(v): "org.acme.vehicle.auction.VehicleListing"
    condition: (v.vehicle.owner.getIdentifier() == m.getIdentifier())
    action: ALLOW
}

rule SystemACL {
    description:  "System ACL to permit all access"
    participant: "org.hyperledger.composer.system.Participant"
    operation: ALL
    resource: "org.hyperledger.composer.system.**"
    action: ALLOW
}

rule NetworkAdminUser {
    description: "Grant business network administrators full access to user resources"
    participant: "org.hyperledger.composer.system.NetworkAdmin"
    operation: ALL
    resource: "**"
    action: ALLOW
}

rule NetworkAdminSystem {
    description: "Grant business network administrators full access to system resources"
    participant: "org.hyperledger.composer.system.NetworkAdmin"
    operation: ALL
    resource: "org.hyperledger.composer.system.**"
    action: ALLOW
}
```

<br>

> ### __3단계 : 비즈니스 네트워크 아카이브 생성__

__비즈니스 네트워크가 정의되었으니 business network archive(.bna) 파일을 생성합니다.__

```
composer archive create -t dir -n .
```

에러 없이 실행되었다면 `carauction-network@0.0.1.bna` 파일이 생성된 것을 확인하실 수 있습니다!

<br>

> ### __4단계 : 비즈니스 네트워크 배포__

1. 비즈니스 네트워크를 PeerAdmin이라는 피어에 설치합니다.

```
composer network install --card PeerAdmin@hlfv1 --archiveFile carauction-network@0.0.1.bna
```

2. 설치가 완료되었다면 비즈니스 네트워크를 시작합니다.

```
composer network start --networkName carauction-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card
```

3. 네트워크 관리자 ID를 네트워크로 가져옵니다.

```
composer card import --file networkadmin.card
```

4. 네트워크에 ping 날리기 (생략 가능)

```
composer network ping --card admin@tutorial-network
```

<br>

> ### __5단계 : REST 서버 생성__

```
composer-rest-server
```

<br>

> ### __6단계 : 골격 생성 각도 응용 프로그램__