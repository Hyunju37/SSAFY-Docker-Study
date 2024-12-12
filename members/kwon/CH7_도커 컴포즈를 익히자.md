# SECTION 1. 도커 컴포즈란?

## 도커 컴포즈

시스템 구축과 관련된 명령어를 하나의 텍스트 파일(정의 파일)에 기재해
명령어 한번에 시스템 전체를 실행하고 종료와 폐기까지 한번에 하도록 도와주는 도구이다.

![](./images/docker-compose1.png)

### 구조

docker compose는 시스템 구축에 필요한 설정을 YAML(YAML Ain't Markup Language) 포멧으로 기재한 정의 파일을 이용해 전체 시스템을 일괄 실행(`run`) 또는 일괄 종료 및 삭제(`down`)할 수 있는 도구다.

정의 파일에는 컨테이너나 볼륨을 어떤 설정으로 만들지에 대한 항목이 기재되어 있다.
작성 내용은 도커 명령어롸 비슷하지만 도커 명령어가 아니다.

#### `up`
`docker run`과 비슷하다, 정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행한다.
정의 파일에는 네트워크나 볼륨에 대한 정의도 기재할 수 있어서 주변 환경을 한꺼번에 생성할 수 있다.

#### `down`
컨테이너와 네트워크를 정지 및 삭제한다.
**볼륨과 이미지는 삭제하지 않는다**. 컨테이너와 네트워크 삭제 없이 종료만 하고 싶다면 stop 커맨드를 이용한다.

docker compose가 Dockerfile 스크립트와 비슷하게 느껴질 수 있다.
하지만 Dockerfile은 이미지를 만들기 위한 것으로 **네트워크나 볼륨은 만들 수 없다.**
반면, docker compose는 docker run 명령어를 여러 개 모아놓은 것과 같은 것으로,
**네트워크와 볼륨까지 함께 만들 수 있다.**

---

# SECTION 2. 도커 컴포즈의 설치와 사용법

## docker compose의 설치
도커 컴포즈는 토커 엔진과 별개의 SW이므로 설치를 따로 해줘야 한다.
윈도우나 mac OS에서 사용하는 docker desktop은 docker compose가 함께 설치되므로 신경 쓸 필요 없다.
리눅스의 경우 아래의 명령어에 따라 설치를 진행한다.

```sh
sudo apt install -y python3 python3-pip
sudo pip3 install docker-compose
```

## docker compose의 사용법

docker compose를 사용하려면 Dockerfile 스크립트로 이미지를 빌드할 때처럼 호스크 컴퓨터에 폴더를 만들고 이 폴더에 정의 파일(YAML)을 배치한다.

정의 파일은 docker-compose.yml이라는 이름으로 만들어야 한다.
파일은 호스트 컴퓨터에 배치되지만 명령어는 똑같이 도커 엔진에 전달되며, 만들어진 컨테이너도 동일하게 도커 엔진 위에서 동작한다.
즉, 사람이 입력해던 명령어들을 docker compose가 대신 입력해주는 구조다.

정의 파일은 한 폴더에 하나만 있을 수 있다.
그래서 여러 개의 정의 파일을 사용하려면 그 개수만큼 폴더를 만들어야 한다.
컨테이너 생성에 필요한 이미지 파일과 같은 부가적인 파일도 compose가 사용할 폴더에 함께 둔다.

---

# SECTION 3. 도커 컴포즈 파일을 작성하는 법

## docker compose의 내용

```yml
version: "3"

sevices:
    apa000ex2:
        image: httpd
        ports:
            - 8080:80
        restart: always
```

이 docker compose 파일은 아래 명령어와 같은 내용을 담고 있다.

```sh
docker run --name apa000ex2 -d -p 8080:80 httpd
```

전체적인 구조는 다음과 같다.

```yml
version: "3"

sevices:
    container_name1:
        image: image_name
        networks:
            - network_name
        ports:
            - port_setting
    container_name2:

networks:
    network_name1:
volumes:
    volume_name1:
    volume_name2:
```

### compose 파일 작성 요령 정리

- 첫 중에 docker compose version을 기재
- 주 항목 services, networks, volumes 아래에 설정 내용을 기재
- 항목 간의 상하 관계는 공백을 사용한 들여쓰기로
- 들여쓰지는 같은 수의 배수만큼의 공백을 사용
- 이름은 주 항목 아래에 들여쓰기 한 다음 기재
- 여러 항목을 기재하려면 출 앞에 '-'을 붙임
- 이름 뒤에는 ':'을 붙임
- 콜론 뒤에는 반드시 공백이 와야 함
- \# 뒤의 내용은 주석으로 간주
- 문자열은 작은/큰따옴표로 감싸 작성

### compose 파일의 항목

#### 주 항목

| 항목     | 내용          |
| :------- | :------------ |
| services | 컨테이너 정의 |
| networks | 네트워크 정의 |
| volumes  | 볼륨 정의     |

#### 자주 나오는 정의 내용

| 항목        | `docker run`에서의 옵션 / 인자 | 내용                                |
| :---------- | :----------------------------- | :---------------------------------- |
| image       | 이미지 인자                    | 사용할 이미지 지정                  |
| networks    | `--net`                        | 접속할 네트워크 설정                |
| volumes     | `-v`, `--mount`                | 스토리지 마운트 성정                |
| environment | `-e`                           | 환경변수 설정                       |
| depends_on  | -                              | 다른 서비스에 대한 의존관계를 정의  |
| restart     | -                              | 컨테이너 종료 시 재시작 여부를 설정 |

#### restart 설정값

| 설정값         | 내용                                       |
| :------------- | :----------------------------------------- |
| no             | 재시작 안함                                |
| always         | 항상 재시작                                |
| on-failure     | 프로세스가 0 이외의 상태로 종료되면 재시작 |
| unless-stopped | 종료 시 재시작하지 않음. 이외에는 재시작   |

## 실습

### docker-compose.yml
```yml
version: "3.8"
services:
  postgres:
    build:
      context: .
      dockerfile: Dockerfile.postgres  # postgres용 Dockerfile
    container_name: backend
    environment:
      POSTGRES_DB: rag
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    working_dir: /home
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - .:/home/app
    ports:
      - "5430:5432"
    stdin_open: true
    tty: true
    command: tail -f /dev/null
    # restart: unless-stopped

  vue:
    build:
      context: .
      dockerfile: Dockerfile.vue  # vue용 Dockerfile
    container_name: frontend
    network_mode: host
    working_dir: /app
    volumes:
      - .:/app  # 로컬 디렉토리를 컨테이너에 마운트
    ports:
      - "5173:5173"
    stdin_open: true
    tty: true

volumes:
  postgres_data:
```

### Dockerfile.postgres
```Dockerfile
# 베이스 이미지 선택
FROM kwon0528/postgres_backend:0.5

# 필요한 패키지 설치 (bash 설치)
RUN apt-get update && apt-get install -y bash

# 작업 디렉토리 설정
WORKDIR /home

# back.sh 파일 복사
COPY back.sh /home/back.sh

# 실행 권한 부여
RUN chmod +x /home/back.sh

# 실행
CMD ["bash", "/home/back.sh"]
```


### Dockerfile.vue
```Dockerfile
# 베이스 이미지 선택
FROM kwon0528/vue_frontend:0.3.2

# 작업 디렉토리 설정
WORKDIR /app

# # front.sh 파일 복사
# COPY front.sh /app/chuibot/front.sh

# # 실행 권한 부여
# RUN chmod +x /app/chuibot/front.sh

# # 실행
# CMD ["sh", "/app/front.sh"]
# CMD cd ~/chuibot && git pull
```

실제 postgresql과 vue를 사용한 프로젝트에서 환경 설정을 위해 구성했던 docker compose이다.
위에서 알아본 것과 다른 점은 image를 바로 명시하지 않고 Dockerfile을 기반으로 build를 구성했다는 것이다.

이처럼 docker compose 안에서도 Dockerfile을 사용하여 구성할수도 있다.

---

# 도커 컴포즈 실행

## docker compose command

### `docker-compose up`

```sh
docker-compose -f compose_file_path up
```

compose 파일의 내용에 따라 컨테이너와 볼륨, 네트워크를 생성하고 실행한다.

#### 옵션 항목

| 옵션                        | 내용                                                 |
| --------------------------- | ---------------------------------------------------- |
| `-d`                        | 백그라운드로 실행                                    |
| `--no-color`                | 화면 출력 내용을 흑백으로 함                         |
| `--no-deps`                 | 링크된 서비스를 싱행하지 않음                        |
| `--force-recreate`          | 설정 또는 이미지가 변경되지 않아도 컨테이너 재생성   |
| `--no-create`               | 컨테이너가 이미 존재할 경우 다시 생성하지 않음       |
| `--no-build`                | 이미지가 없어도 이미지를 빌드하지 않음               |
| `--build`                   | 컨테이너를 실행하기 전에 이미지를 빌드               |
| `--abort-on-container-exit` | 컨테이너가 하나라도 종료되면 모든 컨테이너를 종료    |
| `-t`, `--timeout`           | 컨테이너를 졸요할 때의 타임아웃 설정. 기본은 10초    |
| `--remove-orphans`          | 컴포즈 파일에 섲의되지 않은 서비스의 컨테이너를 삭제 |
| `--scale`                   | 컨테이너 수 변경                                     |

### `docker-compose down`

```sh
docker-compose -f compose_file_path down
```

compose 파일의 내용에 따라 컨테이너와 네트워크를 종료 및 삭제한다. 볼륨과 이미지는 삭제되지 않는다.

#### 옵션 항목

| 옵션               | 내용                                                                                                                                      |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `-rmi` 종류        | 삭제 시에 이미지도 삭제한다. 종류를 all로 지정하면 사용했던 모든 이미지가 삭제된다. local로 지정하면 커스텀 태그가 없는 이미지만 삭제된다 |
| `-v`, `--volume`   | volumes 항목에 기재된 볼륨을 삭제한다. 단, external로 지정된 볼륨은 삭제되지 않는다.                                                      |
| `--remove-orphans` | 컴포즈 파일에 정의되지 않은 서비스의 컨테이너도 삭제한다.                                                                                 |

### `docker-compose stop`

```sh
docker-compose -f compose_file_path stop <option>
```

compose 파일의 내용에 따라 컨테이너와 네트워크를 종료한다.

---

앞서 실습으로 작성했던 compose 파일을 `up`으로 실행하면 postgresql 환경에 django를 쓸 수 있도록 backend container가 실행되고,
vue 환경으로 frontend container가 실행된다.