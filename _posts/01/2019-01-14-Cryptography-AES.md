---
layout: post
title: '블록체인 암호학(2) - AES'
subtitle: '암호학 - AES'
date: '2019-01-14'
background: '/img/01/01-01.png'
categories: Cryptography
comments: true
use_math: true
---

# 블록체인 암호학(2) - AES

- [블록체인 암호학(1) - DES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(1)-DES>)

- [블록체인 암호학(3) - 해시함수](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(3)-%ED%95%B4%EC%8B%9C%ED%95%A8%EC%88%98>)

[블록체인 암호학(4) - SHA](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(4)-SHA>)

이번 포스팅에서는 DES 이후 새로운 암호화 표준인 `AES`에 대해 알아보려한다.

1977년에 표준으로 지정된 DES는 오랫동안 암호화의 표준으로 잘 사용되었지만, 1990년대 이르러 기술 발전으로 56bit Key를 사용하는 기존의 암호화 방식인 DES는 더이상 안전하지 않게 되었다.

이러한 이유로 DES를 발전시킨 3-DES와 같은 방법도 사용되기는 하였지만 112bit 보안성을 위해서는 Key가 168bit여야 하기 때문에 여전히 보안성이 불충분했다.
그리고 무엇보다 소프트웨어에서 실행 속도가 느리기 때문에 대안이 필요했다.

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

아래 그림은 암호문을 State로 변환하는 것을 나타낸 것이다.

![](/img/01/01-18.png)

> Text -> 16진수의 값으로 변환해서 사용한다.

<br />

## 각 Round 암호화 과정

AES는 각 Round마다 4개의 절차를 가지고 있다.

![](/img/01/01-19.png)

위 그림은 AES의 각 Round를 묘사한 그림이다.
그림과 같이 Round에서는 State 값을 사용하게 된다.

> DES와 달리 S-box연산에서 6bit에서 4bit로 Compression(압축)을 하는 연산을 하지 않는다.

AES 알고리즘은 안정성을 제공하기위해 `대치`(Substitution), `치환`(Permutation), `섞음`(Mixing), `키 덧셈`(Key-adding) 4가지 형태의 변환을 제공한다.

1. **SubBytes (Substitute Byte)** <br />
   S-box table을 이용하여 byte단위 형태로 블록을 교환한다.
   각 byte를 4bit씩 2개의 16진수로 계산하여 왼쪽 4bit를 S-box의 행으로 오른쪽 4bit를 열로 Table을 읽는다.

   ![](/img/01/01-28.png)
   ![](/img/01/01-29.png)
   ![](/img/01/01-20.png)

   > 00=63, 12=C9 이런식으로 대치(Substitution)

2. **ShiftRows (Permutation)** <br />
   4개의 행은 각각 왼쪽으로 Shift되는데 행의 첫번째 값은 그 행의 맨 오른쪽 값으로 이동한다.

   - 첫 번째 행은 Shift되지 않는다.
   - 두 번째 행은 한 자리 왼쪽으로 Shift된다.
   - 세 번째 행은 두 자리 왼쪽으로 Shift된다.
   - 마지막 행은 세 자리 왼쪽으로 Shift된다.

   > 이때 byte안의 bit는 그대로 두고 byte를 교환한다. (byte-exchange transformation)

3. **MixColumns** <br />
   열 단위 연산을 수행한다. 각각의 열을 상수 행렬과 곱해서 새로운 값을 가지는 열을 반환한다.

   ![](/img/01/01-21.png)
   ![](/img/01/01-30.png)

   > 행렬의 곱셈을 이용해서 byte를 섞는다.

4. **Add Round Key** <br />
   한 번에 한 열씩 수행을 하게 된다. (MixColumns와 유사) 각 State 열 행렬에 Round Key word를 XOR 연산을 한다.
   1State 행렬(128bit)과 128bit Round Key가 XOR연산이 되는 것이라고 생각하면된다.

   ![](/img/01/01-22.png)

<br />

## Key Expassion (Key Schedule)

AES의 `Key scheduling`은 1) `Key Expanssion`과 2) `RoundKey Selection`의 두 부분으로 이루어져있다.

