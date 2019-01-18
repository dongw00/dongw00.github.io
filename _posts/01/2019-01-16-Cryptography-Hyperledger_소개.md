---
layout: post
title: '하이퍼레저(Hyperledger) 소개 - (1)'
subtitle: 'Hyperledger 개요'
date: '2019-01-16'
background: '/img/tags/RxSwift-bg.jpg'
categories: Hyperledger
comments: true
---

# Hyperledger 소개 - (1)

그동안 블록체인을 공부하면서 비트코인, 이더리움과 같은 퍼블릭 블록체인을 위주로 공부를 해왔다. 그러던중 IBM에서 진행하는 세미나에 참석하게 되면서 하이퍼레저에 입문하게되었다.

퍼블릭 블록체인 공부와 암호화폐에 투자를 하면서 성격이 다른 Private 블록체인은 블록체인 정신에 위배된다고 생각은 해왔지만 기존 블록체인은 신기술이고 기존 산업에 적용하기에는 여러가지 문제점들이 있었다. 

삼성전자에서는 IoT간 Ethereum으로 연결하는 방식을 개념증명(PoC)한 적이 있었다. 하지만 이더리움은 새로운 블록이 생성되는데 평균 13초가 걸리는 등 처리속도에 문제점이 있었다.

Private 블록체인은 기존 법 규제나 회사 입장에서도 블록체인이 필요한 분야에 적용하기에는 Public보다는 효율적이라는 판단하에 Private 블록체인 중 한가지인 Hyperledger을 공부하게 되었다.

들어가기에 앞서서 Hyperledger를 접하면서 느꼈던 궁금증과 블록체인에 대해서 어느정도 접한 경험은 있지만 Hyperledger에 대해서는 처음접할때 어떤 것 위주로 보면 좋을지 고민하면서 포스팅해보았다.

<br />

## Hyperledger 등장

어느 책이든 처음 시작은 어떻게 등장을 하게됬고에 대한 역사부터 시작하게된다. 이 포스팅 역시 그 순서부터 진행한다.

![](/img/01/01-24.png)

Hyperledger Project는 2015년 `Linux Foundation`에서 오픈소스 블록체인 프로젝트로 시작되었다. Project에는 다양한 프레임워크, 툴 등이 포함되어 있는데 그 중에 `Hyperledger Fabric`에 대해 이후 포스팅 부터 중점적으로 소개할 것이다.

IBM은 Hyperledger Project에 44,000줄의 블록체인 코드를 Hyperledger Fabric에 기부를 하면서 프로젝트를 진행시켰다.

IBM이외에도 Cisco, American Express, 화웨이, 삼성SDS등 다른 기업들도 참여하고 있다.

![](/img/01/01-25.png)

위 그림은 Hyperledger에 포함되어 있는 Framework와 Tool의 목록이다.

**Hyperledger Framework**는 합의 알고리즘, Smart Contract, 분산원장 등 블록체인에 대한 원천 기술을 개발하는 프로젝트이다.
- `Hyperledger Burrow` : BFT(Byzantine Fault Tolerance) 합의 알고리즘 기반의 블록체인 플랫폼인데 이더리움 기반 Dapp을 작동시키게 하는 플랫폼이다.
- `Hyperledger Fabric` : Hyperledger Project에서 가장 활발하게 진행되고 있는 프로젝트이다.
- `Hyperledger Indy` : 인터넷 환경에서 중개자 없이 신원확인을 제공하는 것을 목표로하는 프로젝트이다.
- `Hyperledger Iroha` : iOS, Android, javaScrpt 등 모바일 앱, 웹을 위한 인프라를 제공하는 프로젝트이다.

다음은 **Hyperledger Tool**이다. 블록체인 시스템의 성능 측정, 운영, 개발을 쉽게 할 수 있도록 도와주는 역할을 한다.
- `Hyperledger Caliper` : 블록체인 성능 측정을 위한 프로젝트이다.
- `Hyperledger Cello` : 운영 및 관리를 위한 프로젝트이고 플랫폼의 생명주기를 관리하고 대시보드를 통해 시스템 상태 확인과 자원 확장 기능을 제공한다.
- `Hyperledger Composer` : Hyperledger Fabric의 App 개발과 기존 비즈니스 시스템, 블록체인 시스템의 통합에 편의성을 제공해 주는 프로젝트이다.
- `Hyperledger Explorer` : 모니터링 툴이다.
- `Hyperledger Quilt` : 서로 다른 원장 간 연동 프로토콜을 개발하는 프로젝트이다.

<br />

## 기존 암호화폐의 블록체인과 다른점

![](/img/01/01-26.png)

