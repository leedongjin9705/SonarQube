<img width="1395" height="675" alt="12" src="https://github.com/user-attachments/assets/e2acf0aa-2cde-4427-8cbd-8b2b3b03c8a8" /><img width="1381" height="703" alt="11" src="https://github.com/user-attachments/assets/7cc9bd75-34d9-4365-9eef-26e7f8833394" />
<img width="1381" height="703" alt="11" src="https://github.com/user-attachments/assets/fe7314e1-8eca-4b1b-b5b2-2613f9c0e85e" />
도커를 사용한 소나큐브 사용법

무료로 사용하기 위해 도커로 SonarQube Community Build를 설치 후 사용 할 예정.

과거 메뉴얼보다 훨씬 편함



cmd 창에 아래 명령어 입력

docker run -d --name sonarqube -p 9000:9000 sonarqube:community


이후 localhost:9000에 들어가서

id, pass : admin

이후 비밀번호 수정

My Account 클릭
<img width="1381" height="703" alt="11" src="https://github.com/user-attachments/assets/b3fe3d1a-e19d-433f-abdd-dd2f86b68be8" />


global 토큰 생성
<img width="1395" height="675" alt="12" src="https://github.com/user-attachments/assets/51ec931b-e1b2-475b-a720-a079fa593b2e" />


projects → create a local project 클릭
<img width="1361" height="643" alt="13" src="https://github.com/user-attachments/assets/3e8d1a54-dfd1-4509-8f9a-596637b64776" />


프로젝트 명에 따라 생성
<img width="463" height="485" alt="14" src="https://github.com/user-attachments/assets/2094445c-ddbd-4fc4-a8ed-1cfde50cd474" />


이후 cmd에서 프로젝트 경로로 이동


아래 실행.

docker run --rm -e SONAR_HOST_URL=http://host.docker.internal:9000 -e SONAR_TOKEN=여기에토큰 -v c:/DevEnvNew/workspace/TV_OD_WEB:/usr/src sonarsource/sonar-scanner-cli -Dsonar.projectKey=TV_OD_WEB -Dsonar.sources=src,webapp -Dsonar.exclusions=**/target/**,**/.mvn/**,**/node_modules/**,**/deploy/**,webapp/hyper_saas/**,**/*.min.js,**/*.min.css -Dsonar.sourceEncoding=UTF-8 -Dsonar.java.source=17
