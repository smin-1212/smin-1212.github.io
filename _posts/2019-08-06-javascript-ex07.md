---
layout: post
title: javascript 객체로서의 함수
tags: [javascript, js, es6]
---

#### 함수는 일급 객체

자바스크립트의 함수는 Function 객체이다.

다른 객체와 마찬가지로 다음과 같은 특성을 가진다.

* 함수는 변수나 프로퍼티나 배열 요소에 대입할 수 있다.
* 함수는 함수의 인수로 사용할 수 있다.
* 함수는 함수의 반환값으로 사용할 수 있다.
* 함수는 프로퍼티와 메서드를 가질 수 있다.
* 함수는 이름 없는 리터럴로 표현할 수 있다.(익명 함수)
* 함수는 동적으로 생성할 수 있다.

일반적으로 이러한 작업이 가능한 객체를 가리켜 **일급 객체** 라고 한다.

일급 객체인 함수는 **일급 함수** 라고 한다.

#### 함수의 프로퍼티

프로퍼티 이름 | 설명
---|---
caller | 현재 실행중인 함수를 호출한 함수
length | 함수의 인자 개수
name | 함수를 표시할 때 사용하는 이름
prototype | 프로토타입 객체의 참조

* 함수는 Function 생성자의 prototype객체(Function.prototype)의 프로퍼티를 상속 받아 사용한다.

Function.prototype 의 프로퍼티

프로퍼티 이름 | 설명
---|---
apply() | 선택한 this와 인수를 사용하여 함수를 호출한다. 인수는 배열 객체다.
bind() | 선택한 this와 인수를 적용한 새로운 함수를 반환한다.
call() | 선택한 this와 인수를 사용하여 함수를 호출한다. 인수는 쉼표로 구분한 값이다.
constructor | Function 생성자의 참조
toString() | 함수의 소스 코드를 문자열로 만들어 반환한다.

#### apply, call 메서드

* this 값과 함수의 인수를 사용하여 함수를 실행하는 메서드이다.
* apply, call 동작은 본질적으로 같다.
* apply 인수는 배열, call 인수는 쉼표로 구분한 값으 목록이다.


```javascript
function say(greetings, honorifics){
    console.log(greetings + " " + honorifics + this.name);
}
var tom = { name: "Tom Sawyer" };
var becky = { name: "Becky Thatcher" };
say.apply(tom, ["hello!", "Mr."]);
say.apply(becky, ["hi!", "Ms."]);
say.call(tom,"Hello", "Mr.");
say.call(becky, "hi!", "Mr.");
```

apply와 call 메서드의 첫 번째 인수는 함수의 this 값 이다.
apply 메서드의 두 번째 인수는 함수의 인수를 순서대로 담은 배열이다.



#### bind 메서드

Function 객체의 bind 메서드는 객체에 함수를 바인드 한다.

```javascript
function say(greetings, honorifics){
    console.log(greetings + " "  + honorifics + this.name);
}
var tom = { name: "Tom Sawyer" };
var sayToTom = say.bind(tom);
sayToTom("Hello", "Mr.");    // Hello! Mr. Tom Sawyer
```