---
layout: post
title: '블록체인을 위한 암호학(2) - AES'
subtitle: '암호학 - AES'
date: '2019-01-14'
background: '/img/01/01-01.png'
categories: Cryptography
comments: true
---

# 블록체인을 위한 암호학(2) - AES

- [블록체인을 위한 암호학(1) - DES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%95%94%ED%98%B8%ED%95%99(1)-DES>)

이번 포스팅에서는 DES 이후 새로운 암호화 표준인 `AES`에 대해 알아보려한다.

1977년에 표준으로 지정된 DES는 오랫동안 암호화의 표준으로 잘 사용되었지만, 1990년대의 기술 발전으로 56bit Key를 사용하는 DES는 더이상 안전하지 않게 되었다.

기존 DES를 발전시킨 3-DES와 같은 방법도 사용되기는 하였지만 112bit 보안성을 위해서는 Key가 168bit여야 하기 때문에 여전히 보안성이 불충분했다. 그리고 무엇보다 소프트웨어에서 실행 속도가 느리기 때문에 대안이 필요했다.

<br />

## AES (Advanced Encryption Standard)

NIST에서 1997년에 `AES`라는 이름의 **표준으로 제정할 것을 발표**하였고, AES 암호화 알고리즘의 공모를 받게되었다.

조건으로 **128bit Block암호**이며, 128, 192, 256bit 길이의 Key를 지원할 것을 내걸었는데 벨기에 암호학자가 개발한 `Rijndael 알고리즘`이 최종적으로 채택되었다.

NIST는 2001년 11월에 Rijndael 알고리즘을 `FIPS 197`, `AES`라는 이름의 표준으로 발표했다.

<br />

## 특징
AES의 특징으로 3가지를 가지고 있다.

- 대칭키 암호화 알고리즘으로 128bit 블록을 사용한다.
- 암호화 Key로 128, 192, 256bit를 가질 수 있다.
  (AES-128, AES-192, AES-256)
- 암호화 Key의 길이에 따라 실행하는 Round수가 다르다.

<br />

## AES 구조

![](/img/01/01-11.png)

AES는 DES와 다르게 Feistel Network를 사용하지 않았다.

`Feistel`은 **총 bit에서 반을 암호화 하는 방식**인 반면에 **AES**는 `대입치환` - `Substitution-Permutation Network`을 사용하는데, 용어 그대로 **대입(Substitution)**과 **치환(Permutation)**을 이용하여 암호화하는 방법이고 **전체 bit를 암호화 하는 방식을 사용**한다.

또한 각 Round수는 key의 길이에 따라 다르다.

예를들어 Key의 크기가 128/192/256bit일 때 10/12/14번의 Round를 사용한다.

> 모든 경우에서 생성되는 Round Key의 크기는 평문과 같은 128bit이고 Round수의 +1만큼 더 생성된다.
> (Pre-round transformation에서도 사용되기 때문)

<br>
## State
AES의 알고리즘은 내부적으로 State라고 불리는 Byte의 2차원 배열을 사용한다.
> (1State = 4word = 16byte = 128bit)

![](/img/01/01-12.png)

<br />

## 각 Round 암호화 과정
AES는 각 Round마다 4개의 절차를 가지고 있다.

![](/img/01/01-13.png)

위 그림은 AES의 각 Round를 묘사한 그림이다.

> DES와 달리 S-box연산에서 6bit에서 4bit로 Compression(압축)을 하는 연산을 하지 않는다.

1. **SubBytes (Byte Substitution)**
   16-byte 입력을 `S-box`에 따라 **다른 byte 값으로 대체(Substitution)작업**을 한다. 이 결과 4개의 행, 열을 가진 행렬을 반환한다.

2. **ShiftRows**
   4개의 행은 각각 왼쪽으로 Shift되는데 행의 첫번째 값은 그 행의 맨 오른쪽 값으로 이동한다.
   - 첫 번째 행은 Shift되지 않는다.
   - 두 번째 행은 한 자리 왼쪽으로 Shift된다.
   - 세 번째 행은 두 자리 왼쪽으로 Shift된다.
   - 마지막 행은 세 자리 왼쪽으로 Shift된다.

3. **MixColumns**
   행렬에서의 열은 동일한 선형변환([수학식](https://en.wikipedia.org/wiki/Rijndael_MixColumns))에 의해 변화된다. 하나의 4-byte열을 입력으로 받아서 완전히 새로운 4byte 결과값을 원래의 열과 **대체(Substitution)**한다.

   > 마지막 Round에서는 해당 연산(MixColumns)이 수행되지 않는다.

4. **Add Round Key**
   1State 행렬(128bit)과 128bit Round Key가 XOR연산이 된다.

<br />

## Key Expassion (Key Schedule)

AES의 Key scheduling 함수이다. SubBytes와 동일한 S-box와 XOR연산을 통해 128bit key로부터 11개의 Round Key를 생성한다.

<br />

## AES 블록 패딩 - PKCS#7 Padding

다른 Block 암호화 알고리즘처럼 패딩을 하여 Block의 빈 자리를 채울 필요가 있다.
PKCS#7 알고리즘은 byte단위로 덧붙이며, 부족한 byte수만큼 n개를 넣는다.

```
01 02 03 04 05 06 07 08 01 02 04 08 10 20 40 80 00 11 22 33 44 55 66 77 88 99 AA
```

위의 코드블럭에는 27byte(=216bit)이다. 16byte(=128bit) 블록으로 자른다면 5byte(=40bit)가 부족하게된다.

> 8bit = 1byte

```
01 02 03 04 05 06 07 08 01 02 04 08 10 20 40 80 / 00 11 22 33 44 55 66 77 88 99 AA 05 05 05 05 05
```

05를 붙여서 두 블록으로 만든다.

<br />

## 현재

세계적으로 하드웨어에서나 소프트웨어에서 널리 쓰고있는 암호화 기법이다. 현재까지는 알려진 공격방법이 없기때문에 안전하고, AES는 key 길이를 128/192/256bit 여러가지 쓸 수 있기때문에 장점을 가지고 있다.
