## HyperFrameOE-Redis

업로드된 바이너리는 HyperFrame Open Edition 제품 설치를 위한 파일 바이너리임

## 설치 파일

### Redis

* Version : Redis 6.2.3
* Note : https://redis.io/download

## 검증 환경

* CentOS Linux release 7.7.1908

## 설치 및 실행

### 1) Redis 압축 풀기

    $ cd ${INSTALL_HOME}
    $ tar -zxvf redis-6.2.3.tar.gz

### 2) Redis 설치

    $ cd ${REDIS_HOME}
    $ make install

### 4) Redis 실행

    $ cd ${REDIS_HOME}/src
    $ ./redis-server
    
### 5) Redis 종료

    $ cd ${REDIS_HOME}/src
    $ ./redis-cli shutdown

## 디렉토리 구조

      Redis
      ├── deps
      ├── redis.conf 
      ├── runtest            
      ├── runtest-cluster            
      ├── runtest-moduleapi
      ├── runtest-sentinel       
      ├── sentinel.conf
      ├── src
      ├── tests
      └── utils       

## 버전 확인

    $ cd ${REDIS_HOME}/src
    $ ./redis-server --version
    
## redis-cli 접속

* 호스트명과 포트번호를 생략하면 localhost의 6379로 접속

### 1) localhost:6379 접속
    $ cd ${REDIS_HOME}/src
    $ redis-cli
    
### 2) 원격접속
    $ redis-cli -h <호스트명> -p <포트번호>    ## ex) ./redis-cli -h 127.0.0.1 -p 6379

### 3) 정보보기
    $ redis-cli info
    
### 4) help
    $ redis-cli help

### 5) 모니터링
    $ redis-cli monitor

## HA 구성하기 (Master slave)

### 1) Sentinel 방식

#### 1. sentinel config 설정

    $ cd ${REDIS_HOME}                             ## sentinel 구성시 최소 3개이상의 conf 파일을 생성해야 한다.
    $ cp sentinel conf sentinel_11001.conf
    $ cp sentinel conf sentinel_11002.conf
    $ cp sentinel conf sentinel_11003.conf


## Tomcat Session Manager에서의 Redis 사용법

* 아래의 링크 참조
* https://github.com/redisson/redisson/tree/master/redisson-tomcat 
