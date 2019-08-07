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

#### 함수에 프로퍼티 추가하기

* 다른 객체와 마찬가지로 함수에도 프로퍼티를 추가 할 수 있음.

```javascript
function f(x){...}
f.p = a;
f.g = function() {...};
```

Function 객체에 추가된 프로퍼티는 그 함수를 실행하지 않아도 읽거나 쓸 수 있다.

함수의 프로퍼티로 작성하면 함수 객체가 이름공간의 역할을 하기 때문에 문제 발생하지 않는다.


### 고차 함수

**고차함수** : 함수를 인수로 받는 함수, 함수를 반환하는 함수

* 고차 함수를 이용하여 작업을 한 곳에 모아 추상화를 하면 프로그램의 가독성과 유지 보수성을 향상시킬 수 있다.
* 함수형 프로그래밍을 할 때 자주 사용, map, filter, reduce 등의 배열 메서드 사용

* ex) 패턴이 같은 작업을 고차함수로 정리
```javascript
 // 변환 전
 // 수열을 표시하는 프로그램
 digits = "";
 for(var i = 0 ; i <10 ; i++){
     digits += i; // 공통로직
 }
 console.log(digits);

 // 변환 전
 // 무작위 알파벳 문자열을 표시하는 프로그램
 randomChars = "";
 for(var i = 0 ; i < 8 ; i++){
     randomChars += String.fromCharCode(Math.floor(Math.random()*26) + "a".charCodeAt(0)); // 공통 로직
 }
 console.log(randomChars);

 // 이하 공통부분 추출
function joinString(n, f){
    var s="";
    for(var i =0; i<n;i++){
        s += f(i); // 전달받은 함수인자 f()
    }
    return s;
}

// 이하 공통 로직 적용 후
var digits = joinString(10, function(i) { return i; });
var randomChars = joinString(8, function(i)){
    return String.fromCharCode(Math.floor(Math.random()*26) + "a".charCodeAt(0));
}
console.log(digits);
console.log(randomChars);
```

#### 메모이제이션

* 함수를 호출했을 때의 인수와 반환값을 한 쌍으로 만들어 저장해 두는 기법

함수에 메모이 제이션을 적용해 두면 한 번 건네받은 이력이 있는 인수의 결괏값으로 저장해 둔 결과를 반환, **추가적인 계산 생략이 가능**

```javascript
// 피보나치 수열을 구하는 함수
function fibonacci(n){
    if(n<2) {
        return n;
    }
    if(!(n in fibonacci)){
        fibonacci[n] = fibonacci(n-1) + fibonacci(n-2);
    }
    return fibonacci[n];
}

for(var i =0; i <= 20; i++){
    console.log((" " +i).slice(-2) + ":" + fibonacci(i));
}
```


```javascript
// 인수로 받은 함수의 실행 결과를 객체 cache 안에 저장한다.
// 인수로 받은 함수를 같은 인수로 실행하면 실제 계산을 하는 대신
// cache에 저장된 값을 반환하는 함수가 만들어진다.
// memorize 함수의 중첩 함수는 클로저를 생성한다.
// 따라서 memorize의 지역 변수인 cache 는 클러저 내부 상태로 저장된다.
function memorize(f){
    var cache = {};
    return function(x){
        if(cache[x] == undefined) cache[x] = f(x);
        return cache[x];
    };
}

function isPrime(n){
    if(n<2) return false;
    var m = Math.sqrt(n);
    for(var i=2; i<=m ; i++){
        if(n%i == 0 ){
            return false;
        }
    }
    return true;
}

var isPrime_memo = memorize(isPrime);
var N = 1000;
for(var i = 2; i<=N; i++){
    isPrime_memo(i);
}

for(var i = 2 ; i+2<=N ; i++){
    if(isPrime_memo(i) && isPrime_memo(i+2)){
        console.log(i + ","+(i+2));
    }
}
// 재귀 함수에 메모이 제이션을 적용하려면 원래 함수를 재귀호출하는 대신
// 메모이제이션 된 함수를 재귀 호출하도록 만들어야 한다.
```
