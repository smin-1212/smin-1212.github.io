---
layout: post
title: 동작중인 Docker 컨테이너 조작
tags: [docker]
---


# 동작중인 Docker 컨테이너 조작

---

## 1. 동작중인 컨테이너 연결
### 1.1 동작중인 컨테이너에 연결한다.
### 1.1.1 실행 구문
```bash
docker container attach <컨테이너 식별자>
```

### 1.1.2 실행 예시
```bash
# 백그라운드로 실행중인 sample 컨테이너에 접근한다.
]$ docker container attach sample
root@53ba72a334f1:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

---

## 2. 동작중인 컨테이너에서 프로세스 실행
### 2.1 동작중인 컨테이너에서 새로운 프로세스를 실행한다.
### 2.1.1 실행 구문
```bash
docker container exec [option] <컨테이너 식별자> <실행할 명령> [인수]
```

### 2.1.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
--detach, -d|명령을 백그라운드에서 실행한다.
--interactive, -i|컨테이너의 표준 입력을 연다.
--tty, -t|tty(단말 디바이스)를 사용한다.
--user, -u|사용자명을 지정한다.

### 2.1.3 실행 예시
```bash
# 컨테이너에서 bash 실행
]$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
1ceed352a4d8        nginx               "nginx -g 'daemon of…"   16 minutes ago      Up 16 minutes                              webap
29649f6ab2ee        nginx               "nginx -g 'daemon of…"   22 minutes ago      Up 22 minutes       80/tcp                 webfront
53ba72a334f1        ubuntu:latest       "/bin/bash"              39 minutes ago      Up 39 minutes                              sample
b85486c48d3f        nginx               "nginx -g 'daemon of…"   2 hours ago         Up About an hour    0.0.0.0:8080->80/tcp   amazing_nash

# nginx 만 동작하는 서비스 bash 실행
]$ docker container exec -it webfront /bin/bash
root@29649f6ab2ee:/# ls
bin  boot  dev	etc  home  lib	lib64  media  mnt  opt	proc  root  run  sbin  srv  sys  tmp  usr  var

# docker container exec 명령은 동작중인 컨테이너에서만 실행 가능하다.
```

---

## 3. 동작중인 컨테이너의 프로세스 확인
### 3.1 동작중인 컨테이너의 프로세스를 확인한다.
### 3.1.1 실행 구문
```bash
docker container top <컨테이너 식별자>
```

### 3.1.2 실행 예시
```bash
]$ $ docker container top webfront
PID                 USER                TIME                COMMAND
7374                root                0:00                nginx: master process nginx -g daemon off;
7414                101                 0:00                nginx: worker process
```

---

## 4. 동작중인 컨테이너의 포트 전송 확인
### 4.1 동작중인 컨테이너에서 프로세스가 전송되고 있는 포트를 확인한다.
### 4.1.1 실행 구문
```bash
docker container port <컨테이너 식별자>
```

### 4.1.2 실행 예시
```bash
]$ docker container port amazing_nash
80/tcp -> 0.0.0.0:8080
```

---

## 5. 컨테이너의 이름 변경
### 5.1 컨테이너의 이름을 변경한다.
### 5.1.1 실행 구문
```bash
docker container rename <기존 컨테이너명> <변경 후 컨테이너명>
```

### 5.1.2 실행 예시
```bash
]$ docker container rename old new
```

--- 

## 6. 컨테이너 안의 파일을 복사
### 6.1 컨테이너 안의 파일을 복사하여 호스트에 옮긴다.
### 6.1.1 실행 구문
```bash
docker container cp <컨테이너 식별자>:<컨테이너 안의 파일 경로> <호스트 디렉토리 경로>
docker container cp <호스트의 파일> <컨테이너 식별자>:<컨테이너 안의 파일 경로>
```
