---
layout: post
title: javascript ECMAScript 6 추가 된 함수의 기능
tags: [javascript, js, es6]
---

### 화살표 함수 표현식으로 함수 정의

* ECMAScript 6 부터 추가된 화살표 함수 표현식은 함수리터럴(익명 함수)의 단축 표현
* **함수 리터럴과 완전히 같은것은 아님**
* 화살표 함수 표현식으로 정의한 함수를 **화살표 함수**라고 한다.

#### 화살표 함수 표현식의 작성법

```javascript
// 기존의 함수 리터럴로 함수 정의
var square = function(x) { return x*x; };

// 화살표 함수 표현식으로 정의
var square = (x) => { return x*x; };

// 인수가 여러개 있으면 인수와 인수를 쉼표로 구분
var f = (x,y,z) => {...};

// 인수가 하나만 있으면 인수를 묶는 괄호를 생략 가능함
var square = x => { return x*x; };

// 인수가 없으면 인수를 묶는 괄호를 생략할 수 없음
var f = () => {...};

// 함수 몸통 안의 문장이 return 만 있으면 중괄호와 return 키워드 생략 가능
var square = x => x*x;

// 함수 몸통 안에 return 문장만 있더라도 함수의 반환값이 객체 리터럴이면,
// 객체 리터럴을 그룹 연산자인 () 로 묶어야 함
var f = (a, b) => ( {x:a, y:b } );

// 화살표 함수도 즉시 실행 함수(IIFE) 로 사용할 수 있음
(x => x*x )(3); // 9
```

#### 화살표 함수와 함수 리터럴의 차이점

* 화살표 함수는 this 값이 **함수를 정의할 때 결정**된다.

```javascript
// 함수 리터럴로 정의한 함수의 this 값은 함수를 호출할 때 결정된다.
// 화살표 함수의 this 값은 함수를 정의할때 결정된다.
var obj = {
    say: function(){
        console.log(this);    // [object Object]
        var f = function() {
            console.log(this);   // [object Window]
        };
        f();
        var g = () => console.log(this);  // [object Object]
        g();
    }
};
obj.say();

// 화살표 함수는 call, apply 메서드를 사용하여 this를 바꾸어 호출해도 
// this 값이 변하지 않는다.
var f = function(){
    console.log(this.name);
};
var g = () => console.log(this.name);

var tom = {name : "Tom" };
f.call(tom);  // "Tom"
g.call(tom);  // ""
```

* arguments 변수가 없다.

```javascript
// 화살표 함수 안에는 arguments 변수가 정의되어 있지 않아 사용 불가
var f = () => console.log(arguments);
f(); // ReferenceError : arguments is not defined
```

* 생성자로 사용할 수 없다.

```javascript
// 화살표 함수 앞에 new 연산자를 붙여서 호출이 불가
var Person = (name, age) => {
    this.name = name;
    this.age = age;
    };
var tom = new Person("Tom", 17); // TypeError: Person is not a constructor
```

* yield 키워드 사용이 불가

---

### 인수에 추가된 기능

#### 나머지 매개변수

* ECMAScript 6 부터 함수의 인자가 들어가는 부분에 ...을 입력하면 그만큼의 인수를 배열로 받는다.

* ...으로 표현한 인자를 가리켜 나머지 매개변수(rest parameters)라고 부른다.

```javascript
// ...args -> 나머지 매개변수
function f(a,b, ...args){
    console.log(a,b,args);
}
// 나머지 매개변수는 배열로 받음
f(1,2,3,4,5,6); // 1 2 [3, 4, 5, 6]

// 화살표 함수 안에서는 arguments 를 사용할 수 없지만,
// 나머지 매개변수를 이용하여 화살표 함수 안에서도 가변인수를 사용 가능함.
var sum = (...args) => {
    for(var i =0, s=0; i<args.length; i++){
        s+=args[i];
    };
    return s;
};

sum(1,2,3,4,5);
```

#### 인수의 기본값

* 함수의 인자에 대입연산자를 사용해서 기본값을 설정 할 수 있음
* 기본값을 설정한 인자에 호응하는 인수를 생략하거나 undefined 를 넘기면 대입 연산자 우변의 값이 기본값이된다.

```javascript
// b 는 기본값이 1이다.
function multiply(a, b=1){
    return a*b;
}
multiply(3);
multiply(3,2);

// 다른 인자의 값도 기본값으로 사용 가능함
function add(a, b=a+1){
    return a+b;
}
add(2);     // 5
add(2, 1);  // 3
```

