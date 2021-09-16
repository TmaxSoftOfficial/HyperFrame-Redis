HyperFrameOE-Tomcat 9.0.52

업로드된 바이너리는 HyperFrame Open Edition Tomcat 제품 설치를 위한 파일이다.

========================================================

사전 작업

- Java 버전 8 이상 권장

- apache-tomcat-9.0.52.tar.gz 파일 필요

========================================================

설치 방법

1. 다음 tar 파일의 압축 해제
> tar -zxvf apache-tomcat-9.0.52.tar.gz

2. 파일 기동
${TOMCAT_HOME}/bin > ./startup.sh 

3. http:// ip:8080 주소로 접속하여 WEB UI 화면을 확인

4. 종료
${TOMCAT_HOME}/bin > ./shutdown.sh 명령어로 종료

=======================================================

실행 / 종료 명령어

#### 실행 ####

${TOMCAT_HOME}/bin > ./startup.sh

#### 종료 ####

${TOMCAT_HOME}/bin > ./shutdown.sh

=======================================================

디렉토리 구조

#### 루트 디렉토리 구조 ####

 tomcat
    ├── bin		<----------------------- Tomcat 실행에 필요한 실행파일( 바이너리 )
    ├── common		<----------------------- 웹 애플리케이션에서 공통적으로 사용하는 클래스파일
    ├── conf		<----------------------- 서버 전체 설정파일 폴더( server.xml 등 )
    ├── logs   		<----------------------- 예외 발생 사항 등의 로그 저장
    ├── server		<----------------------- 서버에서 사용하는 클래스 라이브러리
    ├── temp		<----------------------- 임시 저장용 폴더
    ├── webapps   	<----------------------- 웹 애플리케이션 루트 폴더
    └── work		<----------------------- jsp 파일을 서블릿 형태로 변환한 java 파일과 class파일 저장

#### bin 디렉토리 구조 ####

tomcat
    ├── bin
      	 ├── bootstrap.jar 	<----------------------- tomcat 서버가 구동될 때 사용되는 main( ) 메소드 포함
	 ├── tomcat-juil.jar 	
	 ├── common-daemon.jar 	
	 ├── catalina.sh 		<----------------------- CATALINA 서버의 제어 스크립트
	 ├── ciphers.sh 		
	 ├── configtest.sh 	<----------------------- CATALINA 서버의 설정 스크립트
      	 ├── daemon.sh		
	 ├── digest.sh 		
	 ├── makebase.sh 	<----------------------- tomcat 실행에 필요한 분산 디렉토리 구조를 생성하는 스크립트
	 ├── setclasspath.sh 	<----------------------- JAVA_HOME 또는 JRE_HOME이 세팅되지 않았을 경우 세팅
	 ├── shutdown.sh 	<----------------------- CATALINA 서버를 중지하는 스크립트
	 ├── startup.sh		<----------------------- CATALINA 서버를 시작하는 스크립트
	 ├── tool-wrapper.sh
             └── version.sh		<----------------------- Tomcat의 정보를 표시해주는 스크립트	

#### conf 디렉토리 구조 ####

tomcat
    ├── conf
      	 ├── catalina.policy 	
	 ├── catalina.properties 	
	 ├── context.xml 	<----------------------- 세션, 쿠키 저장 경로등을 지정하는 설정 파일
	 ├── jaspic-providers.xml 
	 ├── logging.properties 		
	 ├── server.xml 		<----------------------- Tomcat 설정에서 가장 중요, Service, Connertor 등과 같은 주요 기능 설정 가능
	 ├── tomcat-users.xml	<----------------------- Tomcat 의 manager 기능을 사용하기 위해 사용자 권한을 설정	
             └── web.xml		<----------------------- Tomcat의 환경설정 파일	


========================================================

로그

${TOMCAT_HOME}/logs/catalina.out : 서버상에서 발생한 모든 내용을 기록한 파일
${TOMCAT_HOME}/logs/catalina.yyyy-mm-dd.log : 톰캣에서 생기는 로그만을 기록
${TOMCAT_HOME}/logs/manager.log : Tomcat Manager Web App 로그

#### 실시간 로그 확인 ####

${TOMCAT_HOME}/logs > tail -f catalina.out

#### 로그 경로 변경 ####

${TOMCAT_HOME}/conf/logging.properties 에서

1catalina.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
2localhost.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
3manager.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
4host-manager.org.apache.juli.AsyncFileHandler.directory = ${catalina.base}/logs
의 ${catalina.base}/logs 부분을 수정한 뒤,

${TOMCAT_HOME}/bin/catalina.sh 에서
/if [ -z "$CATALINA_OUT" ] ; then
  CATALINA_OUT="$CATALINA_BASE"/logs/catalina.out" 의
$CATALINA_BASE"/logs/catalina.out부분 수정

이후, Tomcat 재기동

=======================================================

환경설정파일

${TOMCAT_HOME}/conf/server.xml
		

#### port 번호 변경 #### 

<Connector port="8080" protocol="HTTP/1.1"
     70                connectionTimeout="20000"
     71                redirectPort="8443" />
         
* Connector port="8080" 으로 되어있는 포트 번호를 변경 

========================================================

-버전 확인
Tomcat : ${TOMCAT-HOME}/lib > $ java -cp catalina.jar org.apache.catalina.util.ServerInfo
Java : $ java -version

========================================================




라이센스

버전 이력
apache-tomcat-9.0.52




