---
layout: post
title: '[Java]AES - 소스코드 분석'
subtitle: 'AES - Java Source Code 분석'
date: '2019-01-19'
background: '/img/01/01-01.png'
categories: Cryptography
comments: true
---

# AES - 소스분석

- [블록체인 암호학(2) - AES](<https://dongw00.github.io/Cryptography-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EC%95%94%ED%98%B8%ED%95%99(2)-AES>)

AES 암호학 알고리즘 이론 포스팅에 이어서 Java 소스코드를 분석하면서 어떻게 실제 적용할 것인지 알아볼 것이다.
이전 DES와는 달리 깊게 들어가진 않고 Java 라이브러리를 이용해서 대략적으로 어떻게 소스코드로 구현을 하는지 알아보려한다.

> 해당코드는 java언어로 작성되었습니다.

<br />

## 대칭 키 생성

```java
public static SecretKey getSecretEncryptionKey() throws Exception {
    KeyGenerator generator = KeyGenerator.getInstance("AES");   // AES Key Generator 객체 생성
    generator.init(128);    // AES Key size 지정
    SecretKey secKey = generator.generateKey();     // AES 암호화 알고리즘에서 사용할 대칭키 생성

    return secKey;
}
```

`KeyGenerator`는 javax.crypto에서 대칭키를 생성하는 클래스이다. AES뿐만 아니라 DES, DESede, HmacSHA1, HmacSHA256의 키 생성도 가능하다.
`getInstance()` 메소드를 이용해서 생성하면 되는데 parameter를 생성하고 싶은 암호 알고리즘을 쓰면된다.

[Oracle KeyGenerator 레퍼런스](https://docs.oracle.com/javase/7/docs/api/javax/crypto/KeyGenerator.html)

[Oracle SecretKey 레퍼런스](https://docs.oracle.com/javase/7/docs/api/javax/crypto/SecretKey.html)

<br />

## 평문 Text 암호화

```java
public static byte[] encryptText(String plainText, SecretKey secKey) throws Exception {
    Cipher aesCipher = Cipher.getInstance("AES");
    aesCipher.init(Cipher.ENCRYPT_MODE, seckey);
    byte[] byteCipherText = aesCipher.doFinal(plainText.getBytes());    // 암호문 생성

    return byteCipherText;
}
```

평문을 AES을 이용해 암호문을 만드는 알고리즘이다. 먼저 `Cipher`클래스를 사용한다.
이 클래스도 마찬가지로 javax.crypto에서 제공된다. 이 클래스에서 AES `암호화/복호화` 기능을 제공하는데 CBC, PKCS5Padding 모드 선택 기능도 제공한다. `getInstance()` 메소드를 이용해서 객체를 생성해야 한다.

getInstance()로 생성된 aesCipher 객체에 `Cipher.ENCRYPT_MODE`를 parameter로 넘겨주어 암호화 모드로 초기설정을 한다.

[Oracle Cipher 레퍼런스](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)

<br />

## 암호문 복호화

```java
public static String decryptText(byte[] byteCipherText, SecretKey secKey) throws Exception {
    Cipher aesCipher = Cipher.getInstance("AES");
    aesCipher.init(Cipher.DECRYPT_MODE, secKey);    // 복호화 모드 초기화
    byte[] bytePlainText = aesCipher.doFinal(byteCipherText);   // 암호문 -> 평문으로 복호화

    return new String(bytePlainText);
}
```

복호화도 크게 다를게 없다. 암호화와 마찬가지로 진행하면 되는데 중간에 `Cipher.DECRYPT_MODE` 모드를 Parameter로 넘겨주면 된다.

<br />

## 테스트

```java
public static void main(String[] args) throws Exception {
    String plainText = "Hello World!";

    SecretKey seckey = getSecretEncryptionKey();    // 대칭키를 받는다.
    byte[] cipherText = encryptText(plainText, secKey); // 평문을 암호화

    String decryptedText = decryptText(cipherText, secKey); // 복호화
    System.out.println("Original Text : " + plainText);
    System.out.println("AES Key (Hex) : " + bytesToHex(secKey.getEncoded()));
    System.out.println("Encrypted Text (Hex) : " + bytesToHex(cipherText));
    System.out.println("Descrypted Text : " + decryptedText);
}
```

Hello Word!를 AES 암호화 하는 코드이다.

![](/img/01/01-31.png)

테스트 결과화면