---

### 이터레이터와 for/of 문

#### 이터레이션(iteration)
* 이터레이션(iteration)은 반복 처리라는 뜻, 데이터 안의 요소를 연속적으로 꺼내는 행위
* 배열의 forEach 메서드는 배열의 요소를 순차적으로 검색하여 그 값을 함수의 인수로 넘기기를 반복

```javascript
var a = [5,4,3];
a.forEach(function(val) {console.log(val); });

// 5
// 4
// 3
```

#### 이터레이터(iterator)
* 이터레이터(iterator)는 반복처리(iteration)가 가능한 객체를 말하며, 반복기라고 한다.
* **반복처리를 단계별로 제어 가능하다.**

```javascript
var a = [5,4,3];
var iter = a[Symbol.iterator]();
// next() 를 호출할때마다 이터레이터 리절트(iterator result)객체가 반환됨
// iterator result 는 value 와 done 프로퍼티를 갖는 객체임
console.log(iter.next()); // Object { value: 5, done: false}
console.log(iter.next());
console.log(iter.next());
console.log(iter.next()); // Object { value: undefined, done: true }
```

이터레이터는 아래의 두가지 항목을 만족하는 객체이다.

* next 메서드를 가진다.
* next 메서드의 반환값은 value 프로퍼티와 done 프로퍼티를 가진 객체이다. 이때 value에는 꺼낸 값이 저장되고 done 에는 반복이 끝났는지를 뜻하는 논리값이 저장됨

#### 반복 가능한 객체와 for/of 문

* 이터레이터를 사용해서 이터레이션을 하려면 적절한 처리를 직접 작성해야 한다.
* for/of 문을 사용하면 반복 처리를 자동으로 하도록 만들 수 있다.

```javascript
// 이터레이터만 사용하여 반복
var a = [5,4,3];
var iter = a[Symbol.iterator]();
while(true){
    var iteratorResult = iter.next();
    if(iteratorResult.done == true){
        break;
    }
    var v = iteratorResult.value;
    console.log(v);
}

// for/of 문 사용하여 반복
var a = [5,4,3];
for(var v of a){
    console.log(v);
}
```

for/of 문은 아래의 두가지 조건을 만족하는 객체를 반복 처리한다.
* Symbol.iterator 메서드를 가지고 있다.
* Symbol.iterator 메서드는 반환값으로 이터레이터를 반환한다.

Array, String, TypedArray, Map, Set 등이 반복 가능한 객체이다.

반복 가능한 객체는 for/of문, 전개 연산자, yield*, 비구조화 할당 등에 활용이 가능하다.

이터레이터 객체와 이터러블 객체는 다른 개념이다.

---

### 제너레이터

제너레이터는 아래와 같은 성질을 지닌함수이다.
> * 반복 가능한 이터레이터를 값으로 반환한다.
> * 작업의 일시 정지와 재시작이 가능하며 자신의 상태를 관리한다.

#### 제너레이터의 정의와 실행
> * 제너레이터는 function* 문으로 정의한 함수
> * 하나 이상의 yield 표현식을 포함한다.

```javascript
function* gen(){
    yield 1;  // 포인트 1
    yield 2;  // 포인트 2
    yield 3;  // 포인트 3
}
var iter = gen();
console.log(iter.next()); // Object { value: 1, done: false }
console.log(iter.next()); // Object { value: 2, done: false }
console.log(iter.next()); // Object { value: 3, done: false }
console.log(iter.next()); // Object { value: undefined, done:true }
```

