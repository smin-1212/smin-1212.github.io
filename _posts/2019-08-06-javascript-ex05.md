---
layout: post
title: 클로저
tags: [javascript, js, es6]
---

### 클로저

클로저는 아래와 같은 동작을 하는 함수와 그 기능을 구현한 자료 구조의 모음이다.
> 자기 자신이 정의된 환경에서 함수 안에 있는 자유 변수의 식별자 결정을 실행한다.

```javascript
var a = "A";
function f(){
    var b = "B";             
    function g(){    // 열린 함수
        var c = "C";
        console.log(a+b+c);
    }
    g();
}
f();
```


중첩 함수 g는 열린 함수 이다. 그러나 유효 범위 체인으로 주변 환경의 변수 b와 a를 들여와서
실질적으로는 폐쇄 함수가 되었다. 열려있는것을 닫는다는 의미로 클로저.

{: .box-note}