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
### 3.1.1 컨테이너의 네트워크 설정 구문

```bash
docker container run [네트워크 옵션] 이미지명[:태그명] [인수]
```

### 3.1.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
--add-host=[호스트명:IP주소]|컨테이너의 /etc/hosts에 호스트명과 IP 주소를 정의
--dns=[IP주소]|컨테이너용 DNS 서버의 IP 주소 지정
--expose|지정한 범위의 포트 번호를 할당
--mac-address=[MAC주소]|컨테이너의 MAC 주소를 지정
--net=[bidge \| none \| container:<name\|id> \| host \| network]|컨테이너의 네트워크를 지정
--hostname, -h|컨테이너 자신의 호스트명을 지정
--publish, -p[호스트의 포트번호]:[컨테이너의 포트번호]|호스트와 컨테이너의 포트 매핑
--publish-all, -P|호스트의 임의의 포트를 컨테이너에 할당

### 3.1.3 사용 예시

```bash
# 컨테이너의 포트를 매핑한다.
# 호스트의 포트 번호 8080과 컨테이너의 포트번호 80을 mapping
# 호스트의 8080 포트에 Access 하면 컨테이너에서 작동하고 있는 Nginx(80) 의 서비스에 엑세스 가능하다.
]$ docker container run -d -p 8080:80 nginx

# 컨테이너의 DNS 서버를 지정한다.
]$ docker container run -d --dns 192.168.1.1 nginx

# 컨테이너에 MAC 주소를 설정한다.
]$ docker container run -d --mac-address="92:d0:c6:0a:29:33" centos
fc4c1d58aa680d1caf0f8062589a80218b8bd81d4fa3b86c949e93982c2c3021
]$ docker container inspect --format="{{ .Config.MacAddress }}" fc4c
92:d0:c6:0a:29:33

# 호스트명과 IP 주소를 정의한다.
]$ docker container run -it --add-host test.com:192.168.1.1 centos
[root@fa6fcb9b4701 /]# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
192.168.1.1	test.com
172.17.0.2	fa6fcb9b4701

# 호스트명을 설정한다.
]$ docker container run -it --hostname www.test.com --add-host node1.test.com:192.168.1.1 centos
[root@www /]# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
192.168.1.1	node1.test.com
172.17.0.2	www.test.com www
```

### 3.1.4 --net 옵션의 지정

설정값|설명
---|---
bidge|브리지 연결(기본값)을 사용한다.
none|네트워크에 연결하지 않는다.
container:[name \| id]|다른 컨테이너의 네트워크를 사용한다.
host|컨테이너가 호스트 OS의 네트워크를 사용한다.
NETWORK|사용자 정의 네트워크를 사용한다.

### 3.1.5 사용 예시

```bash
# 외부 브리지 네트워크 드라이버를 사용하여 'webap-net'이라는 이름의 네트워크를 작성후,
# 작성한 네트워크 상에서 컨테이너를 실행
]$ docker network create -d bridge webap-net
]$ docker container run --net=webap-net -it centos
[root@3a53bb8e5ce8 /]# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.18.0.2	3a53bb8e5ce8
```

---

## 4. 자원지정 후 컨테이너 생성 및 실행
### 4.1 물리적 자원을 지정하여 컨테이너 생성 및 실행을 한다.
### 4.1.1 자원 지정 후 생성 및 실행 구문
```bash
docker container run [자원 옵션] 이미지명[:태그명] [인수]
```

### 4.1.2 지정할 수 있는 주요 옵션

옵션 | 설명
---|---
--cpu-shares, -c|CPU의 사용 배분(비율)
--memory, -m|사용할 메모리를 제한하여 실행 (단위는 b,k,m,g 중 하나)
--volume=[호스트의 디렉토리]:[컨테이너의 디렉토리], -v|호스트와 컨테이너의 디렉토리 공유

### 4.1.3 사용 예시

```bash
# CPU 시간의 상대 비율과 메모리 사용량을 지정
# --cpu-shares 기본값은 1024 이다. 반을 사용하고 싶으면 512 를 넣는다.
]$ docker container run --cpu-shares=512 --memory=1g centos

# 호스트 OS 와 컨테이너 안의 디렉토리를 공유하고 싶을 때는 -v(--volume) 옵션을 지정
# 예를 들어 호스트의 /Users/username/Documents/html 폴더를 컨테이너의
# /usr/share/nginx/html 디렉토리와 공유하고 싶을 때는 아래의 명령을 실행
]$ docker container run -v /Users/username/Documents/html:/usr/share/nginx/html nginx
```

---

## 5. 컨테이너를 생성 및 시작하는 환경을 지정
### 5.1 컨테이너의 환경변수 및 컨테이너 안의 작업 디렉토리 등을 지정하여 컨테이너를 생성/실행
### 5.1.1 환경설정 구문

```bash
docker container run [환경설정 옵션] 이미지명[:태그명] [인수]
```

### 5.1.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
--env=[환경변수], -e| 환경변수를 설정한다.
--env-file=[파일명]|환경변수를 파일로부터 설정한다.
--read-only=[true \| false]|컨테이너의 파일 시스템을 읽기 전용으로 만든다.
--workdir=[path], -w| 컨테이너의 작업 디렉토리를 지정한다.
-u, --user=[사용자명]|사용자명 또는 UID를 지정한다.

