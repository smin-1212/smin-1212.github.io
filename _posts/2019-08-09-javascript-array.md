---
layout: post
title: javascript 배열 심화
tags: [javascript, js, es6]
---

### 배열의 메서드

* 배열은 Array 타입 객체이며 Array.prototype의 프로퍼티를 상속받는다.
* Array.prototype에는 수많은 메서드가 정의되어 있으며, 모든 배열은 이 메서드를 사용 가능하다.

---

#### map 메서드
* 인수로 받은 함수를 배열의 요소별로 한 번씩 실행하며, 마지막에는 그 함수가 반환한 값으로 새로운 배열을 생성한다.
* 인수로 넘긴 함수에는 인수 세 개(value, index, array)가 전달된다.
* map메서드의 인수로 넘기는 함수는 반드시 **값을 반환**해야 한다.

```javascript
// 배열의 각 요소를 두 배 곱한 배열을 생성하는 예
var a = [1,2,3,4,5];
var b = a.map(function(x) {
        return 2*x; 
    }); // b=[2,4,6,8,10]

// 배열의 요소별 제곱근을 배열로 생성
var a = [1,4,9,16,25];
var b = a.map(Math.sqrt); // b = [1,2,3,4,5]

// 객체 배열의 요소 순회, 특정 프로퍼티 값을 꺼내어 배열로
var persons = [
    {name:"Tom", age: 17},
    {name:"Huck", age: 18},
    {name:"Becky", age: 16}
];
var names = persons.map(person => person.name);
var ages = persons.map(person => person.age);
console.log(names); // ["Tom", "Huck", "Becky"]
console.log(age);   // [17,18,16]

// Array.prototype의 메서드를 메서드 체인으로 연결해서 배열 처리가 가능
// persons 객체 배열, 이름을 배열로 만든 후 이름별로 문자의 개수를 구하는 코드
person.map(person => person.name).map(name => name.length);
// [3, 4, 5]
```


#### reduce 메서드
* 배열의 첫 번째 요소부터 마지막 요소까지 합성 곱(convolution) 처리를 한다.
* **합성곱(convolution) 처리** : 배열 요소 하나를 함수로 처리한 후, 반환값을 다음 번 요소를 처리할 때 함수의 입력값으로 사용하는 처리 방법
* reduce 메서드는 마지막 요소를 처리한 함수가 값을 반환

```javascript
// reduce 메서드로 배열 요소의 합을 구하는 예
var a = [1,2,3,4,5];
a.reduce(function(prev, value){
    return prev + value;
}, 0); // 15
a.reduce(function(prev, value){
    return prev + value;
});  // 15

// reduce 메서드로 배열 요소의 곱과 최댓값 구하기
var a = [1,2,3,4,5];
a.reduce(function(p,v){
    return p*v;
}, 1);
a.reduce(function(p,v){
    return (p>v) ? p:v;
});

// 배열의 요소를 공백문자로 연결하기
var a = ["Tom", "Huck", "Becky"];
a.reduce(function(p,v){
    return p + "  " + v; // "Tom Huck Becky"
});
```


```javascript
// 배열의 모든 순열 목록을 구하는 함수

function permutation(a) {
    // 순열 목록(list)을 각 순열의 요소(element)로 추가하면서 갱신해 나간다.
    return a.reduce(function(list, element){
        var newlist = [];
        list.forEach(function(seq){
            for(var i = seq.length; i>=0; i--){
                var newseq = [].concat(seq);
                newseq.splice(i, 0, element);
                newlist.push(newseq);
            }
        });
        return newlist;
    }, [[]] );//초기값은 빈 배열이 저장된 배열
}
var a = [1, 2, 3];
permutation(a).forEach(function(v) {
    console.log(v);
});
```


---


### 유사 배열 객체
* 배열은 아니지만 배열로 처리할 수 있는 객체
* 예를들어 함수의 인수를 저장한 Arguments 객체, DOM의 document.getElementsByTagName메서드, document.getElementByName 메서드 등이 반환하는 NodeList

```javascript
// 유사배열 객체는 직접 생성이 가능하다
var a = {}; // 빈 객체 생성
for(var i = 0 ; i<10; i++){
    a[i] = i; // 객체에 Array 처럼 index 번호로 값을 대입
}
a.length = 10;

for(var i = 0, sum =0; i<a.length; i ++ ){
    sum += a[i];
}
console.log(sum);
```

---

### 비구조화 할당

#### 비구조화 할당(Destructuring)은 배열, 객체, 반복 가능 객체에서 값을 꺼내어 그 값을 별도의 변수에 대입하는 문장.


* 배열의 비구조화 할당 ( Array Destructuring )

```javascript
var [a,b] = [1,2]; // var a=1, b=2; 와 같다.
// 변수 a와 b를 선언한 후 우변의 배열에서 요소를 하나씩 꺼내어 인덱스 순서대로 a,b에 대입

// 이미 선언된 변수를 비구조화 할당하는 예
[a,b] = [2*a, 3*b]; // a = 2*a, b=3*b 와 같음
[a,b] = [b,a];      // a 값과 b 값을 교환함

// 우변과 좌변의 크기가 같지 않으면 undefined 가 자동으로 할당된다.
[a,b,c] = [1,2];      // a=1, b=2, c=undefined 
[a,b] = [1,2,3];      // a=1, b=2, 3은 무시됨
[,a,,b] = [1,2,3,4];  // a=2, b=4 와 같음

// 나머지 요소, 전개연산자(...)를 사용하여 나머지 요소를 이용할 수 있음
[a,b, ...rest] = [1,2,3,4]; // a= 2, b= 2, rest = [3,4] 와 같음

// 요소의 기본값 설정 가능
[a=0, b=1, c=2] = [1,2]; // a=1,b=2, c=2 와 같다.

// 함수가 배열로 반환한 값을 비구조화 할당받기
// 2차원 좌표점(x,y)를 원점 기준으로 각도 theta만큼 회전하기
function rotation(x, y, theta){
    var s = Math.sin(theta), c = Math.cos(theta);
    return [c*x - s*y, s*x + c*y];
}
var [a,b] = rotation(1,2,Math.PI/3);
```

