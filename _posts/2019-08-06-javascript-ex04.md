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



