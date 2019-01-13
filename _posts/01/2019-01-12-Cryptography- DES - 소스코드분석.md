---
layout: post
title: '[Java]DES - 소스코드 분석'
subtitle: 'DES - Java Source Code 분석'
date: '2019-01-12'
background: '/img/01/01-01.png'
categories: Cryptography
use_math: true
comments: true
---

# DES - 소스분석

- [블록체인을 위한 암호학 시리즈(1) - DES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%95%94%ED%98%B8%ED%95%99-%EC%8B%9C%EB%A6%AC%EC%A6%88(1)-DES>)

이전 포스팅에서 DES에 대해 살펴보았는데 이론적으로만 배우니 와닿지 않아서 소스코드를 분석을 해본다면 구체적으로 어떻게 동작이 될지해서 공부해보았다.

[참고자료](https://cafbit.com/post/implementing_des/)

> 해당코드는 java언어로 작성되었습니다.

<br />

## 메시지를 키와 함께 암호화

```java
public static byte[] encrypt(byte[] msg, byte[] key) {
    byte[] cipherText = new byte[msg.length];

    /* 8byte(64bit) 블록단위로 암호화 */
    for (int i=0; i < msg.length; i += 8) {
      encryptBlock(msg, i, cipherText, key);
    }
    return cipherText;
}
```

사용자가 메시지를 입력을 하고 암호화를 할 때 처음 거치는 함수이다.

구현된 함수는 `ECB`(Electronic Code Block)모드로 사용된다.

각 64bit마다 동일한 키로 암호화된다.

<br />

## 64bit block의 평문을 암호화

```java
public static void encryptBlock(byte[] msg, int msgOffset,
    byte[] cipherText, int cipherTextOffset, byte[] key) {
    long m = getLongFromBytes(msg, msgOffset);
    long k = getLongFromBytes(key, 0);
    long c = encryptBlock(m, k);
    getBytesFromLong(cipherText, cipherTextOffset, c);
}
```

이 부분의 과정에서는 Byte로 받은 값을 long으로 변환해주는 작업을 한다.

실제 작업은 아래 부분에서 진행한다.

```java
public static long encryptBlock(long m, long key) {
    long ip = IP(m);  // Initial Permutation 수행
    long subkeys[] = createSubkeys(key);  //16개 subkey 생성

    /* 32bit 값을 16bit 좌, 우 반으로 나눈다. */
    int l = (int)(ip >> 32);
    int r = (int)(ip & 0xFFFFFFFFL);

    /* 16번 Round Function 수행 */
    for (int i = 0; i < 16; i++) {
        int previous_l = l;
        l = r;
        /* 이 부분은 아래설명 */
        r = previous_l ^ feistel(r, subkeys[i]);
    }
    // 16번 Round Function 수행한 결과 값의 좌, 우 32bit를 swap 합친다.
    long rl = (r & 0xFFFFFFFFL) << 32 | (l & 0xFFFFFFFFL);
    long fp = FP(rl);  // Final Permutation 수행

    return fp;  // 암호화된 결과값을 반환
}
```

이전 포스팅에서 설명했던 것과 같이 왼쪽 bit는 Round Function에 의해 XOR되어 암호화가 되지만 오른쪽 bit는 단지 swap이 되기때문에 새로운 좌, 우 bit를 변수 l, r로 표현했다.

<br />

## Feistel function

```java
private static int feistel(int r, /* 48 bits */ long subkey) {
    long e = E(r);  // 1. expansion(확장)
    long x = e ^ subkey;  // 2. key mixing(XOR)
    int dst = 0;  // 3. substitution(S-box연산)
    for (int i = 0; i < 8; i++) {
        dst >>>= 4;
        int s = S(8 - i, (byte) (x & 0x3F));
        dst |= s << 28;
        x >>= 6;
    }
    return P(dst);  // 4. permutation
}
```

DES의 핵심부분이다. 아래 그림을 보면 더 쉽게 이해할 수 있을 것이다.

![](/img/01/01-06.png)

<br />

## Subkey 생성과정

```java
private static long[] createSubkeys(long key) {
    long subkeys[] = new long[16];

    key = PC1(key);  // 대칭 키 PC1 Permutation 수행

    /* 28bit 좌, 우를 나눈다 */
    int c = (int)(key >> 28);
    int d = (int)(key & 0x0FFFFFFF);

    /* 16개 subkey를 생성한다. */
    for (int i = 0; i < 16; i++) {
        /* 28bit 값을 순환시킨다. */
        if (rotations[i] == 1) {
            /* 1bit씩 순환시킨다. */
            c = ((c << 1) & 0x0FFFFFFF) | (c >> 27);
            d = ((d << 1) & 0x0FFFFFFF) | (d >> 27);
        } else {
            /* 2bit씩 순환시킨다. */
            c = ((c << 2) & 0x0FFFFFFF) | (c >> 26);
            d = ((d << 2) & 0x0FFFFFFF) | (d >> 26);
        }
        // 나눴던 key 쌍을 합친다.
        long cd = (c & 0xFFFFFFFFL) << 28 | (d & 0xFFFFFFFFL);
        subkeys[i] = PC2(cd);
    }
    return subkeys;  // 48bit subkey
}
```

`Round-key Generator` 부분 함수이다. 먼저 64bit key를 받는다.

그 다음 PC1 Permutation을 하게되면 56bit로 받게된다.

> 64bit key를 받지만 8bit는 오류검사 parity bit이기 때문에 실제 DES에서 56bit를 사용한다.

1, 2, 9, 16 Round에서는 1bit 왼쪽 순환이동을 하고 나머지 Round에서는 2bit씩 순환이동을 한다.

<br />

## DES 암호화 Test Function

```java
public static boolean test(byte[] message, byte[] expected, byte[] key) {
    System.out.println("Test #" + (++testCount) + ":");
    System.out.println("\tmessage:  " + hex(message));
    System.out.println("\tkey:      " + hex(key));
    System.out.println("\texpected: " + hex(expected));
    byte[] received = encrypt(message, key);
    System.out.println("\treceived: " + hex(received));
    boolean result = Arrays.equals(expected, received);
    System.out.println("\tverdict: " + (result ? "PASS" : "FAIL"));
    return result;
}
```

DES 테스트에서 사용자가 **입력한 평문**과 **서버에 암호문**이 같다면 PASS하는 함수이다.

```java
public static void main(String[] args) {
    test(parseBytes("0123456789ABCDEF"), parseBytes("85E813540F0AB405"),parseByte    ("133457799BBCDFF1"));
}
```

평문 `0123456789ABCDEF` 와 키 `133457799BBCDFF1`가 서버에 저장된 암호문 `85E813540F0AB405` 이 같은지 판단하는 테스트를 진행해보겠다.

![](/img/01/01-10.png)

정상적으로 작동한다.

<br />

## 기타 설정 값들

**Initial Permutation Table**

```java
private static final byte[] IP = { };
```

처음 암호화 하고자 하는 평문을 처음 순열연산할 때 사용하는 Table이다.

> Hash 함수를 이용해서 비밀번호를 저장할때 하는 Salting 작업이라고 생각하면된다.

**Final Permutation Table**

```java
private static final byte[] FP = { };
```

이 부분도 Initial Permutation과 마찬가지다. 마지막 암호화 한 값을 순열 연산할 때 사용하는 Table이다.

**Expanssion Permutation**

```java
private static final byte[] E = { };
```

Round Function에서 64bit를 반으로 나눈 왼쪽 32bit를 입력받는데, **Subkey는 48bit**이다.

따라서 XOR하려면 48bit값이 필요한데 이때 확장전치를 하기 위해 사용하는 Table이다.

**S-box**

```java
private static final byte[][] S = { }
```

`Substituition box`라고 부른다. Feistel에서 가장 중요한 부분이다.

S-box Table에 따라 bit가 바뀐다. 48bit의 값을 6bit 8개 부분으로 나눈후 4bit로 압축한다.

최종적으로 32bit값이 반환된다.

**Straight P-box Permutation**

```java
private static final byte[] P = { };
```

Straight P-box 과정에서 사용하는 Table이다.


## Sub-key Generation

**PC1 Permutation**

```java
private static final byte[] PC1 = { };
```

`PC1 Permutation`에서는 64bit 대칭키를 table을 통해 연산하고 나면 **56bit key**로 변환
받는다. (64bit key에서 8bit는 parity bit이므로 실제 DES에서는 56bit key를 사용한다.)

**PC2 Permutation**

```java
private static final byte[] PC2 = { };
```

`PC2 Permutation` Compression P-box라고 보면된다. 순환 연산 이후 **48bit의 Subkey**를 받게된다.


## Rotations

```java
private static final byte[] rotations = { };
```

1, 2, 9, 16 Round에서는 1bit 왼쪽 순환이동을 하고 나머지 Round에서는 2bit씩 순환이동을 한다. 이 부분을 나타낸 Table이다.

[전체적인 코드](https://cafbit.com/resource/DES.java)
