---
layout: post
title: Docker 이미지 조작
tags: [docker]
---

### Docker image 조작

---
#### Docker Hub 이미지 검색
##### 접속 주소 : [https://hub.docker.com/](https://hub.docker.com/)

##### 이미지 지정
```json
이미지명 [:태그명]
```
##### 사용 예시
```json
centos:7
-> centos 7 을 가지고 있는 Docker 이미지를 의미한다.
-> latest로 태그명을 지정할 수 있다.
-> latest로 지정할 경우 최신버전의 이미지를 받을 수 있다.
```

---

#### Docker Hub 이미지 다운로드

##### 실행 환경
> Docker 가 설치된 OS CLI 환경

##### docker image pull 명령 구문
```bash
# Docker Hub 에서 이미지를 다운로드 받는 명령
# 이미지 취득은 아래와 같은 구문으로 실행한다.
docker image pull [option] 이미지명[:태그명]
```

##### 사용 예시 [CentOS 의 이미지 취득]
```bash
# centOS 7버전 이미지를 다운로드 받는다.
docker image pull centos:7

# -a 옵션을 지정하면 이미지의 모든 태그를 취득한다.
# 단, latest 태그를 지정할 수 없다.
docker image pull -a centos
```


