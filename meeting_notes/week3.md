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