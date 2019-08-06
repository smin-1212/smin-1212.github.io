---
layout: post
title: javascript 함수 심화
tags: [javascript, js, es6]
---

#### 함수를 정의하는 방법
1. 함수 선언문으로 정의하는 방법
```javascript
function square(x) { return x*x; }
```

{: .box-warning}
**Warning** 함수 선언문으로 정의하면 호이스팅된다.


2. 함수 리터럴로 정의하는 방법
```javascript
var square = function(x) { return x*x };
```
3. Function 생성자로 정의하는 방법
```javascript
var square = new Function("x", "return x*x");
```
4. 화살표 함수 표현식으로 정의하는 방법
```javascript
var square = x => x*x;
```

