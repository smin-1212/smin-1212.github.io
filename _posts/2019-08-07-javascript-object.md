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

* 생성자의 프로토타입 객체에 메서드를 추가하면 인스턴스에 메서드를 추가하지 않아도 인스턴스가 프로토타입 객체의 메서드를 사용할 수 있으므로 메모리의 낭비를 피할 수 있다.


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

* Ball 생성자의 프로퍼티 추가, 애니메이션

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>볼 애니메이션</title>
        <script>
            window.onload = function(){
                // 각종 매개변수
                var NBALL = 200; // 공의 개수
                var R = 5;             // 공의 반지름
                var TIME_INTERVAL = 33;  // 애니메이션의 시간간격 (ms 단위)
                var BACK_ALPHA = 0.3; // 배경의 alpha 값
                // canvas 의 렌더링 컨텍스트 가져오기
                var canvas = document.getElementById('mycanvas');
                var ctx = canvas.getContext('2d');
                // 벽을 표현하는 객체
                var wall = { left: 0, right: canvas.width, top:0, bottom: canvas.height};
                // 공 객체를 NBALL개 만들어 배열 balls 에 저장
                var balls = [];
                for(var i =0 ; i<NBALL; i++){
                    balls[i] = new Ball(wall.right/2, wall.bottom/2, R);
                    balls[i].setVelocityAsRandom(2,7).setColorAsRandom(50,255);
                }
                // 애니매이션 실행 : TIME_INTERVAL(ms) 마다 drawFrame 을 실행
                setInterval(drawFrame, TIME_INTERVAL);
                // 애니매이션의 프레임 그리기
                function drawFrame(){
                    ctx.fillStyle = 'rgba(0,0,0,'+BACK_ALPHA+')';
                    ctx.fillRect(0,0,canvas.width, canvas.height);
                    for(i = 0 ; i<balls.length; i++){
                        balls[i].move().callisionWall(wall).draw(ctx);
                    }
                }
            };

            function Ball(x,y,r,vx,vy,color){
                this.x = x;
                this.y = y;
                this.r = r;
                this.vx = vx;
                this.vy = vy;
                this.color = color;
            }

            Ball.prototype = {
                setVelocityAsRandom : function(vmin, vmax){
                    var v = vmin + Math.random() * (vmax - vmin);
                    var t = 2*Math.PI* Math.random();
                    this.vx = v*Math.cos(t);
                    this.vy = v+Math.sin(t);
                    return this;
                }, 
                setColorAsRandom : function(lmin, lmax){
                    var R = Math.floor(lmin + Math.random()*(lmax - lmin));
                    var G = Math.floor(lmin+Math.random()*(lmax - lmin));
                    var B = Math.floor(lmin+Math.random()*(lmax-lmin));
                    this.color = 'rgb('+R+','+G+','+B+')';
                    return this;
                },
                draw : function(ctx) {
                    ctx.fillStyle = this.color;
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.r, 0, 2*Math.PI, true);
                    ctx.fill();
                    return this;
                },
                move: function(){
                    this.x += this.vx;
                    this.y += this.vy;
                    return this;
                },
                callisionWall : function(wall){
                    if(this.x - this.r < wall.left){
                        this.x = wall.left + this.r;
                        if(this.vx < 0 ){
                            this.vx *= -1;
                        }
                    }
                    if(this.x+this.r > wall.right){
                        this.x = wall.right - this.r;
                        if(this.vx > 0 ){
                            this.vx *= -1;
                        }
                    }
                    if(this.y - this.r < wall.top){
                        this.y = wall.top + this.r;
                        if(this.vy < 0){
                            this.vy *= -1;
                        }
                    }
                    if(this.y+ this.r > wall.bottom){
                        this.y = wall.bottom - this.r;
                        if(this.vy > 0){
                            this.vy *= -1;
                        }
                    }
                    return this;
                }
            };
        </script>
    </head>
    <body>
        <canvas id="mycanvas" width="640" height="480"></canvas>
    </body>
</html>
```

--- 

### 프로토타입 상속

* 자바스크립트는 클래스가 아닌 객체를 상속한다.
* 상속은 **프로토타입 체인**이라고 부르는 객체의 자료구조로 구현되어있다.
* **프로토타입 상속**이라고 부른다.

#### 프로토타입 체인
> **내부 프로퍼티 [\[Prototype]]**
> * 모든 객체는 내부 프로퍼티 **[\[Prototype]]** 을 가지고 있다.
> * 함수 객체의 **prototype 프로퍼티와는 다른 객체이다.**
> * ECMAScript 6 부터 **\_\_proto__** 프로퍼티에 **[\[Prototype]]** 의 값이 저장된다.
> * 객체의 \_\_proto__ 프로퍼티는 그 객체에게 상속을 해 준 부모 객체를 가리킨다.
> * 따라서 객체는 \_\_proto__ 프로퍼티가 가리키는 부모 객체의 프로퍼티를 사용할 수 있다.

```javascript
var objA = {
    name: "Tom",
    sayHello: function() {
        console.log("Hello! " + this.name);
    }
};
var objB = {
    name: "Huck"
};
objB.__proto__ = objA;
var objC ={};
objC.__proto__ = objB;
objC.sayHello(); // "Hello! Huck"
```

#### new 연산자의 동작 과정
* 객체의 생성 -> 프로토타입의 설정 -> 객체의 초기화

```javascript
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
}
Circle.prototype.area = function(){
    return Math.PI*this.radius.this.radius;
};

