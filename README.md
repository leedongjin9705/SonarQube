소나큐브(SonarQube)란

소나큐브는 소스 코드를 실행하지 않고도 잠재적인 결함, 보안 취약점, 중복 코드 등을 자동으로 찾아내는 SAST(Static Application Security Testing, 정적 분석) 플랫폼입니다.

사람이 놓치기 쉬운 아주 작은 실수부터 치명적인 보안 구멍까지 검사합니다.

목차

기본 사용법

서버 연동법

로그인법

Global Token 생성법

리포트 생성 및 확인법

JS 라이브러리 검사 추가하기

기본 사용법

플러그인 설치
파일 → 설정 → 플로그인 → SonarQube for IDE 설치 및 재시작
<img width="985" height="721" alt="1" src="https://github.com/user-attachments/assets/8b7b4a81-16ac-4a26-a0ad-27019bf46e63" />

아래처럼 아이콘이 생긴 걸 확인할 수 있습니다.
<img width="559" height="338" alt="2" src="https://github.com/user-attachments/assets/09a4c96f-e69e-4020-937f-6a9ea66d2883" />


사용법
아래 Analyze All Project Files 클릭하면 검사됩니다. 그렇지만 이렇게 보면 리포팅이 어렵습니다.
<img width="831" height="385" alt="3" src="https://github.com/user-attachments/assets/9a0fe7fa-baa3-44ac-8512-6ba78a31b12e" />


서버연동법


인텔리제이에서 확인이 가능하지만, 취약점이 많은 지금 상황에서는 확인이 어렵습니다.
두가지 방법이 있는데
1. 소나큐브 커뮤니티 사용(무료)
2. 웹에서 사용하는 두 가지 방법이 있는데

저희는 무료 기준으로 사용해보겠습니다.

1. 소나큐브 커뮤니티는 java 버전 21 이 필요. 기존 java 버전을 건들면 안되니 소나큐브에만 java 21 사용하겠습니다|(요구되는 자바 버전은 계속 높아질 수 있음)
2. https://adoptium.net/temurin/releases/?version=21   옆의 사이트에서 아래의 zip 파일 다운로드 후 C드라이브에 압축풀기(폴더명 및 경로 맞춰주세요 - 폴더명: jdk-21)
<img width="1024" height="252" alt="4" src="https://github.com/user-attachments/assets/97306613-4083-46b1-bdb0-5cef937f84d4" />


3. https://www.sonarsource.com/products/sonarqube/downloads/success-download-community-edition/ 옆의 사이트에서 
SONARQUBE COMMUNITY BUILD 다운로드
4. C 드라이브에서 압축을 풀어줍니다.(폴더명 및 경로 맞춰주세요 - 폴더명: sonarqube-26.3.0.120487)
5. C:\sonarqube-26.3.0.120487\bin\windows-x86-64 경로에서 git bash 열기
6. 아래 명령어 순서대로 실행

export SONAR_JAVA_PATH="C:/jdk-21/bin/java.exe"
./StartSonar.bat

6-1. export SONAR_JAVA_PATH="C:/jdk-21/bin/java.exe" 명령어는 일회성으로 계속 써줘야 함
귀찮다면 StartSonar.bat 파일을 수정. 
파일중 @echo off 를 찾아 그 아래에 1줄을 추가해주면 된다.

@echo off
set SONAR_JAVA_PATH=C:\jdk-21\bin\java.exe

로그인법

1.  http://localhost:9000 접속
2. 최초 로그인 창에서 
admin/admin 을 치고 로그인하면 아래처럼 비밀번호 수정이 가능 


<img width="457" height="427" alt="5" src="https://github.com/user-attachments/assets/457bc651-88d4-4502-ae56-66308b5ea940" />

global token 생성법



