---
layout: post
title: javascript 함수 심화
tags: [javascript, js, es6]
---

#### 함수를 정의하는 방법
* 함수 선언문으로 정의하는 방법
```javascript
function square(x) { return x*x; }
```

**Warning** 함수 선언문으로 정의하면 호이스팅된다.
{: .box-warning}

* 함수 리터럴로 정의하는 방법
```javascript
var square = function(x) { return x*x };
```
* Function 생성자로 정의하는 방법
```javascript
var square = new Function("x", "return x*x");
```
* 화살표 함수 표현식으로 정의하는 방법
```javascript
var square = x => x*x;
```
---
#### 중첩 함수 (Nested Function)
* 특정 함수 내부에 선언된 함수를 그 함수의 **중첩 함수** 라고 한다.
* 중첩 함수를 지역 함수 또는 내부 함수 라고 부르기도 한다.
* 외부 함수의 최상위 레벨에서만 중첩 함수를 작성할 수 있다.
* 함수 안의 if문과 while 문 등의 문장 블록 안에는 중첩함수 작성이 불가
* 중첩 함수의 참조는 그 중첩 함수를 둘러싼 외부함수의 지역 변수에 저장되므로 외부 함수의 바깥에서는 읽거나 쓸 수 없다.
* 중첩 함수 자신을 둘러싼 외부 함수의 인수와 지역 변수에 접근이 가능하다.

{% highlight javascript linenos %}
// 변수 x 는 외부 함수인 norm 의 인수이다.
// 외부 함수의 변수 유효 범위가 그 함수의 중첩 함수에 까지 미친다.
// 클로저의 핵심 구성요소
function norm(x){
    var sum2 = sumSquare();
    return Math.sqrt(sum2);
    function sumSquare(){
        sum = 0; 
        for(var i = 0 ; i<x.length; i++){
            sum += x[i]*x[i];
        }
        return sum;
    }
}
var a = [2,1,3,5,7];
var n = norm(a);
console.log(n); 
{% endhighlight %}