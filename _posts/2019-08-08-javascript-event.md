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

### removeEventListener 메서드로 이벤트 리스너 삭제하기

```javascript
target.removeEventListener(type, listener, useCapture);
```


---


### 이벤트 객체
* 이벤트 처리기와 이벤트 리스너는 이벤트 객체를 인수로 받는다.
* 이벤트 객체는 해당 이벤트의 다양한 정보를 저장한 프로퍼티와 이벤트의 흐름을 제어하는 메서드를 가지고 있다.

```javascript
// 인수 e => 이벤트 객체
// 이벤트 객체임을 명시하기 위해 e 또는 event 를 사용하는것이 관례
function changeColor(e) {
    e.currentTarget.style.backgroundColor = "red";
}
```

```javascript
// ./elt.js 
/*
함수 이름 : elt
*/
function elt(name, attributes){
    var node = document.createElement(name);
    if(attributes){
        for(var attr in attributes){
            if(attributes.hasOwnProperty(attr)){
                node.setAttribute(attr, attributes[attr]);
            }
        }
    }
    for(var i = 2; i<arguments.length; i++){
        var child = arguments[i];
        if(typeof child == "string"){
            child = document.createTextNode(child);
        }
        node.appendChild(child);
    }
    return node;
}
```



```HTML
<!DOCTYPE html>
<html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>보더 박스 드래그</title>
        <script src="./elt.js"></script>
        <script>
            window.onload = function(){
                var box = elt("div", null, "JavaScript");
                document.body.appendChild(box);
                var boxOffsetX, boxOffsetY;
                box.style.display = "inline-block";
                box.style.position = "absolute";
                box.style.padding = "10px";
                box.style.backgroundColor = "blue";
                box.style.color = "white";
                box.style.cursor = "pointer";

                box.addEventListener("mousedown", function mouseDownListener(e){
                    document.addEventListener("mousemove", mouseMoveListener, false);
                    boxOffsetX = e.offsetX;
                    boxOffsetY = e.offsetY;
                }, false);

                document.addEventListener("mouseup", function mouseUpListener(e){
                    document.removeEventListener("mousemove", mouseMoveListener, false);
                }, false);

                function mouseMoveListener(e){
                    box.style.left = e.pageX - boxOffsetX + "px";
                    box.style.top = e.pageY - boxOffsetY + "px";
                }
            };
        </script>
    </head>
    <body>
    </body>
</html>
```

---

### 이벤트의 단계

* HTML 요소에서는 클릭 같은 사용자 행위와 프로그램 코드가 이벤트를 발생시킨다.
* 이벤트가 발생한 요소는 **이벤트 타깃** 이라고 한다.
* 이벤트를 발생시키는 것을 **이벤트를 발사(Fire)** 한다고 표현하기도 한다.

* 요소에서 이벤트가 발생하면 그 다음 단계에 접으들었을 떄, 그 이벤트를 다음 요소로 전파한다.
* 그 단계에 해당 이벤트에 반응하는 이벤트 처리기나 이벤트 리스너를 발견하면 그들을 실행한다.

* 이벤트의 단계는 아래와 같다.
> * 캡처링 단계 : 이벤트가 window 객체에서 출발해서 DOM 트리를 타고 이벤트 타깃까지 전파된다. 이 단계에 반응하도록 등록된 이벤트 리스너는 이벤트가 발생한 요소에 등록된 이벤트 처리기나 이벤트 리스너보다 먼저 실행된다. 이 단계는 이벤트 타깃이 이벤트를 수신하기 전에 이벤트를 빼돌리는(캡처하는) 단계라는 뜻에서 캡처링 단계라는 이름이 붙었다.
> * 타깃 단계 : 이벤트가 이벤트 타깃에 전파되는 단계이다. 이벤트 타깃에 등록된 이벤트 처리기나 이벤트 리스너는 이 시점에 실행된다.
> * 버블링 단계 : 이벤트가 이벤트 타깃에서 출발해서 DOM 트리를 타고 Window 객체까지 전파된다. 이 단계에 반응하도록 등록된 이벤트 리스너는 이벤트가 발생한 요소에 등록된 이벤트 처리기나 이벤트 리스너 다음에 실행된다. 이 단계는 이벤트가 마치 거품(버블)이 올라오는 것 처럼 DOM 트리 아래에서부터 위로 올라온다는 뜻에서 버블링 단계라는 이름이 붙었다.

* 이벤트 리스너는 이벤트가 전파되는 순서에 따라 차례대로 실행된다.

#### 이벤트의 전파
* 자식 요소에서 발생한 이벤트는 부모 요소에도 전파된다.
* 의도하지 않은 동작을 할 때도 있다.
* 이때는 이벤트 전파를 취소할 수 있다. ( 삭제가 아님 )

```javascript
// 이벤트 전파 취소 : stopPropagation 메서드
event.stopPropagation();

// 이벤트의 일시정지 : stopImmediatePropagation 메서드
event.stopImmediatePropagation();

// 기본 동작 취소하기 : preventDefault 메서드
event.preventDefault();
```



