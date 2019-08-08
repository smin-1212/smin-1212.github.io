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



