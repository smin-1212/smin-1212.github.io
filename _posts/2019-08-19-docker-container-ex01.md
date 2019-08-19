---
layout: post
title: Docker 컨테이너 생성/시작/정지
tags: [docker]
---

#  Docker 컨테이너 생성 / 시작 / 정지

## Docker 컨테이너의 라이프 사이클

![Docker Container Life Cycle](https://drive.google.com/uc?id=1d_4znUy3_JO2qy4UYmggJb43IelqK-sf)

### 컨테이너 라이프 사이클 요소는 아래와 같다.
#### 1.1 컨테이너 생성 [create]
#### 2.1 컨테이너 생성 및 실행 [run]
#### 3.1 컨테이너 시작 [start]
#### 4.1 컨테이너 정지 [stop]
#### 5.1 컨테이너 삭제 [rm]

---

## 1. 컨테이너 생성
### 1.1 컨테이너를 생성한다.
### 1.1.1 컨테이너 생성 구문
```bash
docker container create 
```

### 1.1.2 create 구문 상세
- 이미지로부터 컨테이너를 생성한다.
- 이미지의 실체는 __'Docker에서 서버 기능을 작동시키기 위해 필요한 디렉토리 및 파일들이다.'__
- 명령 실행시 이미지에 포함될 Linux의 디렉토리와 파일들이 스냅샷을 취한다.
- 스냅샷이란 스토리지 안에 존재하는 파일과 디렉토리를 특정 타이밍에서 추출한 것 이다.
- 컨테이너 생성만 하고 컨테이너를 시작(동작)하지는 않는다.

---

## 2. 컨테이너 생성 및 시작
### 2.1 컨테이너 생성 및 시작 한다.
### 2.1.1 컨테이너 생성 및 시작 구문

```bash
docker container run [option] 이미지명[:태그명] [인수]
```

### 2.1.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
--attach, -a|표준 입력(STDIN), 표준 출력(STDOUT), 표준 오류 출력(STDERR) 에 Attach 한다.
--cidfile|컨테이너 ID 를 파일로 출력한다.
--detach, -d|컨테이너를 생성하고 백그라운드에서 실행한다.
--interactive, -i|컨테이너 표준 입력을 연다.
--tty, -t|단말기 디바이스를 사용한다.

