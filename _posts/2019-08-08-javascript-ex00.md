---
layout: post
title: javascript 객체 심화 2
tags: [javascript, js, es6]
---

### 접근자 프로퍼티 ( getter, setter method )

* **데이터 프로퍼티** : 값을 저장하기 위한 프로퍼티
* **접근자 프로퍼티** : 값이 없음, 프로퍼티를 읽거나 쓸때 호출하는 함수를 값 대신에 지정할 수 있는 프로퍼티
* **접근자** : 객체지향 프로그래밍에서 객체가 가진 프로퍼티 값을 객체 바깥에서 읽거나 쓸 수 있도록 제공하는 메서드.

```javascript
var person = {
    _name :"Tom",
    get name(){ // getter 정의
        return this._name;
    },
    set name(value){ // setter 정의
        var str = value.charAt(0).toUpperCase() + value.substring(1);
        this._name = str;
    }
};
console.log(person.name); 
person.name = "huck";
console.log(person.name); 

// person._name 에 직접 할당 가능

// delete 연산자로 프로퍼티를 삭제 가능하다.
delete person.name;
```

### 데이터 캡슐화
* 즉시실행 함수로 클로저를 생성하면, 데이터를 객체 외부에서 읽고 쓸 수 없도록 숨기고 접근자 프로퍼티로만 읽고 쓰도록 만들 수 있다.

```javascript
var person = (function(){
    var _name = "Tom"; // 즉시 실행 함수의 지역변수, private
    return {
        get name(){
            return _name;
        },
        set name(value){
            var str = value.charAt(0).toUpperCase() + value.substring(1);
            _name = str;
        }
    };
})(); // 즉시실행 함수로 클로저 생성,

// person._name 에 할당 불가
```


---

### 프로퍼티 속성

...