---
layout: post
title: Docker 이미지 조작
tags: [docker]
---

# Docker image 조작

## 실행 환경
> Docker 가 설치된 OS CLI 환경

---
## 1. Docker Hub 이미지 검색
### 접속 주소 : [https://hub.docker.com/](https://hub.docker.com/)

### 1.1 Docker Hub 에서 이미지를 검색한다

### 1.1.1 이미지 지정

```json
이미지명 [:태그명]
```
### 1.1.2 사용 예시

```
centos:7
-> centos 7 을 가지고 있는 Docker 이미지를 의미한다.
-> latest로 태그명을 지정할 수 있다.
-> latest로 지정할 경우 최신버전의 이미지를 받을 수 있다.
```

---

## 2. Docker Hub 이미지 다운로드

### 2.1 Docker image 를 다운로드 받는다.

### 2.1.1 docker image pull 명령 구문

```bash
# Docker Hub 에서 이미지를 다운로드 받는 명령
# 이미지 취득은 아래와 같은 구문으로 실행한다.
docker image pull [option] 이미지명[:태그명]
```

### 2.1.2 사용 예시 [CentOS 의 이미지 취득]

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

## 3. 이미지 목록 표시

### 3.1 로컬에 있는 이미지를 검색한다.

### 3.1.1 docker image ls 명령 구문

```bash
docker image ls [option] [repository_name]
```

### 3.1.2 지정 가능한 주요 옵션

옵션|설명
---|---
-all,-a|모든 이미지를 표시
--digests|다이제스트를 표시여부
--no-trunc|결과를 모두 표시
--quiet, -q|Docker 이미지 ID만 표시

### 3.1.3 사용 예시

```bash
]$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
{repo_name}/cheers2019    latest              8aaa8d93a301        About an hour ago   4.01MB
<none>              <none>              72fb9ac88e80        About an hour ago   357MB
golang              1.11-alpine         17915ddcabc4        5 days ago          312MB
centos              7                   9f38484d220f        5 months ago        202MB
```

### 3.1.4 docker image ls 명령 결과

항목|설명
---|---
REPOSITORY|이미지 이름
TAG|이미지 태그명
IMAGE ID|이미지 ID
CREATED|작성일
SIZE|이미지 크기


#### __다이제스트 ?__
> Docker 레지스트리에 업로드한 이미지는 이미지를 고유하게 식별하기위한 다이제스트를 부여받는다.
> 
> 명령창에서 다이제스트를 표시하고 싶을 때는 --digests 옵션을 설정한다.

---

## 4. 이미지 상세 정보 확인

### 4.1 이미지의 상세 정보를 확인한다.

### 4.1.1 docker image inspect 구문

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
```

---

## 5. 이미지 태그 설정

### 5.1 이미지에 대한 태그를 설정한다.

### 5.1.1 docker image tag 구문

```bash
docker image tag <Docker Hub 사용자명>/이미지명:[태그명]
```

### 5.1.2 사용예시

```bash
# nginx라는 이미지를 다운로드 받았다는 가정
docker image tag nginx smahn/webserver:1.0

# 변경된 태그명 확인
docker image ls
```

---

## 6. 이미지 검색

### 6.1 Docker Hub 에 있는 이미지를 찾는다.

### 6.1.1 docker image search 구문

```bash
docker search [option] <search keyword>
```

### 6.1.2 지정 가능한 주요 옵션

옵션|설명
---|---
--no-trunc|결과를 모두 표시
--limit|n 건의 검색결과를 표시
--filter=stars=n|즐겨찾기의 수(n 이상)를 지정

### 6.1.3 사용 예시

```bash
]$ docker search nginx
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

```bash
# 인기있는 이미지 검색, 즐겨찾기가 1000 이상인 이미지 검색
]$ docker search --filter=stars=1000 nginx
NAME                  DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                 Official build of Nginx.                        11837               [OK]
jwilder/nginx-proxy   Automated Nginx reverse proxy for docker con…   1641                                    [OK]
```

---

## 7. 이미지 삭제

### 7.1 로컬에 저장된 이미지 파일을 삭제한다.

### 7.1.1 docker image rm 구문

```bash
docker image rm [option] 이미지명 [이미지명]
```

### 7.1.2 지정 가능한 주요 옵션

옵션|설명
---|---
--force, -f|이미지를 강제로 삭제
--no-prune|중간 이미지를 삭제하지 않음

### 7.1.3 사용 예시

```bash
# image 목록을 확인
]$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              72fb9ac88e80        3 hours ago         357MB
nginx               latest              5a3221f0137b        3 days ago          126M

# image 삭제 명령
]$ docker image rm nginx
Untagged: nginx:latest
Untagged: nginx@sha256:53ddb41e46de3d63376579acf46f9a41a8d7de33645db47a486de9769201fec9
Deleted: sha256:5a3221f0137beb960c34b9cf4455424b6210160fd618c5e79401a07d6e5a2ced
Deleted: sha256:9517458cc6efeca05fee894eb0dda7cda7a704a6469e4624792289c9ff4350c0
Deleted: sha256:963e4580fca3bf364728a7df40a2348939c25213d32508c53849108d05764093
Deleted: sha256:1c95c77433e8d7bf0f519c9d8c9ca967e2603f0defbf379130d9a841cca2e28e

# image 목록 확인
]$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              72fb9ac88e80        4 hours ago         357MB
]$
```

### 7.2 사용하지 않은 이미지 파일을 삭제한다.

### 7.2.1 docker image prune 구문

```bash
docker image prune [option]
```

### 7.2.2 지정 가능한 주요 옵션

옵션|설명
---|---
--all, -a|사용하지 않은 이미지를 모두 삭제
--force, -f|이미지를 강제로 삭제

### 7.2.3 사용예시

```bash
# 사용하지 않은 Docker 이미지를 삭제할때 사용된다.
]$ docker image prune
```

---

## 8. Docker Hub에 로그인 

### 8.1 Docker Hub 에 로그인 한다.

### 8.1.1 docker login 구문

```bash
docker login [option] [server]
```

### 8.1.2 지정 가능한 주요 옵션

옵션|설명
---|---
--password, -p|비밀번호
--username, -u|사용자명

### 8.1.3 사용 예시

```bash
# 옵션을 지정하지 않으면 사용자명과 비밀번호를 차례로 물어본다.
]$ docker login
username: {등록한 사용자명}
password: {등록한 패스워드}
```

---

## 9. 이미지 업로드

### 9.1 Docker Hub 에 이미지를 업로드 한다.

### 9.1.1 image push 구문

```bash
docker image push 이미지명[:태그명]

# 업로드할 이미지는 아래와 같은 형식지정
<Docker Hub 사용자명>/이미지명:[태그명]
```

### 9.1.2 사용 예시

```bash
]$ docker image push smahn/cheers2019
# push 이후 Docker Hub 웹페이지로 이동하면 이미지 등록 확인 가능하다.
```

---

## 10. Docker Hub 로그아웃

### 10.1 Docker Hub 에 로그아웃을 한다.

### 10.1.1 docker logout 구문

```bash
docker logout [서버명]
```