> IBM Hyperledger 소개

사실 Private인 Hyperledger과 비트코인, 이더리움을 비교하기에는 억지스러움이 없지않아 있다. Public과 Private라는 큰 다른점도 있고 암호화폐를 사용하지 않는 하이퍼레저는 목적 자체가 다르기 때문에 비교하는 것은 억지스럽지만 이 표를 본다면 이해가 확실히 될 것같아서 소개해보았다.

<br />

## Hyperledger Fabric의 특징

먼저 Hyperledger Fabric의 특징을 4가지로 뽑을 수 있다.
- 허가형 블록체인 (Permissioned BlockChain)
- 일반 프로그래밍 언어 사용
- 암호화폐 X
- 높은 성능

1. **허가형 블록체인** <br />
    Hyperledger Fabric에서는 `멤버십 관리 서비스`(MSP)를 통해 허가된 참여자의 접근을 허용한다. Public 블록체인인 이더리움 경우 네트워크 망에 누가 참여를 하던 접근제한 권한을 가지고 있지 않다.

2. **일반 프로그래밍 언어 사용** <br />
    Hyperledger Fabric에서 이더리움에서 Smart Contract라고 부르는 것을 `체인코드`라고 부른다. 그리고 이더리움에서 Smart Contract를 작동시키기 위해 Solidity로 작성된 프로그램을 만들어야 했지만 Hyperledger Fabric은 Go, Java, Node.js같은 일반 프로그래밍 언어를 사용할 수 있다.

3. **암호화폐 X** <br />
    암호화폐가 존재하는 이유가 블록체인의 유효성을 검증해 준 노드들에게 대가로 지불할 매개체가 필요하기 때문이다. 그러나 Private 블록체인에서 불특정 다수가 아닌 필요에 의해 맺어진 관계이기 때문에 합의과정에 기여를 한 피어들에게 보상을 해주지 않아도 된다.

    이더리움에서는 이러한 이유 말고도 악의적인 공격자가 네트워크를 교란시키기 위해 **무의미한 Smart Contract를 발생시킨다면 네트워크에 큰 문제점이 발생**을 한다. 이러한 문제점을 해결하기 위해 실행시키는 코드에 Gas라는 단위로 수수료를 매겨 이러한 공격을 방지하였다. Hyperledger도 비슷한 공격을 받기 쉬운데 **Endorsing Peer(보증피어)가 보증정책을 통해 체인코드 실행 포기 시점을 결정**할 수 있기 때문에 **무한한 체인코드 실행을 방지** 할 수 있다.

4. **높은 성능** <br />
    이 부분에 대해서는 뒤에 자세히 설명한다. Public Block Chain과는 다른 메카니즘으로 트랜잭션을 블록에 담기때문에 성능상 차이가 존재한다. 또한, 네트워크에서 합의해야 하는 노드의 숫자부터 너무나도 다르기때문에 당연한 말이기도 하다.

<br />

## Hyperledger Fabric의 구조

이번 포스팅에서는 간략하게 소개하고 다음 포스팅에서 자세히 소개한다.

![](/img/01/01-27.png)

> 박승철 교수님 Hyperledger 강의

Hyperledger Fabric에서 크게 4가지 요소를 가지고 있다.

- 클라이언트 노드
- 피어(Peer)
- 멤버십 서비스 제공자(MSP)
- 순서화 서비스 노드(Ordering Service)

1. **클라이언트 노드** <br />
    dApp이라고 생각하면된다. 사용자를 대신해서 Transaction을 생성해서 보증피어(Endorsing Peer)에게 `거래 보증을 요청`을 한다.
    그리고 Transaction 보증 응답을 수집한 후 `순서화 서비스 노드에 전달`한다.

2. **피어(Peer)**<br />
    피어의 종류에는 2가지가 있다. 
    - Endorsing Peer : Client에서 받은 Transaction을 검증한다.
    - Committing Peer
  
3. **멤버십 서비스 제공자(MSP)**<br />
    Hyperledger Fabric에서 노드들의 역할, 소속, 권한등을 설정할 수 있다.

4. **순서화 서비스 노드(Ordering Service)**<br />
    Client에게 보증절차를 거친 Transaction을 받은 후 정해진 합의 알고리즘에 의해 Transaction이 `순서화`가 되고 새로운 블록에 담겨 `Committing Peer에게 전송`한다.

<br />

**[참고자료]**<br />
[Hyperledger fabric](https://www.hyperledger.org/projects/fabric)

[박승철의정보보호론 제32-11강 대칭키암호화](https://www.youtube.com/watch?v=hh9NXQQRtx4&t=123s)