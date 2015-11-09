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


##파일 > 환경설정 > 입력 > 가상머신 > 호스트키 조합
호스트키 조합 : left alt키 로 변경하기
netcidr 설정
10.0.15.0/24
