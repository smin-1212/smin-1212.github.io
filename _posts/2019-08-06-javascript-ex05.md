---
layout: post
title: 클로저
tags: [javascript, js, es6]
---

### 클로저

클로저는 아래와 같은 동작을 하는 함수와 그 기능을 구현한 자료 구조의 모음이다.
> 자기 자신이 정의된 환경에서 함수 안에 있는 자유 변수의 식별자 결정을 실행한다.

```javascript
var a = "A";
function f(){
    var b = "B";             
    function g(){    // 열린 함수
        var c = "C";
        console.log(a+b+c);
    }
    g();
}
f();
```


중첩 함수 g는 열린 함수 이다. 그러나 유효 범위 체인으로 주변 환경의 변수 b와 a를 들여와서
실질적으로는 폐쇄 함수가 되었다. 열려있는것을 닫는다는 의미로 클로저.

{: .box-note}


클로저는 함수 객체와 렉시컬 환경 컴포넌트의 집합이다.

---

### 클로저의 성질

카운터 함수를 만드는 함수

```javascript
function makeCounter(){
    var count = 0; // 클로저의 내부 상태로서 저장된다.
    return f;
    function f(){
        return count++;
    }
}

var counter = makeCounter();
console.log(counter());   // 0
console.log(counter());   // 1
console.log(counter());   // 2
```

외부 함수 makeCounter 는 중첩 함수 f의 참조를 반환한다.

중첩 함수 f는 외부 함수 makeCounter의 지역변수 count를 참조한다.

```javascript
var counter1 = makeCounter();
var counter2 = makeCounter();
console.log(counter1());   // 0
console.log(counter2());   // 0
console.log(counter1());   // 1
console.log(counter2());   // 1
```

보통은 이름이 없는 **익명 함수**를 반환하는 방법을 자주 사용한다.

위의 makeCounter() 는 아래와 같이 변경가능하다.

```javascript
function makeCounter(){
    var count = 0;
    return function(){
        return conter++;
    };
}

var counter = makeCounter();

console.log(counter());
console.log(counter());
console.log(counter());
```

### 클로저 이해를 위한 핵심 사항
* 외부 함수를 호출하면 그 함수의 렉시컬 환경 컴포넌트가 생성된다.
* 중첩된 중첩 함수의 함수 객체를 생성해서 반환한다.
* 외부 함수의 렉시컬 환경 컴포넌트를 참조하는 중첩 함수가 정의한 클로저가 생성된다.
* 외부 함수는 클로저를 생성하는 팩토리 함수라고 할 수 있다.
* 외부 함수가 속한 렉시컬 환경 컴포넌트는 클로저 내부 상태 자체이다.
* 외부 함수가 호출될 때 마다 클로저는 새로 생성된다.
* 중첩 함수의 함수 객체가 있는 한 외부 함수가 속한 렉시컬 환경 컴포넌트는 지워지지 않는다.
* 외부 함수의 함수 객체가 사라져도 지워지지 않는다.
* 클로저 내부상태(외부함수의 지역변수, 선언적 환경 레코드)는 외부로 부터 은폐되어 있으며 중첩함수 안에서만 읽거나 쓸 수 있다.

