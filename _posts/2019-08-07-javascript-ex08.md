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


