---
layout: post
title: '블록체인 암호학(3) - 해시함수'
subtitle: '암호학 - 해시함수'
date: '2019-02-09'
background: '/img/01/01-01.png'
categories: Cryptography
use_math: true
comments: true
---

# 블록체인 암호학(3) - 해시함수

[블록체인 암호학(1) - DES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(1)-DES>)

[블록체인 암호학(2) - AES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(2)-AES>)

[블록체인 암호학(4) - SHA](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(4)-SHA>)

이번 주제는 비트코인이나 이더리움을 공부하다보면 자주 접하는 단어이다. 바로 `해시`이다.

해시함수는 다양한 곳에서 활용이 되는데 디지털 서명(Digital Signature), 공개 키 암호화(Public-key encryption), 무결성 검증(Integrity verification), 패스워드 보호(Password protection) 등 암호학자들에게 맥가이버 칼과 같은 존재이다.

<br />

## 해시함수의 정의

먼저 해시함수에 대해 소개를 하자면 임의의 길이의 입력값을 받아 고정된 길이의 출력값을 받게되는데 이때 출력값을 `해시 값` 또는 `다이제스트`(Digest)라고 부른다.

    Hello → 185F8DB32271FE25F561A6FC938B2E264306EC304EDA518007D1764826381969

    world → 486EA46224D1BB4FB680F34F7C9AD96A8F24EC88BE73EA8E5A6C65260E9CB8A7

[SHA256 해시생성기](http://www.convertstring.com/ko/Hash/SHA256)

위에 예는 해시 암호학 알고리즘 중에 SHA256의 결과값을 소개했다. 임의의 입력값을 넣어서 정해진 길이의 출력값을 얻는데 이때 두 값은 각각 다른 값을 얻게된다.

<br />

## 해시함수 목적성

이전 포스팅에서 소개했던 대칭키 블록 암호화 알고리즘인 DES, AES와는 조금 다른 목적성을 가지고 있다.

DES, AES는 **평문의 자료를 암호화**하여 저장 혹은 전송함으로써 다른 누군가가 알아내지 못하는데 목적이 있다. 반면에 해시함수는 `자료의 무결성`을 보장한다.

즉, 중간에 누군가가 수정을 방지한다. 실제로 해시함수의 가장 흔한 용도인 **디지털 서명(Digital Signature)**을 생각해보면 응용프로그램은 서명할 메시지 자체가 아닌 메시지의 해시를 처리한다. 이때 해시는 **메시지의 식별자 역할**을 하게된다.

<br />

## 해시함수의 예측 불가능성

해시함수의 암호학적 강도는 `예측 불가능성`에 의해 결정된다.

    SHA256(A) = 559AEAD08264D5795D3909718CDD05ABD49572E84FE55590EEF31A88A08FDFFD
    SHA256(B) = DF7E70E5021544F4834BBEE64A9E3789FEBC4BE81470DF629CAD6DDB03320A5C

A와 B의 비트열의 비트 하나 혹은 두 개 정도만 다르지만 해시 값은 완전히 다른 값을 가지게 된다.

<br />

## 역상 저항성(Preimage resistance) <br />

$$Hash(M) = H$$

M을 해시 값의 역상(Preimage)이라고 부른다. 역상 저항성(Resistance)은 어떤 무작위 값이 주어졌을 때 공격자가 그 해시의 **원래 값을 찾지 못한다는 것을 뜻한다**.

<br />

## 충돌 저항성(Collision resistance)

해시함수의 정의를 보면 임의의 입력값은 `정해진 길이의 출력 값`을 가진다고 했다. 그렇게 된다면 결국에는 어떤 메시지들은 같은 해시 값을 가지는 메시지가 존재하게된다.

> 이는 비둘기집 원리에 의해 증명이된다.

해시 충돌은 해시 함수를 이용한 자료구조나 알고리즘의 효율성을 떨어트린다.

암호학적 해시 함수의 경우 해시 함수의 안전성을 깨뜨리는 충돌 공격이 가능할 수 있기 때문에 의도적인 해시 충돌을 만드는 것이 어렵도록 만들어야 한다.

<br />
**참고자료**<br />
[처음 배우는 암호화 - 한빛미디어](http://www.hanbit.co.kr/store/books/look.php?p_code=B3633028491)
