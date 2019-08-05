---
layout: post
title: javascript 객체
tags: [javascript, js, es6]
---

#### 객체 기초

#### 객체
> * 객체는 이름과 값을 한쌍으로 묵은 데이터를 여러개 모은것이다.
> * 연관 배열, 사전이라고 부른다.

#### 객체 리터럴로 객체 생성
```javascript
 var card = { suit : "하트", rank : "A"}
```
* 마침표 다음에 문자열이 붙어있지 않고, 점과 대괄호 안이 문자열로 채워져 있음,

* 객체에 없는 프로퍼티를 읽으려고 시도하면 undefined 를 반환함

* 없는 프로퍼티 이름에 값을 대입하면, 새로운 프로퍼티가 추가된다.


![ex01](https://drive.google.com/uc?id=1XUjrosXtE0pvvaXv40TMhg6kfH5O7LAj){: width="307px" height="176px"}


* 객체 리터럴 안에 어떠한 프로퍼티도 작성하지 않으면 빈 객체가 생성된다.


![ex02](https://drive.google.com/uc?id=1jFntdbxvT_ZNt6piSBehoN8mkMGRJC7l)
{: width="75px" height="35px"}


* delete 연산자를 사용하면 프로퍼티를 삭제할 수 있다.


![ex03](https://drive.google.com/uc?id=1zAogcaoBMFmWJBwdqtkeVOuTvSRiOlnm){: width="288px" height="138px"}


* 프로퍼티에 저장된 값의 타입이 함수면 그 프로퍼티를 **메서드**라고 한다.


