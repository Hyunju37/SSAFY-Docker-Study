# 2024.11.28(목)

## 논의 사항

### CH5. 컨테이너를 연동해보자

- 제서윤
  - TroubleShooting
    - 계속 mysql 컨테이너가 Exited 상태임
    - DB 로그를 확인할 수 있는 명령어로 로그를 출력해 문제 원인 분석
    - `docker logs <mysql_컨테이너_이름>`

      ![image.png](../members/seoyun/images/CH5_3.png)
    
      - 문제 원인
          1. 오타: `collaction-server`를 `collation-server`로 수정
        
          ![image.png](../members/seoyun/images/CH5_4.png)
        
          2. 버전 문제: MySQL 9.x는 아직 일부 설정이나 플러그인을 완전히 지원하지 않을 수 있다고 함. 
             8.1버전으로 다시 실행
        
          ![image.png](../members/seoyun/images/CH5_5.png)
        
          - 해결 완료

### CH6. 실전에 활용 가능한 컨테이너 사용법을 익히자

- 연재환 
    - Issues
      - 바인드 마운트 실습 과정에서 연결한 로컬 디렉터리가 컨테이너 내부에서 잡히지 않음 
        - 로컬 스토리지 (Users/jaehwan/Documents/apa_folder) 내부의 index.html에는 '안녕하세요'가 작성되어 있으나, 아파치 컨테이너를 웹 브라우저로 접속 (localhost:8090) 해보면 It Works! 메시지가 출력 됨.
        
        <img src='../members/jaefan/images/ch06-32.png' width=50%>

        - 컨테이너 내부 (/usr/local/apache2/htdocs) 에도 index.html 존재 확인 
        
        <img src='../members/jaefan/images/ch06-33.png' width=50%>

        - 컨테이너 내부의 index.html 파일 삭제 `rm index.html` 
        - 그러나, 마운트한 로컬 스토리지 내부의 index.html 이 잡히지 않음.
        - 해당 컨테이너와 로컬 스토리지 간의 바인드 마운트가 제대로 되지 않음을 인식.
        - 최후의 수단으로 컨테이너 중지 -> 컨테이너 삭제 -> 컨테이너 재실행 절차로 인식 성공 

        <img src='../members/jaefan/images/ch06-14.png' width=50%>

      - 문제는 해결 했으나, 원인 파악이 되지 않음. 
  