아래의 순서에 맞게 글로벌 토큰을 생성해줍니다.
우측에 아이콘 클릭 후 보안탭으로 이동 후 Type만 아래 사진처럼 수정해서 생성
<img width="1024" height="479" alt="6" src="https://github.com/user-attachments/assets/c8c6198a-a648-40b6-a7cc-0a290a3912dd" />


위에서 생선된 토큰을 아래에서 사용



Dependency-Check 설치하기
<img width="852" height="463" alt="7" src="https://github.com/user-attachments/assets/d78991dc-5a8b-4c0c-9da5-d4c6a63f5ced" />

아래 install 클릭 후 pending install 이 나오면 서버 재시작
<img width="1024" height="345" alt="8" src="https://github.com/user-attachments/assets/cf216cd5-9295-4e02-816e-4d16fe2fc644" />


오픈소스 취약점 데이터베이스(OSS Index)로 취약점 검사하기

https://ossindex.sonatype.org/ 

위의 사이트 진입 → 우측 상단 Sign In 버튼 클릭 → Register for an account

이후 회원가입 진행
<img width="1024" height="370" alt="9" src="https://github.com/user-attachments/assets/50936c29-f1d3-4f19-a226-a6335f4ea62e" />

이후 우측 상단 설정 클릭
<img width="1024" height="365" alt="10" src="https://github.com/user-attachments/assets/cc85dec2-71ad-4d97-ab08-648068539c68" />

API 토큰 복사
<img width="1024" height="266" alt="11" src="https://github.com/user-attachments/assets/2fefbab8-563a-4ddb-95f9-38cb0db7a8dd" />



인텔리제이 설정법

workspace의 pom.xml에 작업(최상위 폴더에만 설정해주면 됩니다.)

properties 설정
        <sonar.sources>src/main/java,webapp</sonar.sources>
        <sonar.javascript.lcov.reportPaths>${project.build.directory}/test-results/lcov.info</sonar.javascript.lcov.reportPaths>
        <sonar.dependencyCheck.jsonReportPath>${project.build.directory}/dependency-check-report.json</sonar.dependencyCheck.jsonReportPath>
        <sonar.dependencyCheck.htmlReportPath>${project.build.directory}/dependency-check-report.html</sonar.dependencyCheck.htmlReportPath>
        <sonar.scm.exclusions.disabled>true</sonar.scm.exclusions.disabled>
        <sonar.host.url>http://localhost:9000</sonar.host.url>
        <sonar.token>글로벌 토큰</sonar.token>
        <ossindex.analyzer.enabled>true</ossindex.analyzer.enabled>
        <ossIndexUsername>OOS 회원가입 아이디</ossIndexUsername>
        <ossIndexPassword>OOS token 값</ossIndexPassword>


