---
layout: post
title: javascript 프로그램의 평가와 실행과정
tags: [javascript, js, es6]
---

#### 실행 가능한 코드
 자바스크립트 엔진은 **실행 가능한 코드(Executable Code)** 를 만나면, 

그 코드를 평가(Evaluation)해서 실행 문맥(Execution Context)으로 만든다.

실행 가능한 코드(Executable Code)의 유형은 다음과 같다.

* 전역 코드
* 함수 코드
* eval 코드
 
전역코드는 전역 객체(Window) 아래에 정의된 함수를 말한다.

자바스크립트 엔진이 실행가능한 코드의 유형을 분류하는 이유는

**실행 문맥을 초기화 하는 환경과 과정이 다르기 때문이다.**

---

#### 실행 문맥의 구성
**실행 문맥(Execution Context)** 은 실행 가능한 코드가 실제로 실행되고

관리되는 영역, 실행에 필요한 모든 컴포넌트 여러개가 나누어 관리하도록 만들어져 있음.

```javascript
// 실행 문맥
ExecutionContext = {
    // 렉시컬 환경 컴포넌트
    LexicalEnvironment:{},
    // 변수 환경 컴포넌트
    VariableEnvironment: {},
    // 디스 바인딩 컴포넌트
    ThisBinding: null,
}
```

**렉시컬 환경(Lexical Environment) 컴포넌트**와 **변수 환경(Variable Environment) 컴포넌트**는 렉시컬 환경 타입의 컴포넌트이다.

**디스 바인딩(This Binding)** 컴포넌트는 그 함수를 호출한 객체의 참조가 저장되는 곳,

이것이 가리키는 값이 곧 해당 실행 문맥의 this 가 된다.

---

##### **렉시컬 환경 컴포넌트의 구성**
* 자바스크립트 엔진이 자바 스크립트 코드를 실행하기 위해 자원을 모아 둔 곳,
* 함수 또는 블록의 유효 범위 안에 있는 식별자와 그 결과 값을 저장하는 곳
* 자바스크립트 엔진은 해당 자바스크립트 코드의 유효 범위 안에 있는 식별자와 그 식별자가 가리키는 값을 키와 값의 쌍으로 바인드 해서 렉시컬 환경 컴포넌트에 기록한다.
* 환경 레코드(Environment Record) 와 외부 렉시컬 환경 참조 (Outer Lexical Environment Reference) 컴포넌트로 구성되어 있다.
```javascript
// 렉시컬 환경 컴포넌트
LexicalEnvironment :{
    // 환경 레코드
    EnvironmentRecord :{},
    // 외부 렉시컬 환경 참조
    OuterLexicalEnvironment Reference: {}
}
```

**환경레코드(Environment Record)**
* 유효 범위 안에 포함된 식별자를 기록하고 실행하는 영역,
* 자바스크립트 엔진은 유효 범위 안의 식별자와 결과값을 바인드 해서 환경레코드에 기록한다.

**외부 렉시컬 환경 참조(Outer Lexical Environment Reference)**
* 자바스크립트는 함수 안에 함수를 중첩해서 정의할 수 있는 언어. 
-> 따라서 자바스크립트 엔진은 유효 범위 너머의 유효 범위도 검색 가능해야한다.
-> 외부 렉시컬 환경 참조에 함수를 둘러싸고 있는 코드가 속한 렉시컬환경 컴포넌트의 참조가 저장된다.
-> 중첩된 함수 안에서 바깥 코드에 정의된 변수를 읽거나 써야할 때, 자바스크립트 엔진은 외부 렉시컬 환경 참조를 따라 한 단계씩 렉시컬 환경을 거슬러 올라가 그 변수를 검색

---

##### **환경레코드의 구성**
* 환경 레코드는 렉시컬 환경안의 식별자와 그 식별자가 가리키는 값의 묶음이 실제로 저장되는 영역
* **선언적 환경 레코드(Declarative Environment Record)** 와 **객체 환경 레코드(Object Environment Record)** 로 구성되어 있으며 저장하는 값의 유형에 따라 쓰임새가 달라짐
```javascript
// 환경 레코드
EnvironmentRecord : {
    // 선언적 환경 레코드
    DeclarativeEnvironmentRecord : {},
    // 객체 환경 레코드
    ObjectEnvironmentRecord :{}
}
```

* 선언적 환경 레코드 (Declarative Environment Record) 는 실제로 함수와 변수, Catch 문의 식별자와 실행 결과가 저장되는 영역이다.
* 객체 환경 레코드 (Object Environment Record)는 실행 문맥 외부에 별도로 저장된 객체의 참조에서 데이터를 읽거나 쓴다. -> 객체 전체의 참조를 가져와서 객체 환경 레코드의 bindObject 라는 프로퍼티에 바인드 하도록 만들어저 있다.

---

##### 전역 환경과 전역 객체의 생성
* 자바스크립트 인터프리터는 시작하자마자 렉시컬 환경 타입의 전역 환경(Global Environment)을 생성
* 자바스크립트 인터프리터는 웹브라우저에 내장되어 있다. -> 새로운 웹 페이지를 읽은 후 전역 환경을 생성한다.
* 전역객체 생성 후 전역 환경의 객체 환경 레코드에 전역 객체의 참조를 대입한다.

```javascript
// 전역 환경
GlobalEnvironment = {
    ObjectEnvironmentRecord: {
        bindObject : window
    }, 
    OuterLexicalEnvironmentReference: null
}

// 전역 실행 문맥
ExecutionContext = {
    LexicalEnvironment : GlobalEnvironment,
    ThisBinding : window,
}
```

웹 브라우저의 자바스크립트 실행 환경에서는 Window 객체가 전역 객체 이므로 객체 환경 레코드의 

bindObject 프로퍼티에는 전역 객체 Window 의 참조가 할당된다.

이로인해 전역 환경의 변수와 함수를 Window 안에서 검색하게 된다.

전역 환경의 외부에는 다른 렉시컬 환경이 없으므로 외부 렉시컬 환경 참조(Outer Lexical Environment Reference )에는 null을 할당한다.

전역 실행 문맥의 디스 바인딩 컴포넌트에도 Window의 참조가 할당되어 전역 실행 문맥의 this 가 Window를 가리키게 되고, 전역 실행 문맥의 프로퍼티를  디스 바인딩 컴포넌트 안에서 검색하게 된다.

---

#### 프로그램 평가와 전역 변수

전역 환경과 전역객체를 생성한 후, 자바스크립트 프로그램을 읽어 들인다.

자바스크립트 프로그램을 다 읽어 들인 후 프로그램을 평가한다.

최상위 레벨에 var 문으로 작성한 전역 변수는 전역 환경의 환경 레코드(객체 환경 레코드)의 프로퍼티로 추가된다.

```javascript
// 전역 환경
GlobalEnvironment = {
    // 전역 환경의 환경 레코드인 객체 환경 레코드에 Window 의 참조가 설정되어 있다.
    ObjectEnvironmentRecord : {
        bindObject : window
    },
    OuterLexicalEnvironmentReference : null
}
```

