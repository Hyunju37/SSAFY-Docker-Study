# SECTION 1. 워드프레스 구축
---
## 워드프레스 구축

워드프레스는 웹 사이트를 만들기 위한 소프트웨어로, 아파치나 데이터베이스, PHP 런타임 등을 필요로 하기 때문에 구축을 위한 연습 소재로 좋다.

워드프레스는 워드프레스 컨테이너와 MySQL 컨테이너로 구성된다.
워드프레스는 블로그 생성 도구와 같은 것이므로, 프로그램이 MySQL에 저장된 데이터를 읽고 쓸 수 있어야 한다.
때문에 두 컨테이너가 연결돼 있어야 한다.

두 컨테이너를 연결하기 위해서는 가상 네트워크를 만들어 두 개의 컨테이너를 소속시켜 연결한다.
네트워크에 대한 기본 명령어는 다음과 같다.
```sh
# 생성
docker network create <network_name>
# 삭제
docker network rm
# 조회
docker network ls
# 접속
docker network connect
# 접속 끊기
docker network disconnect
# 상세 정보
docker network isnpect
# 아무도 접속하지 않은 네트워크 삭제
docker network prune
```

### MySQL 실행 시 필요한 옵션과 인자

```sh
docker run --name <container_name> -dit --net=<network_name> -e MYSQL_ROOT_PASSWORD=<root_password> MYSQL_DATABASE=<database_name> -e MYSQL_USER=<user_name> -e MYSQL_PASSWORD=<password> mysql --charactoer-set-server=<문자인코딩> --collation-server=<정렬순서> --default-authentication-plugin=<인증방식>
```

뭔가 엄청 많아 보이지만 모두 거의 환경변수 설정이다.
#### 옵션

| 항목                     | 옵션                     |
| :----------------------- | :----------------------- |
| 네트워크 이름            | `--net`                  |
| 컨테이너 이름            | `--name`                 |
| MySQL 루트 패스워드      | `-e MYSQL_ROOT_PASSWORD` |
| MySQL 데이터 베이스 이름 | `-e MYSQL_DATABASE`      |
| MySQL 사용자 이름        | `-e MYSQL_USER`          |
| MySQL 패스워드           | `-e MYSQL_PASSWORD`      |

#### 인자

|    항목     |                값                 |
| :--------- | :------------------------------- |
| 문자 인코딩 |     `--charactoer-set-server`     |
|  정렬 순서  |       `--collation-server`        |
|  인증 방식  | `--default-authentication-plugin` |


### 워드프레스 컨테이너 실행 시 필요한 옵션과 인자

```sh
docker run --name <container_name> -dit --net=<network_name> -p <port> -e ... wordpress
```

#### 옵션

여기도 사용할 수 있는 옵션이 많다.

| 항목                        | 값                          |
| :-------------------------- | :-------------------------- |
| 데이터 베이스 컨테이너 이름 | `-e WORDPRESS_DB_HOST`      |
| 데이터 베이스 이름          | `-e WORDPRESS_DB_NAME`      |
| 데이터 베이스 사용자 이름   | `-e WORDPRESS_DB_USER`      |
| 데이터 베이스 비밀번호      | `-e WORDPRESS_DB_PASSSWORD` |

# SECTION 2. 워드프레스 및 MySQL 컨테이너 생성과 연동

```sh
C:\Users\Kwon>docker run --name mysql000ex11 -dit --net=wp -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=wordpress---db -e MYSQL_USER=wordpress000user -e MYSQL_PASSWORD=1234 mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
853dd040ade1fef5532d8429504d7a1b2ab3f0b3cb6f8ed635918d4df773b1b6

C:\Users\Kwon>docker run --name wordpress000ex12 -dit --net=wp -p 8085:80 -e WORDPRESSS_DB_HOST=mysql000ex11 -e WORDPRES
S_DB_NAME=wordpress000db -e WORDPRESS_DB_USER=wordpress000user -e WORDPRESS_DB_PASSWORD=1234 wordpress
aef41f135fbdba00fe389a0f882e52aaa05f24f638fbf7ce3277dfc964ea5e79

C:\Users\Kwon>docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                  NAMES
aef41f135fbd   wordpress   "docker-entrypoint.s…"   24 seconds ago   Up 21 seconds   0.0.0.0:8085->80/tcp   wordpress000ex12
```

책과 똑같이 작성했는데도 mysql이 실행이 정상적으로 되지 않아 log를 확인해봤다.

```sh
C:\Users\Kwon>docker logs mysql000ex11
2024-12-09 06:59:03+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 9.1.0-1.el9 started.
2024-12-09 06:59:03+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2024-12-09 06:59:03+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 9.1.0-1.el9 started.
2024-12-09 06:59:03+00:00 [Note] [Entrypoint]: Initializing database files
2024-12-09T06:59:03.712828Z 0 [System] [MY-015017] [Server] MySQL Server Initialization - start.
2024-12-09T06:59:03.715059Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 9.1.0) initializing of server in progress as process 80
2024-12-09T06:59:03.733272Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2024-12-09T06:59:04.187540Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2024-12-09T06:59:05.435282Z 0 [ERROR] [MY-000067] [Server] unknown variable 'default-authentication-plugin=mysql_native_password'.
2024-12-09T06:59:05.435689Z 0 [ERROR] [MY-013236] [Server] The designated data directory /var/lib/mysql/ is unusable. You can remove all files that the server added to it.
2024-12-09T06:59:05.435725Z 0 [ERROR] [MY-010119] [Server] Aborting
2024-12-09T06:59:06.816914Z 0 [System] [MY-015018] [Server] MySQL Server Initialization - end.
```

`default-authentication-plugin`이 없단다. 여기저기 찾아봤지만 아직 답을 찾지 못해 일단 제거하고 진행한다.

```sh
C:\Users\Kwon>docker run --name mysql000ex11 -dit --net=wp -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=wordpress---db
-e MYSQL_USER=wordpress000user -e MYSQL_PASSWORD=1234 mysql:8 --character-set-server=utf8mb4 --collation-server=utf8mb4_
unicode_ci --default-authentication-plugin=mysql_native_password
686dada7cdad34fca5dbf41bd22e4f870a449293a9bccdcaf56f883f22ce2775

C:\Users\Kwon>docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                 NAMES
686dada7cdad   mysql:8   "docker-entrypoint.s…"   8 seconds ago   Up 5 seconds   3306/tcp, 33060/tcp   mysql000ex11
```
책과 버전을 맞춰주니 된다.

```sh
C:\Users\Kwon>docker run --name mysql000ex11 -dit --net=wp -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=wordpress---db -e MYSQL_USER=wordpress000user -e MYSQL_PASSWORD=1234 mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
bd36ad18cacf2d5cb868c6007b3b73272477a5dc39e9b52e374fdd84cb1cd51b

C:\Users\Kwon>docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                  NAMES
bd36ad18cacf   mysql       "docker-entrypoint.s…"   11 seconds ago   Up 11 seconds   3306/tcp, 33060/tcp    mysql000ex11
aef41f135fbd   wordpress   "docker-entrypoint.s…"   41 minutes ago   Up 41 minutes   0.0.0.0:8085->80/tcp   wordpress000ex12
```

정상적으로 실행되는 것을 확인할 수 있다.