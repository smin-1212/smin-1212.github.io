---
layout: post
title: Docker 컨테이너 생성/시작/정지
tags: [docker]
---

#  Docker 컨테이너 생성 / 시작 / 정지

## 1. Docker 컨테이너의 라이프 사이클

![Docker Container Life Cycle](https://drive.google.com/uc?id=1d_4znUy3_JO2qy4UYmggJb43IelqK-sf)

### 1.1 컨테이너 생성

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

