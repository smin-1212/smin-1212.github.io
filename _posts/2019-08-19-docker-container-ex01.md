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

### 2.1.3 사용 예시

```bash
# docker container run 대화식 실행
# centos 이미지를 다운로드 받아 /bin/bash 를 실행한다.
docker container run -it --name "test1" centos /bin/bash
[root@xxxxxxxxx /]#
# 이미지 파일이 로컬에 없으면 다운로드 받은 후 실행한다.
##########################################
# 구문 설명
# docker container run : 컨테이너를 생성 및 실행
# -it : 콘솔에 결과를 출력하는 옵션
# --name "test1" : 컨테이너의 이름설정, test1 으로 설정한다.
# centos : 이미지 명
# /bin/bash : 컨테이너에서 실행할 명령

# 컨테이너 종료시 exit 명령을 입력한다.
[root@xxxxxxxx /]# exit
```

### 2.2.1 컨테이너 백그라운드 실행 구문

```bash
docker container run [실행 옵션] 이미지명[:태그명] [인수]
```

### 2.2.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
--detech, -d|백그라운드에서 실행
--user, -u|사용자명을 지정
--restart=[no \| on0failure \| on-failure:횟수n \| always \| unless-stopped]|명령의 실행 결과에 따라 재시작을 하는 옵션
--rm|명령 실행 완료 후에 컨테이너를 자동으로 삭제

### 2.2.3 사용 예시

```bash
]$ docker container run -d centos /bin/ping localhost
##########################################
# 구문 설명
# docker container run : 컨테이너를 생성 및 실행
# -d : 백그라운드에서 실행하는 옵션
# centos : 이미지 명
# /bin/ping localhost : 컨테이너에서 실행할 명령
# 실행 후의 컨테이너를 자동으로 삭제하고 싶을 때는 --rm 옵션을 지정한다.

# 동작중인 컨테이너의 아이디 확인
]$ docker container ls
CONTAINER ID        IMAGE               COMMAND                 CREATED             STATUS              PORTS               NAMES
045e721e0fb1        centos              "/bin/ping localhost"   8 seconds ago       Up 8 seconds                            heuristic_haslett

# 백그라운드로 동작중인 컨테이너의 로그 확인
]$ docker container logs -t 045e721e0fb1
2019-08-19T05:42:07.926233897Z PING localhost (127.0.0.1) 56(84) bytes of data.
2019-08-19T05:42:07.926304150Z 64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.040 ms
2019-08-19T05:42:09.000032071Z 64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.119 ms
2019-08-19T05:42:10.040354741Z 64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.066 ms
2019-08-19T05:42:11.079598887Z 64 bytes from localhost (127.0.0.1): icmp_seq=4 ttl=64 time=0.107 ms
2019-08-19T05:42:12.119377122Z 64 bytes from localhost (127.0.0.1): icmp_seq=5 ttl=64 time=0.061 ms
```

### 2.2.4 --restart 옵션

설정값|설명
---|---
no|재시작하지 않는다.
on-failure|종료 스테이터스가 0이 아닐 때 재시작 한다.
on-failure:횟수n|종료 스테이터스가 0이 아닐 때 n번 재시작 한다.
always|항상 재시작 한다.
unless-stopped|최근 컨테이너가 정지상태가 아니라면 항상 재시작 한다.

### 2.2.5 실행 예시

```bash
# exit 로 종료하여도 항상 재시작 한다.
]$ docker container run -it --restart=always centos /bin/bash
[root@ea3c99d510ef /]# exit
exit

# --restart 옵션을 넣지 않을 경우 exit 로 빠져나오면
# 컨테이너의 동작이 멈춘다.
]$ docker container ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ea3c99d510ef        centos              "/bin/bash"         13 seconds ago      Up 6 seconds                            happy_wright
```

---

## 3. 컨테이너의 네트워크 설정
### 3.1 컨테이너의 네트워크를 설정한다.

