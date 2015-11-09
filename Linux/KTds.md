#

password : Pa$$w0rd

command
net users imsiuser * /add
net localgroup administrators imsiuser /add


##VM 설정

###새로만들기

이름 : centos15a, centos15b
linux redhat 64bit

memory 1024


####파일 > 환경설정 > 입력 > 가상머신 > 호스트키 조합
호스트키 조합 : left alt키 로 변경하기

netcidr 설정
10.0.15.0/24


####su
서브스테틱트? 유저 = 유저변경

```bash
$ su
Password:
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0
```
사용자 변경후, vi로 네트워크 정보파일 열어서 'ONBOOT' 변수값 'yes' 로 변경


vi /etc/sysconfig/network-scripts/ifcfg-eth0
에 접속해서
booton = yes 로 변경
