# Chapter 9. 자산의 토큰화

🔸 **토큰화(tokenize)** : 자산을 마치 법정 화폐나 암호 화폐처럼 송신, 거래, 교환, 규제, 관리할 수 있도록 디지털 단위로 나타내는 것

* 6~8장 -> 종단 간 탈중앙화 애플리케이션 개발에 초점을 맞춤
* 이번 장-> 토큰 개념의 소개와 함께 블록체인 기술의 광범위한 영향력 탐구

서로 다른 토큰 애플리케이션들 간의 경계 없는 상호작용을 위해서 토큰을 표준에 맞추어 개발해야 함.

이더리움은 **이더리움 개선 제안(Ethereum Imporovement Proposal, EIP)** 이라는 과정을 통해 이러한 표준을 마련.

----------------------------------------------
## 9.1 이더리움 표준

비트코인과 스마트 컨트랙트의 출현 이후 많은 독립적인 코인과 토큰이 등장했다. 이러한 확장은 토큰에 관해 많은 이슈와 의문들을 제기했다.

투자자를 사기 상품이나 투자로부터 보호하기 위해 암호 화폐 산업을 규제하려는 목적에 기반하여 이더리움 커뮤니티는 지속적인 개발, 토론, 표준의 도입을 통해 이러한 이슈들을 해결해 왔다.

### 9.1.1 이더리움 개선 제안

이더리움 표준은 EIP 체계를 바탕으로 이루어진다. EIP는 프로토콜 명세, 개선 사항, 업데이트, 클라이언트 API와 컨트랙트 표준 등을 관리하기 위한 수단이다.

EIP는 다음과 같은 서로 다른 카테고리의 이슈를 다룬다.
* 코어 - 코어 이더리움 프로토콜
* 네트워크 - 네트워크 레벨 개선
* 인터페이스 - ABI, RPC 같은 인터페이스
* ERC(Ethereum Request for Comments) - 애플리케이션 레벨 규약 및 표준

### 9.1.2 ERC20 토큰 표준

![ERC20 토큰 표준](https://github.com/TwoPair/Blockchain_Study/assets/39588815/e621bc3c-80d8-42f7-8769-88182732223a)

책에서 보여준 ERC20 인터페이스 정의에서 달라진 점들이 있다. 좀 더 구체적인 정보를 확인해보고 싶다면 다음 링크에서 확인하면 된다.

[ERC20 표준 링크](https://eips.ethereum.org/EIPS/eip-20#methods)

ERC20 표준과 호한하는 자산 또는 유틸리티를 위한 토큰을 만들고 배포하려면, ERC20 인터페이스가 요구하는 함수들을 스마트 컨트랙트에 구현해야 한다.

마치 Java에서의 Interface와 같은 역할을 하는데 이더리움에 배포하고 싶다면 꼭 지켜야 하는 표준이다.

> 🔰 집필 시점 이후에 ERC20를 대체하기 위한, 개선 버전의 대체 가능 토큰인 ERC777이 등장했다. 자세히 알고 싶다면 [여기](https://eips.ethereum.org/EIPS/eip-777)를 살펴보길 바란다.

### 9.1.3 대체 가능 토큰과 대체 불가능 토큰

ERC20 토큰은 어떤 유틸리티나 서비스를 위해 토큰을 지급해서 구매할 수 있는 화폐 같은 역할을 한다.

여기서 하나의 ERC20 호환 토큰은 같은 종류의 다른 토큰과 교환 가능하기 때문에 **대체 가능 토큰(Fungible Token, FT)** 이라고 부른다.

반대로 야구 선수나 부동산 토큰과 같이 여러 가지 요인에 의해 각각의 토큰의 가치가 크게 오를 수도 있고 떨어질 수도 있다. 이러한 종류의 토큰을 **대체 불가능 토큰(NFT)** 이라고 한다.

![그림9.2](https://github.com/TwoPair/Blockchain_Study/assets/39588815/ed62d612-4287-45ea-889f-61217c4cc94e)

정리하자면, ERC20은 대체 가능한 토큰(FT)이고, ERC721은 대체 불가능 토큰(NFT)을 위한 표준이다.

ERC721 토큰 모델은 더 넓은 범위의 대체 불가능한 자산에 적용할 수 있다. 주식과 부동산, 예술 수집품에 이르기까지 많은 유스 케이스가 있다. 9.2절에서 부동산 자산을 나타내는 이러한 ERC721 토큰에 관해 탐구해 볼 것이다.

----------------------------------------------
## 9.2 RES4: 대체 불가능한 부동산 토큰

부동산을 대체 불가능한 자산이라고 가정하고, 이를 위한 토큰 Dapp을 설계하고 개발해보자.

우선 문제 설정을 하고, 설계 원칙(부록 B)을 적용해 애플리케이션을 설계하고 개발할 것이다. 이 부동산 토큰을 RES4라고 부르기로 하자.

![문제설정](https://github.com/TwoPair/Blockchain_Study/assets/39588815/863e7361-6382-48a7-9922-fa5155c34cd9)

### 9.2.1 유스 케이스 다이어그램

설계 원칙 2. 유스 케이스 다이어그램을 그려보자. 그림 9.3은 다음과 같은 RES4의 액터를 가지고 있다.

* 마을 관리자(자산 개발자 또는 생성자)
* 자산 소유자
* 자산 건축가(자산에 가치를 더함)
* 자산 구매자
* 자산 감정평가사

핵심적인 오퍼레이션을 자산 추가, 건축하기, 구매자 허가, 구매와 소유권 이전, 자산 평가 등의 유스 케이스로 표현할 수 있다.

![그림9.3](https://github.com/TwoPair/Blockchain_Study/assets/39588815/e31fde34-803f-4b29-b1ab-ee2a0b47920b)

### 9.2.2 컨트랙트 다이어그램

컨트랙트 다이어그램은 유스 케이스 다이어그램을 바탕으로 여기에 데이터 구조, 수정자, 이벤트, 함수 등의 설계 요소를 추가해서 확장한 것이다. 엑세스 규칙은 함수 안에 require 문을 사용해 규정하였다. RES4는 지정된 함수 헤더를 가진 ERC721 표준을 따른다.

![그림9.4](https://github.com/TwoPair/Blockchain_Study/assets/39588815/5ff0b458-7b87-43b7-be4a-754c5f4b57b7)

### 9.2.3 ERC721 호환 RES4 토큰

#### ERC721 토큰 표준

각 ERC721 토큰은 고유. 이 토큰 표준의 요구 사항 중 하나는 토큰 공급이 한정된다는 것. 부동산 자산의 경우에는 토큰 공급의 한정이 문제되지 않는다. 모든 부동산 자산은 그 수가 한정되어 있기 때문이다.

RES4 개발에 사용한 ERC721 인터페이스 함수들은 다음 링크에서 확인할 수 있다. [링크](https://ethereum.org/ko/developers/docs/standards/tokens/erc-721/)

#### RES4 스마트 컨트랙트

유스 케이스 다이어그램과 컨트랙트 다이어그램을 가이드라인으로 사용하여 RES4를 위한 스마트 컨트랙트를 개발해보자. 그림 9.5는 이 Dapp을 위한 블록 다이어그램을 보여준다.

![그림9.5](https://github.com/TwoPair/Blockchain_Study/assets/39588815/b17cf488-03e4-4630-b57a-8894179eecf3)
