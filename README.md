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

### 3) Redis 실행

    $ cd ${REDIS_HOME}/src
    $ ./redis-server
    
### 4) Redis 종료

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
    $ redis-cli -h <호스트명> -p <포트번호>           ## ex) ./redis-cli -h 127.0.0.1 -p 6379

### 3) 정보보기
    $ redis-cli info
    
### 4) help
    $ redis-cli help

### 5) 모니터링
    $ redis-cli monitor
    
### 6) 데이터 입력 및 확인 (get/set)
    $ ./redis-cli -h localhost -p 6379  
    $ localhost:6379> set key value
    $ OK
    $ localhost:6379> get key
    $ "value"

## Redis Replication (Master Slave) 구성

### 1) Master, Slave 구성

    $ cd ${REDIS_HOME}/utils
    $ install_server.sh
    $ [root@rainbow utils]# ./install_server.sh      ## master(6379), slave1(6380), slave2(6381)로 생성
      Welcome to the redis service installer
      This script will help you easily set up a running redis server

      Please select the redis port for this instance: [6379]     
      Selecting default: 6379
      Please select the redis config file name [/etc/redis/6379.conf] /home/redis6/redis-6.2.3/6379.conf
      Please select the redis log file name [/var/log/redis_6379.log] /home/redis6/logs/reids_6379.log
      Please select the data directory for this instance [/var/lib/redis/6379] /home/redis6/redis-6.2.3/6379
      Please select the redis executable path [] /home/redis6/redis-6.2.3/src/redis-server
      Selected config:
      Port           : 6379
      Config file    : /home/redis6/redis-6.2.3/6379.conf
      Log file       : /home/redis6/logs/reids_6379.log
      Data dir       : /home/redis6/redis-6.2.3/6379
      Executable     : /home/redis6/redis-6.2.3/src/redis-server
      Cli Executable : /home/redis6/redis-6.2.3/src/redis-cli
      Is this ok? Then press ENTER to go on or Ctrl-C to abort.
      Copied /tmp/6379.conf => /etc/init.d/redis_6379
      Installing service...
      Successfully added to chkconfig!
      Successfully added to runlevels 345!
      Starting Redis server...
      Installation successful!
      
### 2) Replication config 설정

    # slave(6380.conf, 6381.conf에 아래내용 설정)
    replicaof 127.0.0.1 6379
    
### 2) redis 실행 및 종료
    
    # 실행
    $ cd ${REDIS_HOME}/src
    $ redis-server ./../6379.conf &
    $ redis-server ./../6380.conf &
    $ redis-server ./../6381.conf &
    
    # 종료
    $ cd ${REDIS_HOME}/src
    $ redis-cli -p 6379 shutdown
    $ redis-cli -p 6380 shutdown
    $ redis-cli -p 6381 shutdown
    
### 3) 프로세스 확인

    $ ps -ef | grep redis
      redis6    9471     1  0 14:47 ?        00:00:03 ./redis-server 0.0.0.0:6379
      redis6    9477     1  0 14:47 ?        00:00:01 ./redis-server 0.0.0.0:6380
      redis6    9483     1  0 14:47 ?        00:00:01 ./redis-server 0.0.0.0:6381
    

## HA 구성 

### 1) Sentinel 방식

#### 1. sentinel config 복사

    $ cd ${REDIS_HOME}                               ## sentinel 구성시 최소 3개이상의 conf 파일을 생성
    $ cp sentinel.conf sentinel_11001.conf
    $ cp sentinel.conf sentinel_11002.conf
    $ cp sentinel.conf sentinel_11003.conf
    
#### 2. sentinel config 설정

    $ cd ${REDIS_HOME} 
    $ vi sentinel.conf sentinel_11001.conf ~ 11003.conf 

    protected-mode no
    port 11001                                       # sentinel_11002.conf, sentinel_11003.conf의 경우도 port와 pidfile 및 logfile을 세팅해서 동일하게 작성
    pidfile "/var/run/redis-sentinel_11001.pid"
    logfile "/home/redis6/logs/sentinel_11001.log"
    sentinel monitor mymaster 127.0.0.1 6379 2

#### 3. redis 재실행 
 
