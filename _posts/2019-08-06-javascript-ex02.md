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