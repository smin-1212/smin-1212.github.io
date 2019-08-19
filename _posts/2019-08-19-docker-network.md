---
layout: post
title: Docker 네트워크 조작
tags: [docker]
---

# Docker 네트워크 조작
### Docker 컨테이너끼리 통신할 때 Docker 네트워크를 통해 수행한다.

---

## 1. 네트워크 목록 표시
### 1.1 Docker 의 네트워크 목록을 확인한다.
### 1.1.1 네트워크 확인 구문
```bash
docker network ls [옵션]
```

### 1.1.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
-f, --filter=[]|출력을 필터링 한다.
--no-trunc|상세 정보를 출력한다.
-q, --quiet|네트워크 ID 만 표시된다.

### 1.1.3 실행 예시
```bash
]$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
e46c31150d00        bridge              bridge              local
4903bd1e683c        host                host                local
05bb85d8dfc1        none                null                local
2abf0b599c3c        webap-net           bridge              local

# 상세정보를 출력한다.
]$ docker network ls --no-trunc
NETWORK ID                                                         NAME                DRIVER              SCOPE
e46c31150d000baf24cbb1b8cc5c1fd92f77b8ca03df53dda2b22be4970c76db   bridge              bridge              local
4903bd1e683cd4e8aae267a09dc98eda96142485780a5adde96ab31fc436fbd1   host                host                local
05bb85d8dfc1add4445252afdbe61cc25f1c0a48c84558ac5cdedbd135d31fd0   none                null                local
2abf0b599c3cd895a7921f5ab96e4ed0c290507aaabc3006b2d84a009b1e54d3   webap-net           bridge              local

# 필터링 예시
]$ docker network ls -q --filter driver=bridge

# 네트워크를 명시적으로 지정하지 않고 Docker 컨테이너를 시작하면 기본값인 Bridge 네트워크로
# Docker 컨테이너를 시작한다.
]$ docker container run -itd --name=sample ubuntu:latest
]$ docker container inspect sample
...(생략)
"Networks": {
    "bridge": {
        "IPAMConfig": null,
        "Links": null,
        "Aliases": null,
        "NetworkID": "e46c31150d000baf24cbb1b8cc5c1fd92f77b8ca03df53dda2b22be4970c76db",
        "EndpointID": "1828012dc3e398af074701fa692d20ca1a5e2344100e24dda3e1ee41a4724e29",
        "Gateway": "172.17.0.1",
        "IPAddress": "172.17.0.3",
        "IPPrefixLen": 16,
        "IPv6Gateway": "",
        "GlobalIPv6Address": "",
        "GlobalIPv6PrefixLen": 0,
        "MacAddress": "02:42:ac:11:00:03",
        "DriverOpts": null
    }
...(생략)
```

#### 오버레이 네트워크 ?
> 오버레이 네트워크(overlay network) 는 물리 네트워크 상에서 소프트웨어적으로 에뮬레이트한 네트워크를 말한다. 가상 네트워크라고 부르기도 한다.
>
> 여러개의 호스트에 걸친 네트워크를 구성할 때 사용할 수 있다.

---

## 2. 네트워크 작성
### 2.1 새로운 네트워크를 작성한다.
### 2.1.1 네트워크 작성 구문
```bash
docker network create [option] 네트워크
```

### 2.1.2 지정할 수 있는 주요 옵션

옵션|설명
---|---
--dirver, -d|네트워크 브리지 또는 오버레이(기본값은 bridge)
--ip-range|컨테이너에 할당하는 IP 주소의 범위를 지정
--subnet|서브넷을 CIDR 형식으로 지정
--ipv6|IPv6 네트워크를 유효화 할지 말지(true/false)
-label|네트워크에 설정하는 라벨

### 2.1.3 실행 예시

```bash
# 브리지 네트워크 작성 및 확인
]$ docker network create --driver=bridge web-network
]$ docker network ls --filter driver=bridge
NETWORK ID          NAME                DRIVER              SCOPE
e46c31150d00        bridge              bridge              local
1a399aaae7b8        web-network         bridge              local
2abf0b599c3c        webap-net           bridge              local
```

