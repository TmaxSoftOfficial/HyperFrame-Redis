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

#### 1. sentinel config 복사

    $ cd ${REDIS_HOME}                             ## sentinel 구성시 최소 3개이상의 conf 파일을 생성해야 한다.
    $ cp sentinel conf sentinel_11001.conf
    $ cp sentinel conf sentinel_11002.conf
    $ cp sentinel conf sentinel_11003.conf
    
#### 1.1 sentinel config 설정

    cd redis_home_dirctory 
    cp sentinel.conf sentinel_1001.conf ~ 1003.conf 

    # 11001.conf ~ 11003.conf 
    # 센티널이 실행될 포트로 각각 설정 
    port 11001 pidfile "/var/run/redis-sentinel_11001.pid" 
    logfile "/var/log/sentinel_11002.log" 

    # 센티널이 감시할 레디스 Master 인스턴스 정보 기입 
    sentinel monitor mymaster 127.0.0.1 6379 2 

    # 센티널이 Master 인스턴스에 접속하기 위한 패스워드 정보
    sentinel auth-pass mymaster foobared 

    # 센티널이 Master 인스턴스와 접속이 끊겼다는 것을 알기 위한 최소한의 시간 설정
    sentinel down-after-milliseconds mymaster 30000 
    
    # fail-over 작업 시간의 타임오버 시간을 지정 (default : 3분)
    sentinel failover-timeout mymaster 180000 

    # Master로부터 동기화 할 수 있는 slave의 개수를 지정
    sentinel parallel-syncs mymaster 1
    

#### 2. master 및 slave config 설정

    # master / slave 종료 
    redis-cli -p 6379 -a foobared shutdown 
    redis-cli -p 6380 shutdown 
    redis-cli -p 6381 shutdown 

    # masterauth 및 requirepass 설정 
    vi ${REDIS_HOME}/src/6379.conf 
    vi ${REDIS_HOME}/src/6380.conf 
    vi ${REDIS_HOME}/src/6381.conf 

    # 6379.conf ~ 6381.conf 
    masterauth foobared 
    requirepass foobared

#### 3. redis 재실행

    cd ${REDIS_HOME}/src/redis-server ./6379.conf & redis-server ./6380.conf & redis-server ./6381.conf & #

    # requirepass 설정후 shutdown 명령어 
    redis-cli -p 6379 -a foobared shutdown 
    redis-cli -p 6380 -a foobared shutdown 
    redis-cli -p 6381 -a foobared shutdown

#### 4. sentinel 실행 및 종료

#### 5. 각 노드의 정보 확인

## Tomcat Session Manager에서의 Redis 사용법

* 아래의 링크 참조
* https://github.com/redisson/redisson/tree/master/redisson-tomcat 