* Redis Replication (Master Slave) 구성의 3) redis 실행 및 종료 참고    

#### 4. sentinel 실행 및 종료

    # sentinel은 다음의 명령어로 실행
    $ cd ${REDIS_HOME}/src
    $ ./redis-sentinel ./../sentinel_11001.conf &   
    $ ./redis-sentinel ./../sentinel_11002.conf &
    $ ./redis-sentinel ./../sentinel_11003.conf &
    
    # sentinel은 다음의 명령어로 종료
    $ redis-cli -p 11001 shutdown
    $ redis-cli -p 11002 shutdown
    $ redis-cli -p 11003 shutdown


#### 5. 각 노드의 정보 확인

    # redis sentinel에 접속해서 info 명령어로 각 노드의 정보 확인
    $ redis-cli -p 11001 ~ 11002
    $ 127.0.0.1:11001> info    
    
### 2) Cluster 방식    
    
#### 1. master로만 구성한 Clustering

    # 마스터 노드로 사용할 redis instance를 복사해서 3개를 만든 후, Master 노드 환경 파일을 수정
    $ cd ${REDIS_HOME}
    $ vi 6379.conf ~ 6381.conf 까지 작성한 후 아래와 같이 설정
    appendonly yes 
    cluster-enabled yes 
    cluster-config-file nodes-6379.conf 
    cluster-node-timeout 15000 

    # Redis 6379, 6380, 6381 재기동

    # Master Node Clustering 설정. 명령어 실행 후 설정한 구성 확인하고 동의(yes)
    $ redis-cli --cluster create 127.0.0.1:6379 127.0.0.1:6380 127.0.0.1:6381
    >>> Performing hash slots allocation on 3 nodes... Master[0] -> Slots 0 - 5460 Master[1] -> Slots 5461 - 10922 Master[2] -> Slots 10923 - 16383 M:        dc3803213aff6f279f6344559c8147198227aacc 127.0.0.1:6379 slots:[0-5460] (5461 slots) master

     .......
     
    # cluster 정보 확인
    $ redis-cli --cluster info 127.0.0.1:6379
    127.0.0.1:6379 (dc380321...) -> 4 keys | 5461 slots | 1 slaves. 
    127.0.0.1:6380 (b79c0eda...) -> 3 keys | 5462 slots | 1 slaves. 
    127.0.0.1:6381 (18b188cb...) -> 1 keys | 5461 slots | 1 slaves. 
    [OK] 8 keys in 3 masters. 
    0.00 keys per slot on average.
    
#### 2. master 및 slave 구조의 clustering    
    
    # Slave Node 생성
    $ cd ${REDIS_HOME}/utils
    $ install_server.sh       ## 6479.conf ~ 6481.conf까지 생성한다.
    
    # vi 6479.conf ~6481.conf
    appendonly yes 
    cluster-enabled yes 
    cluster-config-file nodes-6379.conf 
    cluster-node-timeout 15000

    # Redis 6479, 6480, 6481 재기동
    
    # Master node에 slave node를 추가 
    # redis-cli --cluster add-node {slave ip}:{port} {master ip}:{port} --cluster-slave
    $ redis-cli --cluster add-node 127.0.0.1:6479 127.0.0.1:6379 --cluster-slave
    $ redis-cli --cluster add-node 127.0.0.1:6480 127.0.0.1:6389 --cluster-slave
    $ redis-cli --cluster add-node 127.0.0.1:6481 128.0.0.1:6381 --cluster-slave 
    
    >>> Adding node 127.0.0.1:6481 to cluster 127.0.0.1:6381 
    >>> Performing Cluster Check (using node 127.0.0.1:6381) 
    M: 18b188cb18a61acaa61dc1b7e068602c1d41e308 127.0.0.1:6381 
       slots:[10923-16383] (5461 slots) master 
    M: dc3803213aff6f279f6344559c8147198227aacc 127.0.0.1:6379 
       slots:[0-5460] (5461 slots) master
       
    .....


    
## Tomcat Session Manager에서의 Redis 사용법

* 아래의 링크 참조
* https://github.com/redisson/redisson/tree/master/redisson-tomcat 
