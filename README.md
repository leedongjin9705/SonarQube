소나큐브(SonarQube)란

소나큐브는 소스 코드를 실행하지 않고도 잠재적인 결함, 보안 취약점, 중복 코드 등을 자동으로 찾아내는 SAST(Static Application Security Testing, 정적 분석) 플랫폼입니다.

사람이 놓치기 쉬운 아주 작은 실수부터 치명적인 보안 구멍까지 검사합니다.

목차

1. 기본 사용법
2. DB연결법
3. 서버연동법
4. 로그인법
5. Global Token 생성법
6. Dependency-Check 설치법
7. 오픈소스 취약점 데이터베이스(OSS Index)로 추가 취약점 검사법
8. 인텔리제이 설정법
9. 취약점 확인법


기본 사용법

플러그인 설치
파일 → 설정 → 플로그인 → SonarQube for IDE 설치 및 재시작
<img width="985" height="721" alt="1" src="https://github.com/user-attachments/assets/9da7d649-bd74-49b2-98bd-be19b843b16f" />


아래처럼 아이콘이 생긴 걸 확인할 수 있습니다.
<img width="559" height="338" alt="2" src="https://github.com/user-attachments/assets/924dde87-1431-4c3f-bf3a-22929ccf274c" />


사용법
아래 Analyze All Project Files 클릭하면 검사됩니다. 그렇지만 이렇게 보면 리포팅이 어렵습니다.
<img width="831" height="385" alt="3" src="https://github.com/user-attachments/assets/792d4e65-f4f9-445d-a45e-3a61155eae64" />



DB 연결법

DB를 연결해두지 않으면 설정 이상이 생길 시 서버의 데이터가 사라질 위험이 있습니다.
DB를 연결하여 데이터를 저장하면 그런 걱정을 줄여줍니다.
 * 경험담인데 DB 연결 귀찮다고 나중에 하면 기존 데이터는 사라집니다. 검사한 자료, 토큰, 계정 등



DB 만들기(기존 DB 사용시 생략)
1. pgAdmin 4 실행
2. 전용 DB 생성(모를시 GPT - 기존에 만들어둔 게 있어서 그런지 이름만 넣으니 생성이 되어 못썼습니다..)
3. 계정 생성
4. 사용자 생성 및 권한 설정
<img width="423" height="454" alt="4" src="https://github.com/user-attachments/assets/bab6c5ec-6612-4d2f-b0a1-890cbad43d8a" />

순서대로 실행

CREATE USER sonar WITH ENCRYPTED PASSWORD 'sonar1234';
CREATE DATABASE sonarqube OWNER sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;

C:\sonarqube-26.3.0.120487\conf 폴더의 sonar.properties 수정


파일 연 후

'=' 앞의 키워드로 검색 후 수정. #은 빼줘야함.

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar1234
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube?currentSchema=public

서버 재시작

테이블을 자동으로 생성하느라 실행이 늦을수도 있으니, 기다려 주세요.




서버연동법


인텔리제이에서 확인이 가능하지만, 취약점이 많은 지금 상황에서는 확인이 어렵습니다.
두가지 방법이 있는데
1. 소나큐브 커뮤니티 사용(무료)
2. 웹에서 사용하는 두 가지 방법이 있는데

저희는 무료 기준으로 사용해보겠습니다.

1. 소나큐브 커뮤니티는 java 버전 21 이 필요. 기존 java 버전을 건들면 안되니 소나큐브에만 java 21 사용하겠습니다|(요구되는 자바 버전은 계속 높아질 수 있음)
2. https://adoptium.net/temurin/releases/?version=21   옆의 사이트에서 아래의 zip 파일 다운로드 후 C드라이브에 압축풀기(폴더명 및 경로 맞춰주세요 - 폴더명: jdk-21)
<img width="1024" height="252" alt="5" src="https://github.com/user-attachments/assets/ad2ec020-3c86-49d1-8d5e-674f7ba1c1d9" />



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
<img width="457" height="427" alt="6" src="https://github.com/user-attachments/assets/b3453a81-9421-4a9f-ba73-d63b680d33c9" />


global token 생성법



아래의 순서에 맞게 글로벌 토큰을 생성해줍니다.
우측에 아이콘 클릭 후 보안탭으로 이동 후 Type만 아래 사진처럼 수정해서 생성
<img width="1024" height="479" alt="7" src="https://github.com/user-attachments/assets/3191d8a1-9f65-441c-82c5-dc8eb185c307" />



위에서 생선된 토큰을 아래에서 사용



Dependency-Check 설치법
<img width="852" height="463" alt="8" src="https://github.com/user-attachments/assets/a012055d-e741-48fc-9915-47cc4a1a9eda" />

아래 install 클릭 후 pending install 이 나오면 서버 재시작
<img width="1024" height="345" alt="9" src="https://github.com/user-attachments/assets/1a83d15a-0784-401f-bde5-9fac59f251e9" />



오픈소스 취약점 데이터베이스(OSS Index)로 추가 취약점 검사법

https://ossindex.sonatype.org/ 

위의 사이트 진입 → 우측 상단 Sign In 버튼 클릭 → Register for an account

이후 회원가입 진행
<img width="1024" height="370" alt="10" src="https://github.com/user-attachments/assets/0f0e92fb-65c2-45e6-b1c4-d1516639b1fb" />

이후 우측 상단 설정 클릭
<img width="1024" height="365" alt="11" src="https://github.com/user-attachments/assets/1c40b035-c70c-450f-9e8a-1c500f9e8d96" />

API 토큰 복사
<img width="1024" height="266" alt="12" src="https://github.com/user-attachments/assets/cc781fc7-5816-431b-82e9-ecf90f2f2f74" />


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
        <ossIndexUsername>OSS 회원가입 아이디</ossIndexUsername>
        <ossIndexPassword>OSS token 값</ossIndexPassword>


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



취약점 확인법

인텔리제이로 와서 검사합니다.

아래 코드는 통합 검사로 SAST, JS 라이브러리 검사를 병행합니다.

export JAVA_HOME="/c/jdk-21" && export PATH="$JAVA_HOME/bin:$PATH" && java -version
mvn clean compile dependency-check:check sonar:sonar
<img width="1024" height="190" alt="13" src="https://github.com/user-attachments/assets/85041b98-4e65-4056-80f3-22aef3b1bba2" />


해당 방법은 세계 취약점 데이터베이스(NVD), 오픈소스 취약점 데이터베이스(OSS Index)를 참고 및 다운로드한 뒤 그걸 베이스로 검사를 진행하기에

최초에 2~3시간이 소요됩니다. 3%씩 표시되니 화면 변화가 없어도 기다려주세요.
<img width="347" height="354" alt="14" src="https://github.com/user-attachments/assets/db954513-28ae-4e70-9524-6bce1307ccb5" />

이후 작업 종료시 아래처럼 리포트가 완성됩니다.
<img width="1843" height="837" alt="15" src="https://github.com/user-attachments/assets/5a23aef8-cbd0-40a7-951c-a4064b0bdef8" />

js 라이브러리만 따로 확인법

1. C:\DevEnvNew\workspace\target 의 dependency-check-jenkins.html 파일 확인
<img width="1913" height="881" alt="16" src="https://github.com/user-attachments/assets/e11bdc35-1c0b-499d-8b6b-35986d29afc3" />

