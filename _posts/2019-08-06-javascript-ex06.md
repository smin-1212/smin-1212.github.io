---
layout: post
title: javascript namespace
tags: [javascript, js, es6]
---

#### 전역 이름 공간의 오염

전역 변수와 전역 함수를 전역 객체에 선언하는 행위를 가리켜 **'전역 유효 범위를 오염시킨다'** 고 한다.

전역 유효범위가 오염되면 다음과 같은 상황일 때 변수 이름과 함수 이름이 겹칠 수 있다.

* 라이브러리 파일을 여러개 읽어 들여 사용할 때
* 규모가 큰 프로그램을 만들 때
* 여러 사람이 한 프로그램을 만들 때

#### 전역 유효 범위의 오염 최소화

**객체를 이름 공간으로 활용하기**

객체를 이름 공간으로 활용하려면 객체를 값으로 가지는 전역 변수를 하나 생성하고, 그 객체에 프로그램 전체에서 사용하는 모든 변수와 함수를 프로퍼티로 정의한다.

```javascript
var myApp = myApp || {};

// myApp이 이미 정의되어 있을 때는 그것을 사용하고, 그렇지 않으면 빈 객체를 myApp에 할당
// 전역 유효 범위의 오염을 최소화할 수 있다.
// 부분 이름 공간(sub name space)을 만들 수도 있다.

myApp.view = {};
myApp.controls = {};

// 각각의 부분 이름 공간안에 필요한 변수 또는 함수를 프로퍼티로 정의해서 사용
myApp.view.draw = function(){...};
myApp.controls.timeInterval = 16;

```

객체를 이름 공간으로 이용하면 변수 또는 함수 이름을 계층적으로 관리할 수 있다.

**함수를 이름공간으로 활용하기**

* 함수 안에서 선언된 변수의 유효 범위는 함수 내부이다.
* 그 변수를 함수 안에서는 읽거나 쓸수 있지만 바깥에서는 읽거나 쓸 수 없다.
* 이런 성질을 활용하여 함수를 이름공간으로 활용 할 수 있다.

```javascript
var x = "global x";
(function(){
    var x = "local x";
    var y = "local y";
})();
console.log(x); // global x
console.log(y); // Uncaught ReferenceError: y is not defined
```

