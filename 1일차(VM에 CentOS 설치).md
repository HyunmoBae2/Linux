## 오라클 VM 설치
https://www.virtualbox.org/wiki/Downloads

## 서버 생성
#1.Oracle VM VirtualBox -> 새로만들기 -> 이름, 설치할 폴더 설정 
(이름적으면 종류,버전이 자동선택됨)

2.메모리설정 ( Linux = 2G, Window =4G 권장 )

3.하드디스크 설정 (지금 새 가상 하드디스크 만들기)

4.하드디스크 파일 종류 설정 (VDI = Oracle VM에서만 사용, VHD = Oracle VM, Hyper-V, Xen 등의 
다른 가상머신 Software에서도 사용 가능), 동적,고정크기 할당, 가상하드디스크 만들기(10G이상 권장)

## OS 설치 (CentOS)
ISO 파일 다운(CentOS 7.9 Minimal)
http://mirror.kakao.com/centos/7.9.2009/isos/x86_64/ (CentOS-7-x86_64-Minimal-2009.iso)

## 머신 설정
설정에서 메모리변경, cpu등 설정(가상머신 속도에 영향)

## 네트워크 설정(NIC)
네트워크 - 어댑터1(NAT), 어댑터2(호스트전용 어댑터,Host-Only Ethernet Adapter)


NAT = 내부 망(사설IP 대역)에서 외부 망(공인IP 대역)으로 나갈 때 하나의 공인IP로 변환되는 방식 (내PC 외부와의 통신)

호스트 전용 어댑터 = 가상 머신끼리 통신이 가능하도록 새로운 대역에 사설IP를 부여 해 주는 방식 (내PC 내부에서만 통신)

어댑터에 브릿지 = 내PC가 공유기를 사용 중이라면 공유기에서 해당 NIC로 내 PC와 같은 대역의 사설 IP를 부여 해주는 방식 (공유기에 노트북을 하나 더 연결 했다고 보시면 됨)

## ISO 파일 삽입
저장소 -> CD+(광학드라이브 추가, 아까 다운한 CentOS)

## 실행 및 기본 설정
호스트 키 조합,언어 등 설정 후 디스크설정(파티션 자동 할당), 설치 및 Root암호 설정, 재부팅

## 가상머신 IP 부여
1. 사전확인 : 관리자화면-> 파일 -> 호스트 네트워크 관리자

3. DHCP 체크해제 -> 하단속성에서 "수동으로 어댑터 설정" ('IPv4 주소와 서브넷 마스크' 기억)

(위 어댑터는 내PC에 vm용으로 설치된 네트워크 카드, 해당어댑터로 가상머신의 통로(Gateway)로 사용할 예정)

## IP 세팅
```
cd /etc/sysconfig/network-scripts/ \\IP세팅 폴더로 이동

ls \\폴더내파일 확인
```
ifcfg-enp0s3(어댑터1 -> NAT), ifcfg-enp0s8(어댑터2 -> Host Only) * ifcfg 뒤는 어댑터이름

## IP, Gateway, Netmask 입력
```
vi ifcfg-enpos8 \\파일 열기

BOOTPROTO=none \\line 4 DHCP사용여부

ONBOOT=yes \\line 15 network 서비스 재기동시 활성화여부

IPADDR=192.168.56.[2~254] \\IP주소

GATEWAY=[아까 확인한 IP] \\게이트웨이 주소

NETMASK=[확인한 IP] \\255.255.255.0 = C 클래스
```
## 네트워크 서비스 재기동
```
systemctl restart network

ip a \\ip리스트 확인
```
## 원격 접속하기
putty다운

IP, Port입력 후 Connection type 확인 후 open (SSH)

root, 비밀번호 입력 후 접속

## etc
네트워크에서 안쓰는 ip주석처리

```ping 8.8.8.8 \\구글에 ping을 전송하여 네트워크 상태 확인```

## cent os(7.9)에서 ntp설정하는법
ntp 서버 IP : ping time.bora.net (203.248.240.140)
```
yum install ntp \\ntp 다운

vi /etc/ntp.conf \\설정

#Use public servers from the pool.ntp.org project.
#Please consider joining the pool (http://www.pool.ntp.org/join.html).
\\[기존의 서버들은 사용하지 않으므로 주석처리]

server [설정하고싶은 서버(기준시간)]

설정후 service ntpd start \\ntpd 서비스 시작

chkconfig ntpd on \\매 시스템 시작될때마다 구동

ntpq -p \\ntp상태확인
```
## ntpq -p 명령어
remote : sync 하고있는 ntp server

현재 동기화중인 NTP서버는 [ * ], secondary NTP 서버는 [ + ] 로 표시

refid : remote의  reference 주소 ( 서버가 바라보는 주소 )

st : remote stratum ( ntp 계층 )

t : 시간을 받아보는 방식 ( uniquest 일대일, multicast 일대다, broadcast 일대일 )

when : NTP 서버로부터 데이터를 수신한 후 경과 시간, 초 단위이며 수신받으면 0으로 초기화

poll : NTP 서버에 sync (동기화) 요청 주기, 초 단위

reach : 최근 8번 poll 요청에 대한 응답 여부 (1,3,7,17,37,77,177,377이면 정상)

delay : network 지연시간이며, millisecond 단위

offset : NTP서버와 로컬(자신)의 시간차이, m/s단위

disp : offset에 대한 분산
