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

--- 

#### 클로저 활용

* 여러개의 내부 상태와 메서드를 가진 클로저


```javascript
// 사람 데이터를 저장하는 클로저를 생성하는 함수
function Person(name, age){
    var _name = name;
    var _age = age;
    return {
        getName : function(){ return _name;},
        getAge : function(){ return _age;},
        setAge : function(x){ _age = x;}
    };
}

var person = Person("Tom", 19);
```


* 함수 팩토리


```javascript
// 다양한 매개변수를 받는 함수를 여러개 생성
function makeMultiplier(x) {
    return function(y){
        return x*y;
    };
}
var multi2 = makeMultiplier(2);
var multi10 = makeMultiplier(10);

console.log(multi2(3));      // 6
console.log(multi10(10));    // 30
```


* 초기화 기능이 추가된 함수 생성하기


```javascript
function Prime(n) {
	// 에라토스테네스 체로 2~n 사이의 소수 구하기
    var p = [];
    for(var i=2; i<=n; i++) {
        p[i] = true;
    }
    var max = Math.floor(Math.sqrt(n));
    var x = 2;
    while(x<=max){
        for(var i = 2*x; i<=n ; i+=x){
            p[i] = false;
        }
        while(!p[++x]);
    }
    // 소수만 꺼내 배열 primes에 저장
    var primes = [], nprimes = 0;
    for(var i = 2; i<=n; i++){
        if(p[i]) {
            primes[nprimes++] = i;
        }
    }
    p = null; // 필요 없어진 배열을 메모리에서 해제
    // 소수 m개를 무작위로 선택하여 곱한값을 반환하는 함수를 반환
    return function(m){
        for(var i =0, product=1; i<m; i++){
            product *= primes[Math.floor(Math.random()*nprimes)];
        }
        return product;
    };
}

var primeProduct = Prime(100000);
console.log(primeProduct(2));
console.log(primeProduct(2));
```