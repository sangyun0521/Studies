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
```
mkdir mysql-files
chown mysql:mysql mysql-files 
chmod 750 mysql-files

bin/mysqld --initialize --user=mysql
```

### trouble shooting
1. 기본 data 경로가 바뀐듯 mysql-files -> data
2. 권한 관련 오류로 mysqld user옵션을 주려면 sudo로 실행 해주어야함
3. Innodb 설정 관련 오류
```
2025-03-08T07:49:41.796720Z 1 [ERROR] [MY-012592] [InnoDB] Operating system error number 2 in a file operation.
2025-03-08T07:49:41.796733Z 1 [ERROR] [MY-012593] [InnoDB] The error means the system cannot find the path specified.
2025-03-08T07:49:41.796737Z 1 [ERROR] [MY-012594] [InnoDB] If you are installing InnoDB, remember that you must create directories yourself, InnoDB does not create them.
2025-03-08T07:49:41.796740Z 1 [ERROR] [MY-012646] [InnoDB] File ./ibdata1: 'open' returned OS error 71. Cannot continue operation
```
innodb 생성시 파일을 미리 생성해두어야함
근데 기본 설정 경로가 어딘지 모르겠어서그냥 /etc/mysql/my.conf 설정 파일을 만들어서 기본 경로 설정해주고 
innodb data 경로에 ibdata 파일을 미리 만들어 둠
안되네..



