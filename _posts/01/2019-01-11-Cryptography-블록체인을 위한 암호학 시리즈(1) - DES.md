---
layout: post
title: '블록체인을 위한 암호학 시리즈(1) - DES'
subtitle: '암호학 - DES'
date: '2019-01-10'
background: '/img/01/01-01.png'
categories: Cryptography
use_math: true
comments: true
---

# 블록체인을 위한 암호학 시리즈(1) - DES

블록체인을 공부하면서 dApp을 만들어보고 비트코인이 어떤 방식으로 트랜잭션이 될까라는 부분부터 시작했었다.
그런데 기초 암호학 지식이 없는 상태이다보니 이해하는데 상당한 시간이 걸렸고 수박 겉햛기식으로 학습이 지속되었다.

블록체인을 배우면서 만났던 멘토분께서 암호학을 먼저 공부하는 것이 좋다는 말씀을 듣고 공부한 내용들을 정리하였다.

<br />
## DES (Data Encryption Standard)

[DES](https://namu.wiki/w/DES)는 IBM에서 고안되어 NIST가 미국 표준암호 알고리즘으로 지정한 `대칭 키 블록 암호화` 기법이다.

> 현재는 평범한 PC로도 손쉽게 해독이 가능하기 때문에, 기존에 암호화된 문서를 복호화하는 용도로만 사용을 하고 신규 암호화 문서를 생성하는데는 절대로 사용하지 말 것을 권장하는 암호화 알고리즘이다.

그런데 왜 DES를 공부하냐면 DES의 암호화 알고리즘의 근간인 Feistel Network를 공부하는데 의미가 있다.
<br />

## 개요

DES를 살펴보기 전에 대칭키 암호화 기술에 대해서 알아야한다.
[대칭 키 암호 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EB%8C%80%EC%B9%AD_%ED%82%A4_%EC%95%94%ED%98%B8)

간단하게 설명하자면 암호, 복호화시 같은 암호 키를 사용하는 알고리즘을 뜻한다.

DES는 대칭 키 알고리즘에 속하면서 `블록단위 암호화 방식`을 사용하며 16단계의 `파이스텔 네트워크`(Feistel Network)를 거쳐 암호화를 수행한다.

그런데 파이스텔 네트워크(Feistel Network)라는 것이 무엇일까?

<br />
## **Feistel Network란?**

블록단위 암호화 방식의 일종이며, 암호화시 특정 계산 함수의 반복으로 이루어지는 것을 말한다.

특정 계산 함수를 `Round Function`이라고 한다.

> 블록단위 암호화라고해서 모두가 Feistel을 사용하는 것은 아니다.

<br />
## DES 암호화 과정

![](/img/01/01-02.png)

Feistel Network라고 부르는 부분은 Round1~16인 부분을 뜻한다.

DES 암호화 과정에서 크게 **3가지**를 보면된다.

- Initial Permutation & Final Permutation
- Round Function
- Round-Key Generator

<br />
## Initial Permutation & Final Permutation

P-box (Permutation box)라고 부르는데 특히 이 과정에서는 **암호화가 일어나지 않는다**. <br />

> 서버에 사용자의 패스워드를 저장할때 첨가하는 Salt과정이라고 생각하면된다.
> ![](https://upload.wikimedia.org/wikipedia/commons/c/c7/Link_between_S-Boxes.gif)

위 그림은 [Wikipedia - Permutation box](https://en.wikipedia.org/wiki/Permutation_box)에서 참고한 그림이다.

입력된 64bit 평문 데이터가 `P-box`를 통해 그 다음인 `S-box`(Round Function 부분)에 나뉘어 입력이 되는 것을 묘사한 그림이다.

![](/img/01/01-03.png)

위 그림은 Initial Permutation과 Final Permutation을 자세히 보여주는 그림이다.

아래의 `Permutation Table`에 따라 **bit swap**이 일어난다.

(단순히 말해서 상위bit와 하위bit가 바뀐다고 생각하면된다.)

![](/img/01/01-04.png)

<br />
## Round Function

실제 암호화는 이 과정에서 일어난다.<br>
$$L_x = R_{x-1}$$ <br />
$$R_x = L_{x-1} \oplus \!\, F(R_{x-1}, K_x)$$

- Rx : x단계의 하위(R) 결과물
- Lx-1 : x-1단계의 상위(L) 결과물
- F : Round Function
- K : Round Key

  ![](/img/01/01-05.png)

  <박승철 교수님 - DES강의자료>

수학식만을 본다면 이해하는데 어려움을 느낄수 있지만 그림과 같이 참고한다면 쉽게 이해할 수 있다.

**상위 32bit는 Round Function과 XOR이 되어 암호화**가 진행된다.

하위 32bit는 이와다르게 암호화는 되지않고 다음단계의 **상위 32bit로 swap될 뿐**이다.

![](/img/01/01-06.png)

위 그림은 RoundFunction을 디테일하게 표현한 그림이다.

상위 32bit값이 `Expansion P-box`에서 48bit로 확장이 일어난다.

> 확장이 일어나는 이유는 Round-key Generator에서 만든 Round key와 bit수를 맞추어야하기 때문이다.

아래 그림은 32bit가 48bit로 확장이 일어나는지 설명한 그림이다.

각 섹션의 1~4bit는 그 다음 output의 2~5bit로 복사가된다.

그럼 1, 6bit부분은 비어있게 되는데 1bit는 이 전 섹션의 4bit값을 복사한다. 마찬가지로 6bit부분은 이 후 섹션의 1bit값을 복사한다.

![](/img/01/01-07.png)

확장된 48bit와 Round-key Generator에서 만든 Round key와 XOR을 한 결과 값 48bit는 S-box라는 곳에서 Compression(압축)이 일어난다.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Data_Encription_Standard_Flow_Diagram.svg/1024px-Data_Encription_Standard_Flow_Diagram.svg.png)

위 그림에서 S1~S8부분이 S-box내부의 그림이다.

48bit를 6bit 8개로 나눈다음 `S-box Table`에 의해 4bit로 압축이된다.

마지막으로 Straight P-box에서 좌, 우 Swap을 진행해줌으로써 한 Round가 끝난다.

이러한 과정을 16번 반복한다.

<br />
## Round-Key Generator

이 과정에서는 DES에서 사용하는 56bit 암호 키(대칭키)를 16개의 Subkey로 생성한다. (Round Function에서 사용할 암호 키)

아래 그림을 보면 처음 Input시 64bit인데 8bit는 오류검증으로 사용하는 Parity bit이다.

![](/img/01/01-09.png)

Round-key Generator에서 Round key를 생성은 3단계로 이루어진다.

- **1단계** 56bit key를 Key swap 연산을 통해 두 개의 28bit block으로 나눈다.
- **2단계** 두 개의 블록은 1, 2, 9 ,16Round에는 1bit 왼쪽 순환이동을 하고, 나머지 Round에는 2bit 이동을 한다.
- **3단계** 각 28bit block은 Compression(압축) 전치를 통해 24bit block이 되며, 두 개의 block이 결합되어 48bit Round Key가 생성된다.

<br />
## DES의 특성

**1. Avalanche effect (눈사태 효과)** - 키의 작은 변화가 결과값에 큰 영향을 미치는 효과를 뜻한다. DES는 이 특징에 대해 강력함을 보였다.

**2. Completeness (완전성)** - 암호문의 각각의 bit가 원래의 내용의 bit에 의존적이다. (암호문으로부터 평문보호) P-box와 S-box에 의해 강력한 완전성을 보여준다.

현재 DES는 1997년에 미국 보안업체 RSA에서 DES 해독에 \$10,000 상금을 걸고 해독 대회를 열었는데, 이때 DES의 56bit 키로 가능한 72천조개의 조합중에 18천조 개의 조합을 넣은 단계에서 해독이 되었다.

현재 상업적으로 사용되는 Private Key 알고리즘에는 적어도 80bit 이상이 되어야한다.

아무튼 1998년에 DES공격장치가 **Brutal Force Attack(무차별 공격)**방식으로 2일만에 암호를 탈취당했으니 현재는 사용하지 말아야 할 암호이지만 오랜시간 쓰였던 암호이었던 만큼 강화된 다중 DES (ex, 3-DES)를 사용하는 곳이 많으므로 (비용문제 등으로) 알아두는 것이 좋을 듯 하다.

<br />
**참고자료**<br />
[Data Encryption Standard](https://www.tutorialspoint.com/cryptography/data_encryption_standard.htm)

[박승철의정보보호론 제32-11강 대칭키암호화](https://www.youtube.com/watch?v=dFb2ezWDO38)

[CS480 Cryptography and Information Security - ppt download](https://slideplayer.com/slide/11006182/)
