---
layout: post
title: javascript Map, Set 객체
tags: [javascript, js, es6]
---


### Map
* Map 객체는 데이터를 수집하여 활용하기 위한 객체
* 값의 고유한 식별값인 '키'와 값의 쌍을 Map 객체 안에 저장해서 사용한다.
* Map 과 Object 차이점은 아래와 같다.
> * Map 객체에는 데이터를 수집하기 위한 다양한 메서드가 마련되어있다.
> * Object 객체는 키로 문자열만 사용할 수 있지만 Map 객체는 키 타입에 제한이 없다.
> * Map 객체는 내부적으로 해시 테이블을 활용하기 때문에 데이터 검색 속도가 빠르다.
> * Map 객체는 반복 가능(이터러블) 하며 for/of문으로 순회하면 키와 값으로 구성된 배열을 반환한다.
> * Map 객체는 데이터 개수를 size 프로퍼티로 구할 수 있다. Object 객체는 프로퍼티개수를 수동으로 계산해야 한다.

* Map 객체의 생성

```javascript
// Map 객체는 Map 생성자 함수로 생성한다. 
// 인수를 지정하지 않으면 데이터가 없는 빈 Map 객체가 생성된다.
var map = new Map();
console.log(map); // map -> {}

// 초기데이터를 인수로 지정해서 생성,
var zip = new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
console.log(zip);  // -> Map { "Tom" => "131-8634", "Huck" => "556-0002" }

// 제너레이터로 이터레이터를 생성하는 방식, 위와 똑같은 작업 처리
function* makeZip(){
    yield ["Tom", "131-8634"];
    yield ["Huck", "556-0002"];
}
var zips = makeZip();
zip = new Map(zips);
console.log(zip);
```