---

## 3. 네트워크 연결
### 3.1 컨테이너를 Docker 네트워크에 연결한다.
### 3.1.1 네트워크 연결 구문
```bash
docker network connect [option] 네트워크 컨테이너
```

### 3.1.2 지정할 수 있는 옵션

옵션|설명
---|---
--ip|IPv4 주소
--ip6|IPv6 주소
--alias| 앨리어스 명
--link|다른 컨테이너에 대한 링크

### 3.1.3 실행 예시
```bash
]$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
e46c31150d00        bridge              bridge              local
4903bd1e683c        host                host                local
05bb85d8dfc1        none                null                local
1a399aaae7b8        web-network         bridge              local
2abf0b599c3c        webap-net           bridge              local

# 네트워크에 대한 연결
]$ docker network connect web-network webfront
# 컨테이너 네트워크 확인
]$ docker container inspect webfront
...(중략)
"web-network": {
    "IPAMConfig": {},
    "Links": null,
    "Aliases": [
        "29649f6ab2ee"
    ],
    "NetworkID": "1a399aaae7b8ab1eef5f5ea794de27945a3f020d0035498a5ad5a1ed7c191a45",
    "EndpointID": "a5245b05d4b2b07d347f4972dcf606ddd61eaa62e35c3d4a6b66e2c4ad4740cb",
    "Gateway": "172.19.0.1",
    "IPAddress": "172.19.0.2",
    "IPPrefixLen": 16,
    "IPv6Gateway": "",
    "GlobalIPv6Address": "",
    "GlobalIPv6PrefixLen": 0,
    "MacAddress": "02:42:ac:13:00:02",
    "DriverOpts": {}
}
...(중략)

# 네트워크를 지정한 컨테이너 시작
]$ docker container run -itd --name=webap --net=web-network nginx

# 네트워크 연결 해제
]$ docker network disconnect web-network webfront
]$ docker container inspect webfront
...(중략)
"Networks": {
    "bridge": {
        "IPAMConfig": null,
        "Links": null,
        "Aliases": null,
        "NetworkID": "e46c31150d000baf24cbb1b8cc5c1fd92f77b8ca03df53dda2b22be4970c76db",
        "EndpointID": "d0577bd46814f9136549c7d09308dbeb2576163b3199633b9466cd369318481f",
        "Gateway": "172.17.0.1",
        "IPAddress": "172.17.0.4",
        "IPPrefixLen": 16,
        "IPv6Gateway": "",
        "GlobalIPv6Address": "",
        "GlobalIPv6PrefixLen": 0,
        "MacAddress": "02:42:ac:11:00:04",
        "DriverOpts": null
    }
}
...(중략)
```

---

## 4. 네트워크 상세 정보 확인
### 4.1 네트워크의 상세 정보를 확인한다.
### 4.1.1 상세정보 확인 구문
```bash
docker network inspect [option] 네트워크
```

### 4.1.2 실행 예시
```bash
]$ docker network inspect web-network
[
    {
        "Name": "web-network",
        "Id": "1a399aaae7b8ab1eef5f5ea794de27945a3f020d0035498a5ad5a1ed7c191a45",
        "Created": "2019-08-19T08:26:42.75808759Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "1ceed352a4d8f7f65207890c88b02e07ecd15e7ee2edade117b5f6349c1ab017": {
                "Name": "webap",
                "EndpointID": "5ea92c6d0485fdc1a5d6062d216bc1555c21ab1597bf016bd140ead65f7ac8ba",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

---

## 5. 네트워크 삭제
### 5.1 네트워크를 삭제한다.
### 5.1.1 네트워크 삭제 구문

```bash
docker network rm [option] 네트워크
```

### 5.1.2 실행 예시
```bash
# network 와 container 에 대한 연결이 모두 해제되어 있어야 한다.
]$ docker network disconnect web-network webap
]$ docker network rm web-network
web-network
```

