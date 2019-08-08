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

* Map 객체의 메서드

Map 객체는 Map.prototype의 프로퍼티와 메서드를 상속받는다.

메서드 | 설명
---|---
clear() | Map 객체 안에있는 모든 데이터를 삭제한다.
delete(key) | Map 객체에서 key가 가리키는 데이터를 삭제한다.
entries() | Map 객체가 가진 데이터(키와 값 쌍) 값을 저장한 이터레이터를 데이터를 삽입한 순서대로 반환
forEach(callback) | Map 객체의 모든 데이터를 대상으로 callback 함수를 실행한다. 이때 실행 순서는 데이터가 삽입된 순서이다.
get(key) | Map 객체에서 key 가 가리키는 데이터를 반환한다.
has(key) | Map 객체에서 key 가 가리키는 데이터가 있는지 판정한다.
keys() | Map 객체에서 데이터 키를 값으로 가지는 이터레이터 반환한다.
set(key, value) | Map 객체에 키가 key 고 값이 value 인 데이터를 추가
values() | Map객체에서 데이터 값을 값으로 가지는 이터레이터를 반환한다.

```javascript
// 데이터 추가하기
// set(key, value) 메서드 사용

var zip = new Map();
zip.set("Tom", "131-8634");
zip.set("Huck", "556-0002");
console.log(zip); // Map {"Tom" => "131-8634", "Huck" => "556-0002" }

// 값 읽기
// 특정 키 값이 가리키는 데이터 값을 읽을때, get(key) 메서드를 사용
console.log(zip.get("Tom")); // 131-8634

// 데이터가 있는지 확인
console.log(zip.has("Tom")); // true

// 데이터의 삭제
// Key 는 데이터의 키 값이다.
zip.delete("Huck"); 

// 모든 데이터 삭제
zip.clear();

// 모든 키값의 열거
var iter = zip.keys();
for(var v of iter){
    console.log(v); // key 출력
}

// 모든 값의 열거
var iter = zip.values();
for(var v of iter){
    console.log(v); // value 출력
}

// 비구조화 할당 활용
var zip = new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
for(var [key, value]] of zip ){
    console.log(key, value);
}

// 모든 데이터를 함수로 처리하기
var zip = new Map([["Tom", "131-8634"], ["Huck", "556-0002"]]);
zip.forEach(function(value, key, map){
    console.log(key + " => " + value);
});
```




---

### Set 객체 
* Set 객체는 중복되지 않는 유일한 데이터를 수집하여 활용하기 위한 객체.
* Set 객체는 데이터 값의 단순 집합으로 간주.
* Map 과 비슷


* WeakMap 과 WeakSet
> * Map이나 Set과 유사한 객체로 WeakMap과 WeakSet 이 있다.
> * 데이터의 키 값을 약한 참조로 관리한다.
> * 약한 참조는 다른 객체가 참조하고 있는 객체라도 가비지 컬렉션의 대상이 될 수 있다.
