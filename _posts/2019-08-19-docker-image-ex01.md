---
layout: post
title: Docker 이미지 조작
tags: [docker]
---

## Docker image 조작

### 실행 환경
> Docker 가 설치된 OS CLI 환경

---
### Docker Hub 이미지 검색
#### 접속 주소 : [https://hub.docker.com/](https://hub.docker.com/)

#### 이미지 지정
```json
이미지명 [:태그명]
```
#### 사용 예시
```
centos:7
-> centos 7 을 가지고 있는 Docker 이미지를 의미한다.
-> latest로 태그명을 지정할 수 있다.
-> latest로 지정할 경우 최신버전의 이미지를 받을 수 있다.
```

---

### Docker Hub 이미지 다운로드

#### docker image pull 명령 구문
```bash
# Docker Hub 에서 이미지를 다운로드 받는 명령
# 이미지 취득은 아래와 같은 구문으로 실행한다.
docker image pull [option] 이미지명[:태그명]
```

#### 사용 예시 [CentOS 의 이미지 취득]
```bash
# centOS 7버전 이미지를 다운로드 받는 명령
docker image pull centos:7

# -a 옵션을 지정하면 이미지의 모든 태그를 취득한다.
# 단, latest 태그를 지정할 수 없다.
docker image pull -a centos

# URL 을 지정하여 다운로드 가능하다.
docker image pull gcr.io.tensorflow/tensorflow
```

---

### 이미지 목록 표시

#### docker image ls 명령 구문
```bash
docker image ls [option] [repository_name]
```
##### 지정 가능한 주요 옵션


옵션|설명
---|---
-all,-a|모든 이미지를 표시
--digests|다이제스트를 표시여부
--no-trunc|결과를 모두 표시
--quiet, -q|Docker 이미지 ID만 표시


```bash
]$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
{repo_name}/cheers2019    latest              8aaa8d93a301        About an hour ago   4.01MB
<none>              <none>              72fb9ac88e80        About an hour ago   357MB
golang              1.11-alpine         17915ddcabc4        5 days ago          312MB
centos              7                   9f38484d220f        5 months ago        202MB
```

##### docker image ls 명령 결과


항목|설명
---|---
REPOSITORY|이미지 이름
TAG|이미지 태그명
IMAGE ID|이미지 ID
CREATED|작성일
SIZE|이미지 크기


##### 다이제스트 ?
> Docker 레지스트리에 업로드한 이미지는 이미지를 고유하게 식별하기위한 다이제스트를 부여받는다.
> 
> 명령창에서 다이제스트를 표시하고 싶을 때는 --digests 옵션을 설정한다.

---

### 이미지 상세 정보 확인

#### docker image inspect 구문

```bash
docker image inspect 이미지명[:태그명]
# 결과는 json 문자열 형식으로 출력된다.
# 주요 확인 항목은 아래와 같다.
# ID : 이미지의 ID
# Created : 작성일
# DockerVersion : Docker 버전
# Architecture : CPU 아키텍처

docker image inspect --format="{{ .Os}}" centos:7
# --format 옵션은 JSON의 계층구조를 지정한다.
# 
```

---

### 이미지 태그 설정

#### docker image tag 구문

```bash
docker image tag <Docker Hub 사용자명>/이미지명:[태그명]
```

#### 사용예시

```bash
# nginx라는 이미지를 다운로드 받았다는 가정
docker image tag nginx smahn/webserver:1.0

# 변경된 태그명 확인
docker image ls
```

---

### 이미지 검색

#### docker image search 구문

```bash
docker search [option] <search keyword>
```

#### 지정 가능한 주요 옵션

옵션|설명
---|---
--no-trunc|결과를 모두 표시
--limit|n 건의 검색결과를 표시
--filter=stars=n|즐겨찾기의 수(n 이상)를 지정

### 사용예시
```bash
docker search nginx

NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                             Official build of Nginx.                        11837               [OK]
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   1641                                    [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   734                                     [OK]
linuxserver/nginx                 An Nginx container, brought to you by LinuxS…   73
bitnami/nginx                     Bitnami nginx Docker Image                      70                                      [OK]
tiangolo/nginx-rtmp               Docker image with Nginx using the nginx-rtmp…   51                                      [OK]
...( 중략 )

# NAME : 이미지 이름
# DESCRIPTION : 이미지 설명
# STARS : 즐겨찾기 수
# OFFICIAL : 공식 이미지인지에 대한 여부
# AUTOMATED : Dockfile 바탕으로 생성된 파일인지에 대한 여부
```
