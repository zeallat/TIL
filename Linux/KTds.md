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



####cat
짧은 내용 확인용



###설정 확인하기
```bash
$ yum install bind
$ rpm -qc bind
```



```bash
$ cd ~
$ cd ~user1
```




#Day2



```bash
$ man ls | grep \\-l
```
manual에서 -l에 관해서만 찾아보는 방법


##유저 추가
```bash
$ useradd user1
$ passwd user1
$ cat /etc/passwd
```

'/etc/passwd'경로에 유저정보가 있다.


openssh-server
openssh-client

ssh가 왕중요..

ssh가 돌고있는지 확인해야함..




##su
```bash
$ su root
$ su -root
$ su
```



##cd
(change directory)
```bash
$ cd /home/adminuser/
$ cd ~user1
$ cd ~
$ cd
```



##-R 옵션
recurse옵션 : 하위~ 관련



##remove
remove는 복구 불가능.
delete와 다른개념이다.




##외부에서 공부하기
AWS에서 가상머신 생성해서
모바일환경에서 ssh 클라이언트로 접속해서 명령어 테스트하기



##파일 링크
심볼릭 링크, 하드 링크 이렇게 두가지가 있다.


##fallocate
```bash
$ fallocate -l 10M smallfile.avi
```
빈 파일 생성





##파일 찾기
```bash
$ find / -name smallfile.avi
```

###에러구문 제외하고 찾기
```bash
$ find / -name smallfile.avi 2> /dev/null
```
에러구문은 /dev/null 경로로 보내라 -> 화면에 보이지마라.. 라는 뜻

###찾은 파일 삭제하기
```bash
$ find / -name smallfile.avi -exec rm -rf {} \;
```




#Day 3

forward - proxy
  forward와 proxy는 비슷하면서도 다르다
redirect


###conemu에서 ssh접속하기

```bash
$ ssh 10.225.152.202 -l adminuser
adminuser@10.225.152.202's password:
```


#Day 4

#### 백그라운드 작업하기
명령 뒤에 & 붙여준다






```bash
$ cat /etc/fstab
```




```bash
$ fdisk -l | grep /dev

```



partition : fdisk
format : mkfs
mkdir
mount
cf
df -h
