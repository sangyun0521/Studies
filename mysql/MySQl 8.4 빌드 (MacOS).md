https://dev.mysql.com/doc/refman/8.0/en/source-installation-methods.html

# Prerequisites
* CMake : 3.31.6
* Make (>= 3.75) : 3.81 
* Xcode (>= 10) : 16.2 
* C++, C 컴파일러 : clang 16.0.0, 
* OpenSSL : 3.3.6
* Boost : 1.82.0
* ncurses : 6.5
* Perl : 5.34.1
* bison (3.0.4 >=) : 3.8.2 
* git : 2.39.5

# cmake 
* 사용자 환경에 맞는 빌드 파일을 생성
* CMakeLists.txt 파일을 읽어서 수행
```
mysql이 있는 디렉토리에서

mkdir build
cd build
cmake ../mysql-server
```
* 터미널에도 에러가 뜨지만 CMakeConfigureLog.yaml 파일에 더 자세한 로그가 나온다.
* 성공하면 build 디렉토리에 makefile이 생성됨
### trouble shooting 
```
CMakeConfigureLog.yaml

에러 내용 : 
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance

해결 : 
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -license 
agree
```

# make 
* 소스 코드 컴파일하고 실행 파일을 생성
* Makefile을 읽어 수행
```
make -j(nproc)
sudo make install (/usr/local/mysql에 설치)
```


# Setup

### config 파일 생성
/etc/my.cnf
```
    1 # vi /etc/my.cnf
    2 [client]
    3 port  = 3306
    4 socket  = /tmp/mysql.sock
    5
    6 [mysqld]
    7 user  = isang-yun
    8 port  = 3306
    9 basedir = /usr/local/mysql
   10 datadir = /Users/isang-yun/workspace/build/data
   11 log-error= /Users/isang-yun/workspace/build/log/mysql_error.log
```
절대 경로로 작성해야 오류 발생 안함

### 데이터베이스 생성
```
mkdir data
chown [OS사용자]:[OS사용자] data
chmod 750 data

mysqld --defaults-file=/etc/my.cnf --initialize-insecure (기본 데이터베이스 생성, root password 없이 생성)
```

### trouble shooting
1. 기본 data 경로가 바뀐듯 mysql-files -> data
2. 권한 관련 오류로 mysqld user옵션을 주려면 sudo로 실행 해주어야함 (디렉토리 권한과 일치 해야함)
3. Innodb 설정 관련 오류
```
2025-03-08T07:49:41.796720Z 1 [ERROR] [MY-012592] [InnoDB] Operating system error number 2 in a file operation.
2025-03-08T07:49:41.796733Z 1 [ERROR] [MY-012593] [InnoDB] The error means the system cannot find the path specified.
2025-03-08T07:49:41.796737Z 1 [ERROR] [MY-012594] [InnoDB] If you are installing InnoDB, remember that you must create directories yourself, InnoDB does not create them.
2025-03-08T07:49:41.796740Z 1 [ERROR] [MY-012646] [InnoDB] File ./ibdata1: 'open' returned OS error 71. Cannot continue operation
```
- 파일 권한 확인
- 절대 경로 확인

### 시작
```
mysqld_safe --defaults-file=/etc/my.cnf
```

### 접속
```
mysql -u root
show databases
```