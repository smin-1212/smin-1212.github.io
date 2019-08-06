---
layout: post
title: javascript 함수
tags: [javascript, js, es6]
---

#### 함수

자바스크립트에서는 함수가 객체이다.

> * 함수 선언문으로 함수 정의하기
> ```javascript
>  function square(x) { return x * x; }
> ```
> 함수는 function 키워드로 정의한다.
> 함수 **호이스팅**이 발생한다.

> * 함수 리터럴로 함수 정의하기
{% highlight javascript linenos %}
var square = function(x) { 
    return x * x ; 
  };
{% endhighlight %}

> 함수 리터럴은 **이름없는 함수**이다. 
>
> **익명함수, 무명함수** 라고 부른다.
>
> 함수 리터럴을 사용할 때는 **끝에 반드시 세미콜론을 붙여야 한다.**
>
> 함수 호이스팅이 __발생하지 않는다.__
>
> 익명함수에 이름을 붙일 수 있다.
>
> ```javascript
> var square = function sq(x){return x * x ; };
> ```
> 
> 코드에서 sq라는 이름은 함수안에서만 유효하기 때문에 함수 밖에서는
> sq 라는 이름으로 호출이 불가하다.
> 디버깅할 때 유용하다.(어떤 함수가 호출되었는지 확인 가능)

##### 함수의 실행 흐름

호출한 코드에 있는 인수가 함수 정의문의 인자에 대입된다.

return 문이 실행되면 호출한 코드로 돌아간다. return 문의 값은 함수의 반환값이 된다.

return 문이 실행되지 않은 상태로 마지막 문장이 실행되면, 호출한 코드로 돌아간 후에 undefined가 함수의 반환값이 된다.