각 Round에 사용하는 Round Key를 생성하기위해 **먼저 키 확장(Key Expanssion)과정을 진행**한다.

이렇게 생성한 Round Key는 Round Function 암호화 전에 PlainText와 XOR하고 나머지 Round Key들은 Round 마지막 단계에서 AddRoundKey 동작을 한다.

아래 그림은 AES-128에서 키 확장이 일어나는 그림이다.

![](/img/01/01-23.png)

Word는 4개의 byte로 이루어져있는데, `Key Expanssion 루틴`은 Word단위로 Round Key를 생성한다. 10개의 Round로 이루어진 AES-128의 경우 44word가 필요하다.
(각 Round마다 4개의 word를 사용하는데 1 Round 들어가기 전에 한번 Round Key와 XOR하기 때문에 4word=1 Roundkey가 더 필요하다)

그럼 이제 자세하게 Key Expanssion이 어떻게 일어나는지 알아보자.

1. 128bit키를 4개의 word로 바꾼다. ($w_0$ ~ $w_3$)
2. 마지막 word는 `RotWord`(left Rotate), `S-box연산`을 한 후 마지막으로 `Round 상수와 XOR연산`한다. 이 값은 t값이 되어 다음 Round word가 생겨날때 XOR을 한다.

각 Round마다 반복

$$if(i\ mod\ 4=0) w_i=t\oplus \!\,w_{i-4}$$

$$else\ w_i=w_{i-1}\oplus \!\,w_{i-4}$$

$$t = SubWord(RotWord(w_{i-1}))\oplus \!\,RCon_{i/4}$$

- `RotWord` 루틴은 워드를 순환이동 시키는 ShiftRows변환과 유사한데, RotWord는 하나의 열에만 적용된다.
  하나의 word를 입력으로 왼쪽으로 1byte씩 이동한다.

- `SubWord`는 SubBytes변환과 유사하다.4byte만 적용이 되는데, 이 루틴에서 워드의 각 byte를 S-box를 이용하여 대치(Substitution)한 후 출력한다. RCon(Round Constant)는 4byte 값으로, 가장 오른쪽의 3byte는 모두 0이다.

- `RCon`은 Round 상수라고 생각하면된다.

마지막으로 `Round Key Selection`은 Key Expanssion 과정에서 나온 결과를 4word = 1State 단위로 묶어서 각 Round의 Key로 사용된다.

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

## AES 특성

`AES`는 DES가 설계된 후에 만들어졌기 때문에 DES에 대한 알려진 대부분 공격들에 대한 대비책이 AES에 적용되었다.

1. **무작위 대입 방식(Brute-Force Attack)**
   AES가 더 큰 Size의 Key를 사용하기 때문에 DES보다 안전하다.

2. **Statistical Attack**
   수 많은 테스트들이 암호문의 통계적인 분석에 실패했다.

3. **Differential and Linear Attacks**
   아직까지 AES에 대한 차분 공격과 선형 공격이 알려지지 않았다.

<br />

## 현재

세계적으로 하드웨어에서나 소프트웨어에서 널리 쓰고있는 암호화 기법이다. 현재까지는 알려진 공격방법이 없기때문에 안전하고, AES는 key 길이를 128/192/256bit 여러가지 쓸 수 있기때문에 장점을 가지고 있다.

<br />

**연관 게시글** <br>
[AES - 소스코드 분석](https://dongw00.github.io/Cryptography-AES-%EC%86%8C%EC%8A%A4%EC%BD%94%EB%93%9C%EB%B6%84%EC%84%9D)

<br />

**참고자료**<br />
[Wikipedia - AES](https://ko.wikipedia.org/wiki/%EA%B3%A0%EA%B8%89_%EC%95%94%ED%98%B8%ED%99%94_%ED%91%9C%EC%A4%80)

[Advanced Encryption Standard](https://www.tutorialspoint.com/cryptography/advanced_encryption_standard.htm)

[AES-문서분석-4-KeyExpanssion](https://frontjang.info/entry/AES-%EB%AC%B8%EC%84%9C-%EB%B6%84%EC%84%9D-4-Key-Expansion)

[처음 배우는 암호화 - 한빛미디어](http://www.hanbit.co.kr/store/books/look.php?p_code=B3633028491)
