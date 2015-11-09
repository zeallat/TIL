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

그후 ifconfig로 확인
```
$ ping www.google.com
```
으로 확인





####shutdown
종료

$ shutdown -h now
중지

####hostname

``` bash
$ hostname -I
```



####uname
커널 정보 확인
```bash
$ uname -a
$ uname -n
```



##포트포워딩 설정

버츄얼박스에서
파일>환경설정>네트워크>NatNetWork>도구>포트포워딩
에서 추가하기 누른뒤
호스트IP에 데스크탑 아이피 입력, 게스트 IP에는 가상머신 ip 입력.
각각포트에는 임의의 포트를 동일하게 입력.



####winscp
윈도우 -> 리눅스로 파일 업로드 가능하도록 만들어주는 프로그램

####touch
파일 생성 명령어




ssh client
깃 배쉬
git bash
ConEmu



git bash 설치 -> conEmu 설치


