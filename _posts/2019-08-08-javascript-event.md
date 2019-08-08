---
layout: post
title: javascript 이벤트
tags: [javascript, js, es6]
---


### 이벤트 등록방법

* HTML 요소의 이벤트 처리기 속성에 설정하는 방법

```HTML
<input type="button" onclick="changeColor();" >
<!-- 
 문제점 : 가독성이 떨어짐, 프로그램의 유지보수성이 떨어짐
       특정 요소의 특정 이벤트에 대하여 이벤트 처리기를 단 하나만 등록 가능함.
       똑같은 이벤트 처리기를 등록하면 이전에 등록한 함수를 덮어쓴다.
-->
```

* DOM 요소 객체의 이벤트 처리기 프로퍼티에 설정하는 방법

```javascript
var btn = document.getElementById("button");
btn.onclick = changeColor();
// 문제점
// 특정 요소의 특정 이벤트에 대해서 이벤트 처리기를 단 하나만 등록 가능
```

* **addEventListener 메서드 사용**하는 방법

```javascript
var btn = document.getElementById("button");
btn.addEventListener("click", changeColor, false);
```

--- 

### addEventListener 메서드로 이벤트 리스너 등록하기
* addEventListener 로 등록한 함수는 **이벤트 리스너** 라고 한다.
* addEventListener 메서드를 이용하면 같은 요소의 같은 이벤트에 이벤트 리스너를 여러개 등록 가능하다.

```javascript
// 이벤트 리스너 사용방법
target.addEventListener(type, listener, useCapture);
// target : 이벤트 리스터를 등록할 DOM 노드
// type : 이벤트 유형을 뜻하는 문자열("click","mouseup" 등)
// listener : 이벤트가 발생했을 때 처리를 담당하는 콜백 함수의 참조
// useCapture : 이벤트 단계

// useCapture 는 다음과 같다.
// true : 캡처링 단계
// false : 버블링 단계 ( 생략했을 때 기본값 )
```

#### addEventListener 를 사용해서 얻을 수 있는 장점은 아래와 같다.
> * 같은 요소의 같은 이벤트에 **이벤트 리스너를 여러개 등록 가능**하다.
> * 버블링 단계는 물론 캡처링 단계에서도 활용 가능하다. DOM요소 객체에 직접 등록한 이벤트 처리기는 버블링 단계의 이벤트만 캡처 가능함
> * removeEventListener, stopProgagation, preventDefault 를 활용하여 이벤트 전파를 정밀하게 제어할 수 있음
> * HTML 요소를 포함한 모든 DOM 노드에 이벤트 리스너를 등록 할 수 있다.