**실행 과정**
> 1. 제너레이터 함수 gen은 **호출해도 바로 실행하지 않는다.** 대신 **이터레이터를 반환**한다.
> 2. 이터레이터의 next() 메서드가 호출되면 함수의 첫 번째 yield 연산자의 위치까지 실행하며, 결과값으로 이터레이터 리절트를 반환한다. 이터레이터 리절트의 value프로퍼티 값으로 yield 표현식에 지정한 값을 저장하고, done 프로퍼티 값으로 제너레이터 함수를 끝까지 실행했는지 저장한다. 이때 제너레이터 함수의 내부처리는 **포인트 1** 에서 **일시정지 상태**가 된다.
> 3. 또다시 이터레이터의 next() 메소드가 호출되면 **일시 정지한 위치에 있는 처리를 재개**한다. 그리고 제너레이터 함수 안의 다음 번 yield 연산자의 위치(**포인트 2**)까지 실행한다. 마찬가지로 value 프로퍼티와 done 프로퍼티를 가진 이터레이터 리절트를 반환하고 처리를 **일시 정지**한다.
> 4. 이터레이터의 next 메서드가 호출된 후 멈추었던 처리를 재개한다. 그리고 제너레이터 함수 안에 있는 다음 yield연산자의 위치까지 실행한다. 마찬가지로 value 프로퍼티와 done프로퍼티를 가진 이터레이터 리절트를 반환하고 처리를 일시 정지한다.
> 5. 이터레이터의 next 메서드가 호출되어 함수 처리가 마지막 yield 에 도착, value 프로퍼티 값이 undefined고 done 프로퍼티 값이 true 인 이터레이터 리절트를 반환한다.

**yield** 는 프로그램이 일시적으로 정지하는 위치이다.

제너레이터로 생성한 이터레이터의 next 메서드는 제너레이터 함수의 상태를 일시 정지 상태에서 실행 상태로 바꾸는 역할을 한다.

```javascript
// yield 키워드 사용법
yield;
yield 표현식;

// yield 표현식은 yield에 지정한 표현식을 값으로 가지며 이 자체를 변수에 대입 가능
var a = yield 2; // a 값은 2가 된다.

// 제너레이터로 생성한 이터레이터는 반복 가능하며, for/of 문으로 반복해서 처리 가능
for(var v of iter){
    console.log(v); // 1, 2, 3 을 순서대로 표시한다.
}
```

```javascript
// 제너레이터 활용예제
// m부터 n까지의 정수 값을 순서대로 꺼내는 이터레이터를 생성하는 제너레이터
function* createNumbers(from, to){
    while(from<=to){
        yield from++;
    }
}
var iter = createNumbers(10,20);
for(var v of iter){ // 10~20 사이의 정수를 순서대로 출력한다.
    console.log(v);
}
```



* 무작위 행보 시뮬레이터

```html
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>무작위 행보</title>
        <script>
            function* randomWalk(c, x0, y0, d){
                var dx = [1,0,-1,0], dy = [0,1,0,-1]; // 이동 방향
                var x = x0;
                var y = y0;
                c.strokeStyle = "red";
                c.globalAlpha = 0.25;
                while(true){
                    yield;
                    c.beginPath();
                    c.moveTo(x,y);
                    var dir = Math.floor(Math.random()*4); // 0~3 사이의 난수
                    x += d*dx[dir];
                    y += d*dy[dir];
                    c.lineTo(x,y);
                    c.stroke();
                }
            }
            window.onload = function(){
                var canvas = document.getElementById("mycanvas");
                var ctx = canvas.getContext("2d");
                var iter = randomWalk(ctx, 300, 400, 4, "red");
                setInterval(function(){iter.next();}, 10);
            };
        </script>
        <style>
            #mycanvas {border:1px solid gray; }
        </style>
    </head>
    <body>
        <canvas id="mycanvas" width=600 height=600 ></canvas>
    </body>
</html>
```

#### 제너레이터에 값 넘기기
* 제너레이터로 생성한 이터레이터의 next 메서드에 값을 대입하면 제너레이터에 값을 넘길 수 있음.
* next 메서드에 넘긴 값은 제너레이터가 일시적으로 정지하기 직전의 yield 표현식의 값으로 사용됨.
* 제너레이터의 내부상태를 외부에서 변경 가능

```javascript
// 피보나치 수열 열거하기
// 피보나치 수열을 다시 설정할 때 next 메서드에 값을 넘긴다.
function* fibonacci() {
    var fn1 =0, fn2 = 1;
    while(true){
        var fnew = fn1 + fn2;
        fn1 = fn2;
        fn2 = fnew;
        reset = yield fn1;
        if(reset){
            fn1 = 0;
            fn2 = 1;
        }
    }
}
var iter = fibonacci();
for(var i = 0 ; i<10; i++){
    console.log(iter.next().value);
}   // 1, 1, 2, 3, 5, ... , 55 순으로 출력
console.log(iter.next().value);     // 89
console.log(iter.next(true).value); // 1
```

* 실행 결과
![ex01](https://drive.google.com/uc?id=19caGIfgSA0O-MfJct7V2_lAL8OomO0RH){: width="267px" height="751px"}
