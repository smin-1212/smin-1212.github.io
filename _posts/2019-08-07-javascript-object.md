---
layout: post
title: javascript 객체 심화
tags: [javascript, js, es6]
---

### 객체의 생성

```javascript
// 객체 리터럴로 생성
var card = { suit: "하트", rank: "A" };

// 생성자로 생성하는 방법
function Card(suit, rank){
    this.suit = suit;
    this.rank = rank;
}
var card = new Card("하트", "A");

// Object.create로 생성하는 방법
var card = Object.create(Object.prototype, {
    suit: {
        value: "heart",
        writable: true,
        enumerable: true,
        configurable: true
    },
    rank: {
        value: "A",
        writable: true,
        enumerable: true,
        configurable: true
    }
});
```

--- 

### 프로토 타입

#### 생성자 안에서 메서드를 정의하는 방식의 문제점
* 생성자 안에서 this뒤에 메서드를 정의하면 그 생성자로 생성한 모든 인스턴스에 똑같은 메서드가 추가된다.
* 따라서, 메서드를 포함한 생성자로 인스턴스를 여러 개 생성하면 같은 작업을 하는 메서드를 인스턴스 개수만큼 생성하게 되며 결과적으로 그만큼의 메모리를 소비한다.

```javascript
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
    this.area = function(){
        return Math.PI*this.radius*this.radius;
    };
}
// 각각의 인스턴스가 똑같은 메서드 area 를 공유한다.
// 메소드 area 를 소유,,, 실행 컨텍스트 영역을 각각 가진다...?
var c1 = new Circle({x:0, y:0}, 2.0);
var c2 = new Circle({x:0, y:1}, 3.0);
var c3 = new Circle({x:1, y:0}, 1.0);

// 프로토 타입을 정의하여 문제 해결
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
}
// Circle 생성자의 prototype 프로퍼티에 area 메서드를 추가
Circle.prototype.area = function(){
    return Math.PI*this.radius*this.radius;
}
// 인스턴스 c1, c2, c3 에는 area 메서드가 존재하지 않지만, area메서드를 사용가능함
var c1 = new Circle({x:0, y:0}, 2.0);
var c2 = new Circle({x:0, y:1}, 3.0);
var c3 = new Circle({x:1, y:0}, 1.0);

console.log("넓이 = " , c1.area());
```