build 설정
<plugin>
    <groupId>org.owasp</groupId>
    <artifactId>dependency-check-maven</artifactId>
    <version>12.2.0</version> <configuration>
        <format>ALL</format>
        <outputDirectory>${project.build.directory}</outputDirectory>
        <ossindexAnalyzerEnabled>true</ossindexAnalyzerEnabled>
        <ossIndexUsername>${ossIndexUsername}</ossIndexUsername>
        <ossIndexPassword>${ossIndexPassword}</ossIndexPassword>
        <autoUpdate>true</autoUpdate>
        <skipTestScope>true</skipTestScope>
        <scanSet>
            <fileSet>
                <directory>webapp</directory>
                <includes><include>**/*.js</include></includes>
            </fileSet>
        </scanSet>
    </configuration>
    </plugin>
            <plugin>
                <groupId>org.sonarsource.scanner.maven</groupId>
                <artifactId>sonar-maven-plugin</artifactId>
                <version>3.9.1.2184</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.14.0</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <parameters>true</parameters>
                </configuration>
            </plugin>

이후 메이븐 업데이트(인텔리제이 껏다 키는 게 빠름)



추가로 webapp이 없으면 검사가 어려움으로, 간단하게 webapp 폴더가 없는 프로젝트(ex.WS_FW_SS2)

는 프로젝트 경로 바로 아래에 webapp 폴더를 만들어주세요.



취약점 확인하기

인텔리제이로 와서 검사합니다.

아래 코드는 통합 검사로 SAST, JS 라이브러리 검사를 병행합니다.

export JAVA_HOME="/c/jdk-21" && export PATH="$JAVA_HOME/bin:$PATH" && java -version
mvn clean compile dependency-check:check sonar:sonar

<img width="1024" height="190" alt="12" src="https://github.com/user-attachments/assets/126261a5-1a0a-45cd-b250-4f6cbeecda7a" />


해당 방법은 세계 취약점 데이터베이스(NVD), 오픈소스 취약점 데이터베이스(OSS Index)를 참고 및 다운로드한 뒤 그걸 베이스로 검사를 진행하기에

최초에 2~3시간이 소요됩니다. 3%씩 표시되니 화면 변화가 없어도 기다려주세요.
<img width="347" height="354" alt="13" src="https://github.com/user-attachments/assets/d3f31305-9576-4a51-9f93-fc00de68fda7" />

이후 작업 종료시 아래처럼 리포트가 완성됩니다.
<img width="1843" height="837" alt="14" src="https://github.com/user-attachments/assets/52d777bf-2154-4284-9ab6-fd1d719cb2a1" />

js 라이브러리만 따로 확인법

C:\DevEnvNew\workspace\target 의 dependency-check-jenkins.html 파일 확인

<img width="1913" height="881" alt="15" src="https://github.com/user-attachments/assets/a27d5e23-f380-4d77-8730-39634b3c1f33" />



DB 연결하기

DB를 연결해두지 않으면 설정 이상이 생길 시 서버의 데이터가 사라질 위험이 있습니다.

DB를 연결하여 데이터를 저장하면 그런 걱정을 줄여줍니다



DB 만들기(기존 DB 사용시 생략)
1. pgAdmin 4 실행
2. 전용 DB 생성(모를시 GPT - 기존에 만들어둔 게 있어서 그런지 이름만 넣으니 생성이 되어 못썼습니다..)
3. 계정 생성


순서대로 실행

CREATE USER sonar WITH ENCRYPTED PASSWORD 'sonar1234';
CREATE DATABASE sonarqube OWNER sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;

4.  C:\sonarqube-26.3.0.120487\conf 폴더의 sonar.properties 수정
파일 연 후

= 뒤의 키워드로 검색 후 수정. #은 빼줘야함.

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar1234
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube?currentSchema=public

5. 서버 재시작
테이블을 자동으로 생성하느라 실행이 늦을수도 있으니, 기다려 주세요.





DB 연결하기

DB를 연결해두지 않으면 설정 이상이 생길 시 서버의 데이터가 사라질 위험이 있습니다.

DB를 연결하여 데이터를 저장하면 그런 걱정을 줄여줍니다



DB 만들기(기존 DB 사용시 생략)
1. pgAdmin 4 실행
2. 전용 DB 생성(모를시 GPT - 기존에 만들어둔 게 있어서 그런지 이름만 넣으니 생성이 되어 못썼습니다..)
3. 계정 생성
<img width="423" height="454" alt="16" src="https://github.com/user-attachments/assets/3a4569d3-0231-4155-a42b-8b0a23e6790e" />


순서대로 실행

CREATE USER sonar WITH ENCRYPTED PASSWORD 'sonar1234';
CREATE DATABASE sonarqube OWNER sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;

4.  C:\sonarqube-26.3.0.120487\conf 폴더의 sonar.properties 수정
파일 연 후

= 뒤의 키워드로 검색 후 수정. #은 빼줘야함.

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar1234
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube?currentSchema=public

5. 서버 재시작
테이블을 자동으로 생성하느라 실행이 늦을수도 있으니, 기다려 주세요.
