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
    $ ./redis-cli
    
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
