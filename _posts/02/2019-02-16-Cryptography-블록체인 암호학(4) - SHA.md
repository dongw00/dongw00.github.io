---
layout: post
title: '블록체인 암호학(4) - SHA'
subtitle: '암호학 - SHA'
date: '2019-02-16'
background: '/img/01/01-01.png'
categories: Cryptography
use_math: true
comments: true
---

# 블록체인 암호학(4) - SHA

[블록체인을 위한 암호학(1) - DES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(1)-DES>)

[블록체인을 위한 암호학(2) - AES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(2)-AES>)

[블록체인을 위한 암호학(3) - 해시함수](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(3)-%ED%95%B4%EC%8B%9C%ED%95%A8%EC%88%98>)

이번 주제는 비트코인, 이더리움 등 수 많은 블록체인을 공부하다보면 항상 접하는 해시 `암호화 알고리즘`이다. 블록체인에 사용되는 암호화 알고리즘 중에서 가장 중요하므로 꼭 알아야할 부분이다.

<br />

## SHA란?

SHA(`Secure Hash Algorithm`)은 이전 포스팅에서 소개했던 해시함수를 이용해서 만든 해시 암호화 알고리즘의 모음이다. 이 함수들은 미국 국가보안국(NSA)에서 1993년 처음으로 설계했고 미국 국가표준으로 지정되었다.

SHA종류로는 SHA-0, SHA-1, SHA-2, SHA-3 이런식으로 나타내는데 실제 많이 사용되고 있는 함수군인 SHA-2에서 `digest` 의 길이에 따라 **SHA-224, SHA-256, SHA-384, SHA-512** 라고 불린다.

그런데 중요한점은 이전에 소개했던 DES, AES와는 다른 목적성을 가지고 있다.

DES와 AES는 `기밀성`(노출방지)을 목적으로 사용하는 반면에 SHA는 `무결성`(위조, 변조X)을 보증하는 역할로 사용한다. 실제 비트코인이나 이더리움에서 SHA가 사용되는 형태를 보면 블록헤더, 전자서명, 공개키등 위변조 방지에 초점을 둔다.

<br />

## SHA-1

SHA-1을 소개하기 전에 SHA-1의 이전에는 SHA-0이 존재했다. 1995년에 NSA는 SHA-0에 있는 보안문제에 의해 표준으로 지정된지 2년만에 SHA-1을 표준으로 정했다.

> 현재 어떤 이유로 SHA-1으로 대체하였는지 공표하지는 않았다.

충돌문제가 발생되어, 현재 대부분의 브라우저에서 "안전하지 않음" 메시지를 표시한다. NIST역시 SHA-1을 더이상 사용하지 말 것을 권장하고 있다.

<br />

## SHA-2

SHA-2는 **SHA-224, SHA-256, SHA-384, SHA-512** 종류가 있다.
실제 SHA-256는 비트코인의 해시 알고리즘으로 사용되므로 알아두는 것이 좋다.

<br />

## 해시함수의 예측 불가능성

해시함수의 암호학적 강도는 `예측 불가능성`에 의해 결정된다.

    SHA256(A) = 559AEAD08264D5795D3909718CDD05ABD49572E84FE55590EEF31A88A08FDFFD
    SHA256(B) = DF7E70E5021544F4834BBEE64A9E3789FEBC4BE81470DF629CAD6DDB03320A5C

A와 B의 비트열의 비트 하나 혹은 두 개 정도만 다르지만 해시 값은 완전히 다른 값을 가지게 된다.

<br />