// Circle 생성자로 인스턴스는 아래와 같이 생성
var c = new Circle({x:0, y:0}, 2.0);

// new 연산자로 Circle 생성자를 사용하면 내부적으로는 다음과 같은 작업을 수행
// 1. 빈 객체를 생성
var newObj = {};

// 2. Circle.prototype을 생성된 객체의 프로토타입으로 설정한다.
newObj.__proto__ = Circle.prototype; // 인스턴스의 프로토타입 체인이 정의됨
// 이때 Circle.prototype이 가리키는 값이 객체가 아니라면 Object.prototype을 
// 프로토 타입으로 설정한다.

// 3. Circle 생성자를 실행하고 newObj를 초기화 한다. 이때 this는 1. 로
// 생성한 객체로 설정한다.
Circle. apply(newObj, arguments);

// 4. 완성된 객체를 결괏값으로 반환한다.
return newObj;
```


---

### 프로토타입 객체의 프로퍼티

> * 함수를 정의하면 함수 객체는 기본적으로 prototype 프로퍼티를 갖게 된다.
> * prototype 프로퍼티는 프로토타입 객체를 가리킨다.
> * 기본적으로 constructor 프로퍼티와 내부프로퍼티 \[\[Prototype]](\_\_proto__) 를 가지고 있다.


#### constructor 프로퍼티
constructor 프로퍼티는 **함수 객체의 참조를 값**으로 가지고 있다.

```javascript
function F(){};
console.log(F.prototype.constructor);
// Function F() {}
```

![ex01](https://drive.google.com/uc?id=1bNiXbTPr7RQlzaZRpSS408mJ_bwh_SvI){: width="542px" height="239px"}

* 생성자와 생성자의 프로토타입 객체는 서로를 참조한다.
* '생성자의 prototype 프로퍼티가 프로토타입 객체를 가리키며, 이 프로토타입 객체의 constructor 프로퍼티가 생성자를 가리키는 연결고리'로 묶여있다.
* 생성자로 생성한 인스턴스는 생성될 때의 프로토타입 객체의 참조만 가지고 있고, 생성자와는 직접적인 연결고리가 없다.
* 인스턴스가 어떠한 생성자로 생성된 것인지 알아내는 방법으로 인스턴스가 가진 프로토 타입의 constructor 프로퍼티 값을 확인 하는 방법이 있다.

```javascript
function F() {};
obj = new F();
console.log(obj.constructor); // Function F() {}
```

#### 내부 프로퍼티 \[\[Prototype]]

* 함수 객체가 가진 프로토타입 객체의 내부 프로퍼티 **[\[Prototype]]** 는 기본적으로 Object.prototype 을 가리킨다.
* 프로토타입 객체의 프로토타입은 **Object.prototype** 이다.

```javascript
function F() {};
console.log(F.prototype.__proto__); // Object {} : Object.prototype
```

* 생성자로 생성한 인스턴스가 Object.prototype 의 프로퍼티를 사용 가능

#### 프로토타입 객체의 교체 및 constructor 프로퍼티


* 생성자가 가진 prototype 프로퍼티 값을 새로운 객체로 교체할 때는 주의해야함.
* 프로퍼티만 정의되어 있는 새로운 객체를 prototype 프로퍼티 값으로 대입하면 인스턴스와 생성자 사이의 연결 고리가 끊어지게 된다.
* 그 객체에는 constructor 프로퍼티가 없다.
* 인스턴스와 생성자 사이의 연결고리를 유지하려면 prototype으로 사용할 객체에 constructor 프로퍼티를 정의하고, 그 프로퍼티에 생성자의 참조를 대입해야 한다.
{: .box-warning}

```javascript
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
}
Circle.prototype = {
    constructor: Circle, // 생성자를 constructor로 대입
    area: function() {  
            return Math.PI* this.radius*this.radius;
        }
};
var c = new Circle({x:0, y:0}, 2.0);
console.log(c.constructor); // Function Circle
console.log(c instanceof Circle); // true
```

#### 인스턴스 생성 후에 생성자의 프로토타입을 수정하거나 교체한 경우

* 인스턴스의 프로토타입은 생성자가 인스턴스를 생성할 때 가지고 있던 프로토타입 객체이다.
* 인스턴스를 생성한 후에 생성자의 prototype 프로퍼티 값을 다른 객체로 교체해도 인스턴스의 프로토타입은 바뀌지 않는다.
* 인스턴스의 프로퍼티는 생성되는 시점의 프로토타입에서 상속 받는다.
* 생성된 후에는 생성자의 프로토타입을 바꾸어도 교체한 객체로부터는 프로퍼티를 상속받지 않는다.
* 생성자가 기존에 가지고 있던 프로토 타입 객체에 프로퍼티를 추가한 경우, 생성자와 인스턴스 사이의 연결고리가 끊기지 않는다. -> 생성자에서 정의한 프로퍼티를 인스턴스에서 사용가능

```javascript
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
}
var c = new Circle({x:0, y:0}, 2.0); // 생성
// 프로토타입 자체를 변경한다.
Circle.prototype = {
    constructor: Circle,
    area: function(){
        return Math.PI*this.radius*this.radius;
    }
};
c.area(); // Uncaught TypeError : c.area is not a function
```

```javascript
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
}
var c = new Circle({x: 0, y: 0}, 2.0);
// 프로토타입에 area 메소드추가, Error 발생 없음
Circle.prototype.area = function(){
    return Math.PI*this.radius*this.radius;
};
c.area(); // 12.5.....
```