---
layout: post
title: javascript 생성자
tags: [javascript, js, es6]
---

### 생성자 함수

#### 생성자로 객체 생성하기
{% highlight javascript linenos %}
function Card(suit, rank){
    this.suit = suit;
    this.rank = rank;
}

// 생성자로 객체 생성하기, new 연산자를 사용한다.
// Card 생성자함수로 실행된 객체를 Card 객체라고 한다.
var card = new Card("heart", "A");

console.log(card); // -> Card { suit : "heart", rank : "A" }
{% endhighlight %}

#### 생성자
* new 연산자로 객체를 생성할 것이라 기대하고 만든 함수를 **생성자**라 한다.
* 보통은 파스칼 표기법을 사용한다.
* 생성자 안에서 **this.프로퍼티** 이름에 값을 대입하면 그 이름을 가진 프로퍼티에 값이 할당된 객체가 생성됨
* this 는 생성자가 생성하는 객체를 가리킨다.
* 생성자와 new 연산자로 생성한 객체를 그 생성자의 인스턴스라고 부른다.(관례적으로,)

#### 생성자의 역할
* 생성자는 객체를 생성하고 초기화 하는 역할을 한다.
* 생성자를 사용하면 이름은 같지만 프로퍼티 값이 다른 객체(인스턴스) 여러개를 간단히 생성 가능하다.
* 생성자는 함수이므로 프로퍼티에 값을 대입 가능하다.

{% highlight javascript linenos %}
function Particle(x, y, vx, vy){
    this.x = x;
    this.y = y;
    this.vx = vx;
    this.vy = vy;
    // velocity 프로퍼티에 추가한다.
    this.velocity = Math.sqrt(vx * vx + vy * vy);
}

var p = new Particle(0,0,3,4);
console.log(p);  // Particle { x:0, y:0, vx:3, vy:4, velocity:5};
