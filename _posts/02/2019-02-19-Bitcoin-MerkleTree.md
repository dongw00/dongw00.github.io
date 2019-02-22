---
layout: post
title: '비트코인이 Merkle Tree를 사용하는 이유'
subtitle: '왜 비트코인은 Merkle Tree 구조를 사용할까?'
date: '2019-02-19'
background: '/img/tags/Xcode-bg.jpg'
categories: Bitcoin
comments: true
---

# 왜 Bitcoin은 Merkle Tree 구조를 사용할까?

Bitcoin은 Transaction들을 Merkle Tree구조로 구성한 다음 Root Node의 해시 값(Merkle Root)를 블록헤더에 넣어서 블록을 만든다. 그런데 왜 Transaction 정보들을 `Merkle Tree구조`로 저장을 할까라는 의문이 들었다.
물론 2019년 현재까지 잘 이용해오는 Bitcoin을 개발자들이 잘 만들었겠지만 왜라는 부분을 찾으면서 그동안 배웠던 자료구조 개념에 대해서 더 이해하게되었다.

<br />

## Why Tree?

Merkle Tree를 알아보기 전 왜 Tree구조를 사용할 까라는 생각이 들었다. 컴퓨터 자료구조의 큰 관점으로 보면 `선형구조`(List, Stack, Queue 등), `비 선형구조`(Graph, Tree)로 나눌 수 있다.

![](/img/02/02-18.png)

선형구조와 비 선형구조에서 차이점은 노드 검색속도로 들 수 있다. 예를들어 Linked List구조로 저장해둔 과일 목록이 있다고 하자

![](/img/02/02-19.png)

위 Linked List에서 Graph를 찾는다고 하면 Apple -> Banana -> Graph 이 순서를 거쳐야 한다. 단순한 예라 금방 찾겠지만 원소가 무수히 많아진다면 검색시간이 정수배만큼 늘어난다.

![](/img/02/02-20.png)

Tree의 경우를 살펴보자 13을 검색한다고 했을때 이전 예에서 들었던 선형구조는 1부터 13까지 차례대로 검색해야한다.(오름차순으로 정렬되었다고 가정했을 때)

그러나 Tree구조로 13을 검색한다면 1 -> 6 -> 13으로 단순해진다.

시간복잡도로 표현한다면 `O(logN)`으로 나타낼 수 있다. 원소가 1024개라고 한다면 10번의 경로 내로 모든 원소를 검색할 수 있다.

> 선형구조인 경우 시간복잡도는 O(N)이다.

<br />

## Merkle Tree

![](/img/02/02-21.png)

이 글에서는 Merkle Tree가 무엇인지에 대해 자세히 다루지 않는다. Why에 초점을 맞춘 글이라 무엇인지는 자세히 설명하지는 않는다.

위의 그림은 Wikipedia Merkle Tree를 검색해보면 볼 수 있는 그림이다. 각 자식노드의 해시의 합을 해시한 값이 부모노드이다.

본론으로 돌아가서 왜 Bitcoin은 `Merkle Tree구조`를 사용하는지에 대해서 설명하고자 한다.

![](/img/02/02-22.png)

Bitcoin Network의 노드인 `Light Node`가 일부 블록의 내용 만으로 올바른 정보인지 검증을 해야할 때가 있다. `Full Node`와는 달리 `Light Node`는 전체 블록정보가 아닌 일부 정보만 가지고 있으므로 이런 경우가 생긴다.

현재 Light Node는 그림에서와 같이 Merkle Tree Root의 해시 값과 L4(Tom -> Jack에게 100BTC를 보냄)만 가지고 있다.

Light Node는 L4를 검증하고 싶은데 Light Node는 전체의 블록정보를 알지못하므로 Full Node에게 정보를 받아야 한다.

이때 Full Node에게 최소한의 Node정보만을 받는 것이 효율적이다.

![](/img/02/02-23.png)

Merkle Tree는 이 처럼 2개의 Node만을 받는다면 L4의 트랜잭션을 검증할 수 있게된다.

<br/>

**[참고자료]**

[Wikipedia - 해시트리](https://ko.wikipedia.org/wiki/%ED%95%B4%EC%8B%9C_%ED%8A%B8%EB%A6%AC)

[머클트리(Merkle Tree)란?](https://brownbears.tistory.com/372)