### 5.1.3 사용 예시

```bash
# 컨테이너 시작시 환경변수를 설정하려면 명령에 -e 옵션을 지정하여 실행
]$ docker container run -it -e foo=bar centos /bin/bash

# 환경변수를 정의한 파일로부터 일괄적으로 등록하고 싶은 경우는 파일로부터 읽어와 실행한다.
]$ cat env_list
hoge=fuga
foo=bar

]$ docker container run -it --env-file=env_list centos /bin/bash

# 작업 디렉토리 지정하여 실행하는 경우
]$ docker container run -it -w=/tensorflow centos /bin/bash
[root@xxxxxxx]# pwd
/tensorflow
```

---

## 6. 가동 컨테이너 목록 표시
### 6.1 가동중인 컨테이너의 목록을 확인 할 수 있다.

### 6.1.1 컨테이너 목록 확인 구문
```bash
docker container ls [option]
```

### 6.1.2 지정할 수 있는 주요 옵션

옵션 | 설명
---|---
--all, -a|실행 중/정지 중인 것도 포함하여 모든 컨테이너를 표시
--filter,-f|표시할 컨테이너의 필터링
--format|표시 포맷을 지정
--last, -n|마지막으로 실행된 n건의 컨테이너만 표시
--latest, -l|마지막으로 실행된 컨테이너만 표시
--no-trunc|정보를 생략하지 않고 표시
--quiet, -q|컨테이너 ID만 표시
--size, -s|파일 크기 표시

### 6.1.3 사용 예시

```bash
]$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
b85486c48d3f        nginx               "nginx -g 'daemon of…"   26 minutes ago      Up 7 seconds        0.0.0.0:8080->80/tcp   amazing_nash

# CONTAINER ID :  컨테이너 ID
# IMAGE : 컨테이너의 바탕이 된 이미지
# COMMAND : 컨테이너 안에서 실행되고 있는 명령
# CREATED : 컨테이너 작성 후 경과 시간
# STATUS : 컨테이너의 상태(restarting|running|paused|exited)
# PORTS : 할당된 포트
# NAMES : 컨테이너 이름

# 표시할 컨테이너의 필터링옵션은 -f
]$ docker container ls -a -f name=test1
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
1d8747fe7317        centos              "/bin/bash"         2 hours ago         Exited (127) 2 hours ago                       test1
```

### 6.1.4 출력형식의 지정

플레이스 홀더|설명
---|---
.ID|컨테이너 ID
.Image|이미지 ID
.Command|실행 명령
.CreatedAt|컨테이너가 작성된 시간
.RunningFor|컨테이너의 가동 시간
.Ports|공개 포트
.Status|컨테이너의 상태
.Size|컨테이너 디스크 크기
.Names|컨테이너명
.Mounts|볼륨마운트
.Networks|네트워크명

### 6.1.5 사용 예시

```bash
# 컨테이너 ID와 가동 중인지 아닌지의 상태(Status)를 콜론으로 구분하여 표시.
]$ $ docker container ls -a --format "{{.Names}}: {{.Status}}"
vigilant_kalam: Exited (1) 15 minutes ago
hopeful_blackwell: Exited (0) 24 minutes ago
amazing_nash: Up 8 minutes
vigorous_turing: Exited (0) 35 minutes ago
blissful_lovelace: Exited (0) 38 minutes ago
quirky_mendel: Exited (0) 49 minutes ago

# 컨테이너 목록을 표 형식으로 출력한다.
]$ docker container ls -a --format "table {{.Names}}\t{{.Status}}\t {{.Mounts}}"
NAMES               STATUS                            MOUNTS
vigilant_kalam      Exited (1) 17 minutes ago
hopeful_blackwell   Exited (0) 26 minutes ago         /Users/usernam…
amazing_nash        Up 10 minutes                     /Users/usernam…
vigorous_turing     Exited (0) 37 minutes ago         /Users/usernam…
blissful_lovelace   Exited (0) 39 minutes ago         /Users/usernam…
quirky_mendel       Exited (0) 51 minutes ago
```

---

## 7. 컨테이너 가동 확인
### 7.1 컨테이너들의 가동상태를 확인한다.

### 7.1.1 컨테이너 가동상태 확인 명령 구문

```bash
docker container stats [컨테이너 식별자]
```

### 7.1.2 실행 예시

```bash
# 컨테이너의 가동을 확인한다.
]$ docker container stats webserver
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
1d8747fe7317        test1               0.00%               0B / 0B             0.00%               0B / 0B             0B / 0B             0

# CONTAINER ID : 컨테이너 식별자
# NAME : 컨테이너 명
# CPU % : CPU 사용률
# MEM USAGE / LIMIT : 메모리 사용량/컨테이너에서 사용할 수 있는 메모리 제한
# MEM % : 메모리 사용율
# NET I/O : 네트워크 I/O
# BLOCK I/O : 블록 I/O
# PIDS : PID(Windows 컨테이너 제외)

# 실행중인 프로세스 확인
]$ docker container top amazing_nash
```

