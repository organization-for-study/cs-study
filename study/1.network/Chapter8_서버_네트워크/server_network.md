# Server network

서비스를 만들면 네트워크와 연결하고 네트워크를 통해 서비스를 제공한다.<br>
이번 챕터에서는 약간 네트워크 엔지니어와 서버 엔지니어 (Backend)<br>
서로의 역할이 겹치는 부분이 있어서 서버 네트워크에 대해 알아보려고 한다.

## 서버 네트워크 구성

**기본적인 설정으로**

1. IP 주소
2. 서브넷 마스크
3. 게이트웨이
4. DNS 서버

### 리눅스 서버 네트워크

리눅스는 CLI 기반의 서버이다.<br>
CentOs , Ubuntu , Debian 등등이 있다.<br>
각각의 설정 파일을 통해 인터페이스 설정을 관리할 수 있다.

#### 인터페이스 설정 파일

ex)
`/etc/sysconfig/network-scripts`

```bash
Type=Ethernet # 인터페이스 타입
ONBOOT=yes # 부팅시 자동으로 인터페이스를 활성화 할지 여부
BOOTPROTO=static # 부팅 시 사용할 프로토콜 (none , dhcp , static)
IPADDR= # IP 주소
NETMASK= # 서브넷 마스크
GATEWAY= # 게이트웨이
DNS1= # 주 DNS 서버
DNS2= # 보조 DNS 서버
```

여기서 말하는 인터페이스는 네트워크 카드를 말한다.<br>

## 서버의 라우팅 테이블

## 네트워크 명령어 