---
layout: post
title: 'Hyperledger composer REST Server 사용하기 - 예제'
subtitle: 'Composer REST Server와 React Native 연결'
date: '2019-01-29'
background: '/img/posts/06.jpg'
categories: Hyperledger
comments: true
---

# Hyperledger Rest Server

## Requirement

- OS : Ubunru 14.04 / 16.04 / 18.04 LTS or higher or Mac OS
- Docker Engine: 17.03 or higher
- Node : 8.9 or higher (9이상은 지원하지 않음)
- npm: v5.x
- git
- VS code

Hyperledger Fabric의 Network를 구성할 때 편하게 하려고 Composer를 사용하게된다.

React Native와 Hyperledger Composer의 Rest server를 연결하는 작업을 하는데 생각보다 자료가 너무 없어서 만드는데 고생을 했다.

이번 포스팅에서는 React Native의 간단한 예제를 바탕으로 Hyperledger Composer Rest Server에서 GET과 POST를 해보는 간단한 예제를 소개하려고 한다.

<br />

## 1. Hyperledger Composer playground 접속

    composer-playground

**1) Deploy a new business network 선택**

![](/img/02/02-01.png)

**2) carauction-network 선택**

![](/img/02/02-02.png)

1) 3. CREDENTIALS FOR NETWORK ADMINISTRATOR에서 ID and Select 선택

![](/img/02/02-03.png)

```text
Enrollment ID : admin

Enrollment Secret : adminpw
```

(Hyperledger fabric 기본값이므로 해당 값과 똑같이 설정해야 함)

**4) 생성완료됬으면 Connect now를 눌러서 해당 business card에 접속을 한다.**

![](/img/02/02-04.png)

**5) Model File의 내용을 수정한다.**

```text
asset Vehicle identified by vin {
  o String vin
  o String manufacturer
  o String model
  o Integer year
  --> Member owner
}
```

![](/img/02/02-05.png)

**6) Deploy해서 Update network**

![](/img/02/02-06.png)

**7) Asset 생성해보기**

![](/img/02/02-07.png)

Create Net Assets 클릭

![](/img/02/02-08.png)

```json
{
  "$class": "org.acme.vehicle.auction.Vehicle",
  "vin": "3676",
  "manufacturer": "Benz",
  "model": "S500",
  "year": 2018,
  "owner":"resource:org.acme.vehicle.auction.Member#9674"
}
```

저장

**8) REST Server 실행**

```bash
composer-rest-server -c admin@carauction-network -n never -w true
```

![](/img/02/02-09.png)

`Browse your REST API at`이 나왔다면 REST Server 구동 성공

[http://localhost:3000/explorer](http://localhost:3000/explorer) 접속

**9) Hyperledger Composer REST Server 접속**

![](/img/02/02-10.png)

**10) Postman 설치**

> REST API개발할 때 테스트 용으로 자주 사용하는 툴

**Ubuntu 18.04**

```bash
snap install postman
```

**Ubuntu 16.04**

    wget https://dl.pstmn.io/download/latest/linux64 -O postman.tar.gz
    sudo tar -xzf postman.tar.gz -C /opt
    sudo ln -s /opt/Postman/Postman /usr/bin/postman

설치가 완료됬으면 postman을 실행시킨다.

    postman &

![](/img/02/02-11.png)

[`http://localhost:3000/api/Vehicle`](http://localhost:3000/api/Vehicle)을 입력하고 GET을 선택한다.

![](/img/02/02-12.png)

**결과값 화면**

```json
[
    {
        "$class": "org.acme.vehicle.auction.Vehicle",
        "vin": "3676",
        "manufacturer": "Benz",
        "model": "S500",
        "year": 2018,
        "owner": "resource:org.acme.vehicle.auction.Member#9674"
    }
]
```

<br />

# Hyperledger Composer REST API와 RN연동

예제자료 Github 주소 : [https://github.com/dongw00/Learning-ReactNative](https://github.com/dongw00/Learning-ReactNative)

**1) 초기설정**

```bash
cd ~
git clone https://github.com/dongw00/Learning-ReactNative
cd Learning-ReactNative/RESTAPI_ReactNative
npm install
```

**2) 자신의 local REST API 주소로 수정**

```bash
ifconfig -a | grep 192
```

![](/img/02/02-13.png)

VS code → Profile.js의 `const URL` 수정

![](/img/02/02-14.png)

3) **Expo 구동**

```bash
expo start
```

![](/img/02/02-15.png)

GET ITEM화면

![](/img/02/02-16.png)

POST 화면

**Hyperledger Composer Playground에서 POST 결과 확인**

![](/img/02/02-17.png)


간단한 예제가 끝났다.

기본적으로 어떻게 Composer rest-server와 통신을 하는지를 이번 포스팅으로 알았기 때문에 Client가 React Native이던 웹 환경이든 서버에 요청하는 부분만 알맞기게 고쳐주면 된다.

<br />

**[참고자료]**

[Hyperledger composer tutorial](https://hyperledger.github.io/composer/latest/introduction/introduction)
