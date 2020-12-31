---
title: Linux(ubuntu) commands
categories:
- Linux
toc: true
toc_sticky: true
---

# 자주 쓰는 리눅스 명령어 모음
> 개발/운영 서버는 보통 리눅스 OS를 이용한다. <br>
> 이번 글에서는 우분투를 기준으로 자주 쓰는 리눅스 명령어들을 정리한다. <br>
> 필요한 명령어가 있는지 검색해보고 사용 가능 <br>


## 패키지 관리
```
# 저장소(repository)로부터 사용 가능한 패키지 리스트와 버전 정보를 업데이트
sudo apt-get update 
```

```
# 현재 설치된 모든 패키지를 최신 버전으로 업그레이드
sudo apt upgrade 
```

## 사용자  관리

``` 
# 계정의 현재 홈 디렉토리 확인
echo ~[계정명]
# 계정 홈디렉토리 설정/변경
usermod -d [홈디렉토리] [계정명]
```
```
# 계정 목록 확인
grep /bin/bash /etc/passwd
```
```
# 새로운 계정 생성
sudo adduser [username]
```
```
# 계정의 홈 디렉토리 설정
sudo usermod -d [경로] [username]
```

## 파일/폴더 관리
```
# 리눅스 간 파일 전송
scp  ./test.txt  [계정]@[IP]:[경로]
```
```
# 파일 변경 추적하여 로그 보기
tail -f [파일명]
```
```
# tar로 파일 묶기/풀기 (압축 효과는 없음)
tar -cvf [파일명.tar] [폴더명]
tar -xvf [파일명.tar]
# tar.gz 압축/풀기 
tar -cvfz [파일명.tar.gz] [폴더명]
tar -xvfz [파일명.tar.gz]
# tar.gz 분할 압축/풀기
tar cvzf - [폴더명] | split -b 100m - [파일명.tar.gz] (100MB 단위로 분할)
cat [파일명.tar.gz]* | tar xvfz -
```
```		
# 파일 소유 사용자나 소유 그룹 변경
chown [소유 사용자] [파일명]
chown -R [소유 사용자] [디렉토리명]
chown [소유 사용자].[소유 그룹] [파일명]
chown -R [소유 사용자].[소유 그룹] [디렉토리명]
chgrp [소유 그룹] [파일명]
```
```
# 파일에 대한 보호 모드 설정
chmod u+w [파일명] (소유자에게 쓰기 권한)
chmod g+w [파일명] (소유 그룹에게 쓰기 권한)
chmod o=r [파일명] (other에게 읽기 권한만 부여)
chmod a+w [파일명] (모든 사용자들에게 쓰기 권한을 부여)
chmod a-w [파일명] (모든 사용자들에게 쓰기 권한을 제거)
chmod g+w [파일명] (소유 그룹에게 쓰기 권한)
chmod 777 [파일명] (모든 사용자에게 모든 권한 부여)
```
```
# 디스크의 남은 용량 확인
df -h (보기 좋게 보여줌)
du -h --max-depth=2 (서브 디렉토리의 depth를 2로 제한)
du -sh [폴더명] (해당 폴더의 총 사용량 확인)
du -ah [폴더명] (현재 디렉토리의 사용량을 파일단위로 출력)
```
```
# 명령어의 실행 파일 위치, 소스 위치, man 페이지파일의 위치를 찾는 명령어
whereis java
# 특정 파일의 위치를 찾는 명령어
locate [검색 키워드]
# 파일 찾기
find . -name "FILE(정규식 가능)"
# 파일 텍스트 검색
grep -r '텍스트' [파일명]
```
```
# 파일 인코딩 확인
file -bi [파일명]
# 파일 인코딩 변환
utf-8 인코딩 파일을 euc-kr로 변환
iconv -c -f utf-8 -t euc-kr [변경 전 파일명] > [변경 후 파일명]
```
```
# SFTP 사용한 파일 다운로드 (https://cccding.tistory.com/100)
sftp [계정]@[IP]
get [파일경로]
```
```
# 파일/폴더 심볼릭 링크
## 생성
ln -s /home/text.txt testlink
ln -s /home/testfolder/ testlink
## 삭제
rm -f testlink
rm -f testlink/
```
```
# 현 위치 파일 수 세기
find . -type f | wc -l
# 서브 디렉토리 미포함
ls -l | grep ^- | wc -l
```




## 프로세스 관리
```
# 그래픽 드라이버 정보 확인, 현재 그래픽 카드 사용 현황 확인
nvidia-smitop
```
```
# 실행 중인 자바 프로세스 보기
ps -ef | grep java
```
```
# 메모리 사용량 순 프로세스 확인
ps -eo user,pid,ppid,rss,size,vsize,pmem,pcpu,time,comm --sort -rss | head -n 11
```
```
# 프로세스 별 스레드 수 확인
ps -Lf -p [PID]
# 열려있는 포트 확인
netstat -tnlp
# 현재 사용중인 프로세스의 실행 경로 찾기
lsof -p [pid]
```
```
# Background 에서 프로세스 실행하기
[명령어] &

#프로세스 실행 -> [Ctrl+Z] -> jobs 명령어로 id 확인
bg %[id]

# Background의 작업을 foreground로 옮기기
fg %[id]

# 작업 죽이기
kill %[id]

# 백그라운드에서 프로세스 실행
## 기본적으로 생성되는 nohup.out 파일 대신 원하는 파일명에 저장
## ssh 접속이 종료되어도 백그라운드 프로세스 유지 됨
nohup [실행할 명령어] > [출력 로그 파일].out &
tail -f nohup.out

## -u: unbuffered, 메모리 필요없이 바로 print
nohup python -u *.py &

# 현재 Stopped 상태의 작업 확인
jobs
```

## OS 성능 확인
```
# 리눅스 프로세스별 cpu, 메모리 사용량
top
# CPU 정보 확인
cat /proc/cpuinfo | more
# CPU 물리 개수 확인
grep "physical id" /proc/cpuinfo | sort -u | wc -l
# CPU 논리 코어 수 확인
grep -c processor /proc/cpuinfo
# CPU당 물리 코어 수 확인
grep "cpu cores" /proc/cpuinfo | tail -1
# 메모리 용량 확인
free -h
```