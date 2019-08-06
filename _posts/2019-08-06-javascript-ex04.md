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

#### 실행 문맥의 구성
**실행 문맥(Execution Context)** 은 실행 가능한 코드가 실제로 실행되고
관리되는 영역, 실행에 필요한 모든 컴포넌트 여러개가 나누어 관리하도록 만들어져 있음.
