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

    $ cd ${REDIS_HOME}                             ## sentinel 구성시 최소 3개이상의 conf 파일을 생성해야 한다.
    $ cp sentinel conf sentinel_11001.conf
    $ cp sentinel conf sentinel_11002.conf
    $ cp sentinel conf sentinel_11003.conf
    
#### 아래와 같이 conf 파일에 환경설정을 정의

# 11001.conf ~ 11003.conf 
# 센티널이 실행될 포트 (이부분은 포트별로 다르게 설정) 
port 11001 
pidfile "/var/run/redis-sentinel_11001.pid" 
logfile "/var/log/sentinel_11002.log" 

# 센티널이 감시할 레디스 Master 인스턴스 정보 
# sentinel monitor mymaster <redis master host> <redis master port> <quorum> 
sentinel monitor mymaster 127.0.0.1 6379 2 
# 센티널이 Master 인스턴스에 접속하기 위한 패스워드를 넣어줍니다. 
sentinel auth-pass mymaster foobared # 센티널이 Master 인스턴스와 접속이 끊겼다는 것을 알기 위한 최소한의 시간입니다. sentinel down-after-milliseconds mymaster 30000 # 페일오버 작업 시간의 타임오버 시간을 정합니다. # 기본값은 3분입니다. sentinel failover-timeout mymaster 180000 # 이 값은 Master로부터 동기화 할 수 있는 slave의 개수를 지정합니다. # 값이 클수록 Master에 부하가 가중됩니다. # 값이 1이라면 Slave는 한대씩 Master와 동기화를 진행합니다. sentinel parallel-syncs mymaster 1

출처: https://mycup.tistory.com/282 [IT.FARMER]

출처: https://mycup.tistory.com/282 [IT.FARMER]

출처: https://mycup.tistory.com/282 [IT.FARMER]


## Tomcat Session Manager에서의 Redis 사용법

* 아래의 링크 참조
* https://github.com/redisson/redisson/tree/master/redisson-tomcat 