* 객체의 비구조화 할당 ( Object Destructuring )
#### 좌변에는 객체 리터럴과 비슷한 문법을 사용한다.

```javascript
var {a: x, b:y} = {a:1, b:2}; // x=1, y=2 와 같음

// 이미 선언된 변수를 비구조화 할당
{a:x, b:y} = {a:y, b:x}; // x값과 y 값을 교환한다.

// 좌변의 변수에 호응하는 프로퍼티 이름이 오른쪽에 없으면 그 변수에는 undefined가 할당
{a:x, b:y} = {a:1, c:2}; // x=1, y=undefined와 같다.

// 우변에 값이있지만, 그에 대응하는 이름의 변수가 좌변에 없으면 무시됨
{a: x, b: y} = {a: 1, b: 2, c: 3}; // x=1, y=2 와 같음. 3은 무시됨

// Math객체의 프로퍼티를 변수에 대입
var {sin:sin, cos:cos, tan:tan, PI:PI} = Math;
// var sin = Math.sin, cos = Math.cos, tan = Math.tan, PI = Math.PI 와 같다.
// Math.sin(Math.PI /3) -> sin(Math.PI/3) 으로 간결하게 표현 가능

// 프로퍼티의 기본값
{a: x=1, b: y=2, c: z=3} = {a:2, b:4}; // x=2, y=4, z=3과 같음

// 프로퍼티 이름 생략하기
{a, b} = {a:1, b: 2}; // {a:a, b:b} = {a:1, b:2} 와 같음
var {sin, cos, tan, PI} = Math;
// var sin = Math.sin, cos=Math.cos ... 과 같음

// 프로퍼티 이름을 생략한 상태에서도 기본값 지정이 가능함
{a=1, b=2, c=3} = {a:2, b:4}; // a=2, b=4, c=3과 같음
```


* 반복 가능한 객체의 비구조화 할당

```javascript
// 우변에 반복 가능한(이터러블한) 객체가 있을 때도 비구조화 할당을 할 수 있음.
// 좌변은 배열 리터럴과 비슷한 문법을 사용

var [a,b,c] = "ABC";   // var a = "A", b = "B", c = "C" 와 같음
function* createNumbers(from, to){
    while(from <= to) {
        yield from++;
    }
}
var [a,b,c,d,e] = createNumbers(10,15);
// a = 10, b = 11, c = 12, d = 13, e = 14
```


* 중첩된 자료 구조의 비구조화 할당

```javascript
var[a,[b,c]] = [1,[2,3]]; // var a = 1, b = 2 , c = 3 과 같다.
```


---


### 전개 연산자 ( spread operator )

* "..." 은 전개 연산자 이다.
* 전개 연산자는 반복 가능한(이터러블한) 객체를 반환하는 표현식 앞에 표기한다.
* 반복 가능한 객체를 배열 리터럴 또는 함수의 인수 목록으로 펼칠 수 있다.

```javascript
[..."ABC"] // ["A", "B", "C"]
f(..."ABC") // f("A", "B", "C") 와 같다.
[1, ...[2,3,4],5] // [1,2,3,4,5]
f(...[1,2,3])    // f(1,2,3) 과 같다.

// 제너레이터가 만든 이터레이터를 배열 리터럴 안에 펼치는 예
function* createNumbers(from, to){
    while(from <= to){
        yield from++;
    }
}
var iter = createNumbers(10,15);
[...iter] // [10,11,12,13,14,15]

// 배열 두 개를 연결할 때, 전개연산자 사용
var a = ["A","B","C"];
a.push(...["D","E"]);  // ["A", "B", "C", "D", "E"]

// 배열안의 최대값을 Math.max로 구한다.
var a = [5,2,3,7,1];
Math.max(...a); // 7

```

---

### ArrayBuffer 와 형식화 배열

* ArrayBuffer, DataView, 형식화 배열(TypedArray) 은 연속된 데이터 영역(버퍼)을 조작하기 위해 만들어진 객체임, 웹브라우저에서 WebGL 3차원 그래픽을 구현할 용도로 만들어짐

#### ArrayBuffer
* ArrayBuffer 생성자는 메모리에 고정 길이를 가진(길이가 변하지 않는) 인접한 영역(버퍼)을 확보한다.
* ArrayBuffer는 메모리에 영역을 확보하는 역할만 할 뿐 버퍼를 조작하는 메서드는 제공하지 않는다.

```javascript
// 메모리에 1024바이트의 버퍼(ArrayBuffer 객체)를 만든다.
var buffer = new ArrayBuffer(1024);

// 만들어진 버퍼의 크기 구하기 
console.log(buffer.byteLength); // 1024
// byteLength 프로퍼티는 읽기 전용이며 수정 불가하다.

```

#### 형식화 배열 ( TypedArray )
* 배열은 Array 객체이다. 편리하지만 배열 요소접근이 다소 느리다.
* 형식화 배열( TypedArray ) 객체는 ArrayBuffer 가 확보한 버퍼를 데이터의 저장장소로 이용하여 데이터의 빠른 읽기와 쓰기를 구현하였다.