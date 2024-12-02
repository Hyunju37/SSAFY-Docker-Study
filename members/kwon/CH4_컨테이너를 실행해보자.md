# SECTION 1. 도커 엔진 시작하기/종료하기
---
## 리눅스에서 시작하기
```bash
# 도커 엔진 시작
sudo systemtl start docker
# 도커 엔진 종료
sudo systemtl stop docker
# 자동 실행 설정
sudo systemtl enable docker
```

---

# SECTION 2. 컨테이너의 기본적인 사용방법
> 컨테이너를 다루는 모든 명령은 `docker` 명령어로 시작한다
```bash
docker ~
```

## 명령어와 대상

`docker` 명령어 뒤에 '무엇을', '어떻게', '대상' 순으로 지정하여 명령어를 작성한다.

<p align = "center"><img src = "./images/docker-command1.png" width = "50%"></p>

'무엇을', 어떻게 부분을 커맨드라고 하고, 상위 커맨드가 '무엇을', 하위 커맨드가 '어떻게'에 해당하는 내용을 지정한다.

상위 커맨드에 들어가는 **대상의 종류**는 12종류이다. 개인적으로는 프로그래밍 언어에서의 type과 같은 것이라고 이해했다.

```bash
docker image pull penguin
docker container start penguin
...
```

## 옵션과 인자

필요에 따라 '옵션'이나 '인자'를 붙여 사용하는 경우도 있다

<p align = "center"><img src = "./images/docker-command2.png" width = "50%"></p>

위 명령어는 penguin container를 백그라운드로(`-d`), mode 1로(`--mode=1`) 실행하라는 의미이다.

모든 명령어에 옵션이나 인자가 붙는 것은 아니며, 여러개 붙을 수 있는 명령어라도 자주 쓰이는 것은 한정되어 있으므로 기억해놓는 것이 좋다.

## 기본적인 명령어

### 컨테이너 조작 관련 커맨드(`container`)

| 하위 커맨드 | 내용                                                                                                                      | 생략 가능 여부 |               주요 옵션                |
| :---------: | :------------------------------------------------------------------------------------------------------------------------ | :------------: | :------------------------------------: |
|   `start`   | 컨테이너 실행                                                                                                             |       O        |                  `-i`                  |
|   `stop`    | 컨테이너 정지                                                                                                             |       O        |           거의 사용하지 않음           |
|  `create`   | 도커 이미지로부터 컨테이너를 생성                                                                                         |       O        |        `--name` `-e` `-p` `-v`         |
|    `run`    | 도커 이미지를 내려받고 컨테이너를 생성해 실행(`docker image pull` + `docker container create` + `docker container start`) |       O        | `--name` `-e` `-p` `-v` `-d` `-i` `-t` |
|    `rm`     | 정지 상태의 컨테이너 삭제                                                                                                 |       O        |               `-f` `-v`                |
|   `exec`    | 실행 중인 컨테이너 속에서 프로그램을 실행                                                                                 |       O        |               `-i` `-t`                |
|    `ls`     | 컨테이너 목록 출력                                                                                                        |  `docker ps`   |                  `-a`                  |
|    `cp`     | 도커 컨테이너와 도커 호스트 간에 파일 복사                                                                                |       O        |           거의 사용하지 않음           |
|  `commit`   | 도커 컨테이너를 이미지로 변환                                                                                             |       O        |           거의 사용하지 않음           |


### 이미지 조작 관련 커맨드(`image`)

| 하위 커맨드 | 내용                                            | 생략 가능 여부 |     주요 옵션      |
| :---------: | :---------------------------------------------- | :------------: | :----------------: |
|   `pull`    | 도커 허브 등의 repository에서 이미지를 내려받음 |       O        | 거의 사용하지 않음 |
|    `rm`     | 도커 이미지 삭제                                |  `docker rmi`  | 거의 사용하지 않음 |
|    `ls`     | 내려받은 이미지 목록                            |       X        | 거의 사용하지 않음 |
|   `build`   | 도커 이미지를 생성                              |       O        |        `-t`        |

### 볼륨 조작 관련 커맨드(`volume`)

| 하위 커맨드 | 내용                           | 생략 가능 여부 |     주요 옵션      |
| :---------: | :----------------------------- | :------------: | :----------------: |
|  `create`   | 볼륨 생성                      |       X        |      `--name`      |
|  `inspect`  | 볼륨의 상세 정보 출력          |       X        | 거의 사용하지 않음 |
|    `ls`     | 볼륨 목록                      |       X        |        `-a`        |
|   `prune`   | 마운트되지 않은 볼륨 모두 삭제 |       X        | 거의 사용하지 않음 |
|    `rm`     | 지정한 볼륨 삭제               |       X        | 거의 사용하지 않음 |

### 네트워크 조작 관현 커맨드(`network`)

도커 네트워크란 도커 요소 간의 톤신에 사용하는 가장 네트워크이다.

| 하위 커맨드  | 내용                                   | 생략 가능 여부 |     주요 옵션      |
| :----------: | :------------------------------------- | :------------: | :----------------: |
|  `connect`   | 컨테이너를 도커 네트워크에 연결        |       X        | 거의 사용하지 않음 |
| `disconnect` | 컨테이너를 도커 네트워크 연결 해제     |       X        | 거의 사용하지 않음 |
|   `create`   | 도커 네트워크 생성                     |       X        | 거의 사용하지 않음 |
|  `inspect`   | 도커 네트워크 상세 정보 출력           |       X        | 거의 사용하지 않음 |
|     `ls`     | 도커 네트워크 목록                     |       X        | 거의 사용하지 않음 |
|   `prune`    | 컨테이너가 접속하지 않은 네트워크 삭제 |       X        | 거의 사용하지 않음 |
|     `rm`     | 지정한 네트워크 삭제                   |       X        | 거의 사용하지 않음 |

### 단독으로 쓰이는 커맨드

도커 허브의 검색이나 로그인에 사용되는 커맨드

| 하위 커맨드 | 내용                                 |     주요 옵션      |
| :---------: | :----------------------------------- | :----------------: |
|   `login`   | 도커 레지스트리에 로그인             |       -u -p        |
|  `logout`   | 도커 레지스트리에 로그아웃           | 거의 사용하지 않음 |
|  `search`   | 도커 레지스트리를 검색               | 거의 사용하지 않음 |
|  `version`  | 도커 엔진 및 명령행 도구의 버전 출력 | 거의 사용하지 않음 |


# SECTION 3. 컨테이너의 생성과 삭제, 실행, 정지

도커 커맨드에는 컨테이너를 생성하는 `docke (container) create`, 실행하는 `docker (container) start`, 이미지를 내려받는 `docker (container) pull` 이 따로 존재하지만 이를 한 번에 수행할 수 있는 `docker (container) run`을 사용하는 것이 일반적이다.

<p align = "center"><img src = "./images/docker-lifecycle.png" width = "70%"></p>

2장에서 나왔던 생애주기에 따라 컨테이너는 쓰고 버리는 방식으로 사용한다. 동작 중인 컨테이너를 그대로 삭제할 수 없으므로 정지하는 방법과 삭제하는 방법을 함께 알아야 한다.

## `docker (container) run`

컨테이너를 생성해 실행한다. 해당 이미지르 내려받으 상태가 아니라면 먼저 이미지를 내려받는다. '대상'으로는 이미지의 이름을 지정한다.

|             옵션 형식             | 내용                                                                                            |    Full name    |
| :-------------------------------: | :---------------------------------------------------------------------------------------------- | :-------------: |
|             `--name`              | 컨테이너 이름 지정                                                                              |        -        |
| `-p {host_port}:{container_port}` | 포트 번호 지정                                                                                  |   `--publish`   |
| `-v {host_disk}:{container_dir}`  | 볼륨 마운트                                                                                     |   `--volume`    |
|      `--net={natwork_name}`       | 컨테이너를 네트워크에 연결                                                                      |     `-env`      |
|    `-e {env-var_name}={vlaue}`    | 환경 변수를 설정                                                                                |        -        |
|               `-d`                | 백그라운드로 실행                                                                               |   `--detach`    |
|               `-i`                | 컨테이너에서 터미널(키보드) 연결 (표준 입력으로 컨테이너 생성)                                  | `--interactive` |
|               `-t`                | 특수 키를 사용 가능하도록 함 (터미널 드라이버를 추가하여 터미널을 이용하여 연결할 수 있도록 함) |     `--tty`     |
|              `-help`              | 사용 방법 안내 메세지                                                                           |        -        |

## `docker (container) stop`

```bash
docker stop {container_name}
```

## `docker (container) rm`

```bash
docker rm {container_name}
```

### 한 번만 실행되는 컨테이너와 데몬 형태로 동작하는 컨테이너

`-d`를 붙이지 않고 컨테이너를 싱행하면 실행된 컨테이너가 프로그램의 실행을 마칠 때까지 터미널의 제어를 차지하므로 다음 명령을 입력할 수 없다.
`-it`를 붙이지 않으면 컨테이너 안의 파일 시스템이 접근할 수 없다.

한 번만 실행되는 컨테이너는 실행하자마자 종료되므로 컨테이너가 터미널의 제어를 차지해도 일시적이므로 문제가 되지 않는다.
하지만 데몬처럼 지속적으로 실행되는 경우 저절로 종료되지 않으므로 한 번 터미널 제어를 넘지면 이를 되찾아오기 번거롭다.

또한 바로 종료되는 컨테이너의 경우 컨테이너 속 파일 시스템에 접근할 필욕사 없으므로 `-it`가 필요없는 옵션이 된다.

## `docker ps (docker container ls)`

```bash
# 실행 중인 컨테이너 확인
docker ps

# 존재하는 컨테이너 확인 (정지된 컨테이너 포함)
docker ps -a
```

> `ps`는 **pocess status**를 의미한다

### 컨테이너 목록 정보

목록을 출력하면 다음과 같은 정보들이 출력된다.
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

자세한 내용은 다음과 같다.

|      항목      | 내용                                                                                                        |
| :------------: | :---------------------------------------------------------------------------------------------------------- |
| `CONTAINER ID` | 컨테이너 식별자, 무작위 문자열이 할당된다. (SHA256 해시 알고리즘)                                           |
|    `IMAGE`     | 컨테이너를 만들 때 사용한 이미지의 이름                                                                     |
|   `COMMAND`    | 컨테이너 실행 시에 실행되도록 설정한 프로그램 이름                                                          |
|   `CREATED`    | 컨테이너 생성 후 경과된 시간                                                                                |
|    `STATUS`    | 컨테이너의 현재 상태. 실행 중이면 'Up', 종료 상태이면 'Exited'                                              |
|    `PORTS`     | 컨테이너에 할당된 포트 번호 'host_port -> container_port'형식으로 출력. 포트 번호가 동일할 경우 하나만 출력 |
|    `NAMES`     | 컨테이너 이름                                                                                               |


#### SHA256

docker에서의 입력값

- 이미지:
  - 이미지 레이어 정보
  - 파일 및 디렉터리의 해시 값
  - Dockerfile 명령과 빌드 옵션
- 컨테이너:
  - 랜덤 데이터(랜덤 시드, 시간, 호스트 정보 등)
  - 생성 시 옵션과 환경 변수


SHA-256은 **SHA-2(Secure Hash Algorithm 2)** 계열의 암호학적 해시 함수로, 입력 데이터를 고정된 256비트(32바이트) 해시 값으로 변환합니다. 데이터 무결성 검증 및 고유 식별자 생성 등에 널리 사용됩니다.

---

##### **특징**
1. **입력 크기 제한 없음**:
   - 어떤 크기의 데이터도 입력으로 처리 가능.

2. **출력 크기 고정**:
   - 항상 **256비트(64자리 16진수)** 길이의 해시 값을 생성.

3. **결정론적**:
   - 동일한 입력은 항상 동일한 해시 값을 생성.

4. **충돌 저항성**:
   - 서로 다른 입력이 동일한 해시 값을 가질 확률이 극히 낮음.

5. **비가역성**:
   - 해시 값으로 원본 데이터를 복원 불가.

6. **빠른 계산**:
   - 입력 데이터 크기와 관계없이 빠르게 계산 가능.

---

##### **주요 사용 사례**
1. **데이터 무결성 검증**:
   - 파일이 전송 중 변경되지 않았는지 확인.
   - 예: 소프트웨어 배포 시 파일의 SHA-256 해시 제공.

2. **암호학**:
   - 디지털 서명, 인증서, 전자 서명 등에 활용.

3. **비밀번호 저장**:
   - 비밀번호를 해시 값으로 변환하여 안전하게 저장.
   - 
4. **블록체인**:
   - Bitcoin 등 블록체인 기술에서 트랜잭션 및 블록 해시에 사용.

5. **고유 식별자 생성**:
   - 파일, 데이터, 이미지 등에서 고유 ID 생성.

---

##### **SHA-256의 동작 원리**
1. **패딩(Padding)**:
   - 입력 데이터를 512비트의 배수로 채움. 마지막 64비트는 원래 데이터 길이 정보 포함.

2. **초기 해시 값 설정**:
   - 256비트 크기의 초기 해시 값(8개의 32비트 단어)을 정의.

3. **메시지 분할**:
   - 입력 데이터를 512비트 블록으로 분할.

4. **압축 함수(Compression Function)**:
   - 각 블록에 대해 비트 연산을 반복하여 해시 값 갱신.

5. **최종 해시 값 출력**:
   - 모든 블록 처리 후, 256비트 최종 해시 값 반환.

---

##### **Python을 사용한 SHA-256 해시 생성 예제**
```python
import hashlib

data = "hello"
sha256_hash = hashlib.sha256(data.encode()).hexdigest()
print("SHA-256 Hash:", sha256_hash)

# SHA-256 Hash: 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
```
---


## [실습] 컨테이너 생성, 실행 상태확인, 종료, 삭제

```bash
PS C:\Users\Kwon> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# docker image pull + docker container create + docker container start
PS C:\Users\Kwon> docker run --name apa000ex1 -d httpd 
latest: Pulling from library/httpd
334a67c7f78b: Download complete
3ed0d9182dde: Download complete
d675ed392a91: Download complete
0062038102c9: Download complete
4f4fb700ef54: Download complete
2d429b9e73a6: Download complete
Digest: sha256:6bdbdf5ac16ac3d6ef543a693fd5dfafae2428b4b0cdc52a480166603a069136
Status: Downloaded newer image for httpd:latest
174a5c39573f4df13fad5620c4899bcde5828ff7b983e4456de688227e0ccecd

# 실행된 container 확인
PS C:\Users\Kwon> docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS     NAMES
174a5c39573f   httpd     "httpd-foreground"   12 seconds ago   Up 11 seconds   80/tcp    apa000ex1

# container 정지
PS C:\Users\Kwon> docker stop apa000ex1
apa000ex1

CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS                      PORTS     NAMES
# 정지되었으므로 ps -a로 확인할 수 있음 (STATUS: Exited)
PS C:\Users\Kwon> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

PS C:\Users\Kwon> docker ps -a
CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS                      PORTS     NAMES
174a5c39573f   httpd     "httpd-foreground"   3 minutes ago   Exited (0) 12 seconds ago             apa000ex1

# container 삭제
PS C:\Users\Kwon> docker rm apa000ex1
apa000ex1

PS C:\Users\Kwon> docker ps -a       
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

# SECTION 4. 컨테이너의 통신

## 아파치

> 아파치는 웹 서버 기능을 제공하는 소프트웨어이다.

웹 브라우저를 통해 들어온 요청에 따라 아파치 서버가 웹 사이트의 내용을 반환해준다.

## 컨테이너와의 통신

웹 브라우저를 통해 컨테이너에 접근하려면 외부와 접속하기 위한 설정이 필요하다. 이를 위해 포트를 설정한다.

> **port**는 통신 내용이 드나드는 통로이다.

docker에서는 `-p` 옵션으로 설정할 수 있다.

컨테이너를 사용하면 여러 개의 웹 서버를 함께 실행할 수도 있다. 이러한 경우 컨테이너와 연결하는 호스트의 포트 번호를 겹치지 않게 성정해야 한다.

### Reverse Proxy

#### Forard Proxy

client가 직접 server에 요청하는 것이 아니라 proxy server(중계 서버)를 거쳐 요청하는 것.
이렇게 되면 사실상 proxy server가 요청하는 것으로 되기 때문에 client가 누군지 server는 알 수 없다.

#### Reverse Proxy

reverse proxy는 server가 반환해주는 data를 proxy server가 대신 해주는 것이다. 그러므로 client는 server의 정보를 알 수 없다.
보안상 이점이 있으며, 서버 부담을 분산할 수 있다.

![](images/proxy.png)

reverse proxy를 활용하면 여러 대의 서버에 proxy가 요청을 전달 하게 할 수 있다. 그러므로 같은 포트로 요청이 들어왔을 때 proxy가 서버를 구분하는 방식으로 알맞은 컨테이너에 요청을 보낼 수 있다.

## [실습]

```bash
# host: 8080, container: 80으로 아파치 서버 container를 실행
PS C:\Users\Kwon> docker run --name apa000ex2 -d -p 8080:80 httpd
4ea55871346bea8332fdd1aee1c23620aff366895fbc573144b5cf4bd246d710
PS C:\Users\Kwon> docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS                  NAMES
4ea55871346b   httpd     "httpd-foreground"   20 seconds ago   Up 19 seconds   0.0.0.0:8080->80/tcp   apa000ex2

# 정지 및 삭제
PS C:\Users\Kwon> docker stop apa000ex2
apa000ex2
PS C:\Users\Kwon> docker rm apa000ex2  
apa000ex2
PS C:\Users\Kwon> docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS C:\Users\Kwon>
```

![실습 실행 화면](images/net_test.png)

위와 같은 실행 화면을 얻을 수 있다.

---

# SECTION 5. 컨테이너 생성에 익숙해지기

## 리눅스 운영체제가 담긴 컨테이너

리눅스 운영체제 컨데이너는 컨테이너 속 파일 시스템을 다루는 것을 전제로 하므로 인자로 '셀 명령어'를 지정한다.

즉 `-d` 없이 `-it` 옵션만 사용한다. 인자로는 `/bin/bash` 등 셀 명령어를 지정한다.

| 이미지 이름 | 컨테이너의 내용 |
| :---------: | :-------------: |
|   ubuntu    |     우분투      |
|   centos    |     CentOS      |
|   edbian    |     데비안      |
|   fedora    |     페도라      |
|   busybox   |     BizyBox     |
|   alpine    |  알파인 리눅스  |

## 웹 서버/데이터베이스 서버용 컨테이너

웹 서버는 통신이 전제가 되므로 옵션을 통해 포트 번호를 지정해야 한다.

데이터베이스 관리 소프트웨어는 기본적으로 **루트 패스워드**를 반드시 지정해야 한다.

| 이미지 이름 | 컨테이너의 내용 | 컨테이너 실행에 주로 사용되는 옵션 및 인자|
| :---------: | :-------------: |:---:|
|   httpd    |     우분투      |`-d` `-p`|
|   nginx    |     CentOS      |`-d` `-p`|
|   mysql    |     데비안      |`-d` `-e MYSQL_ROOT_PASSWORD`|
|   postgres    |     Postgre      |`-d` `-e POSTGRES_ROOT_PASSWORD`|
|   mariadb   |     MariaDB     |`-d` `-e MYSQL_ROOT_PASSWORD`|

## 프로그램 실행을 위한 런타임과 그 외 소프트웨어

프로그램을 실행하려면 해당 언어의 실행 환경인 **런타임**이 필요하다. 이 또한 컨테이너 형태로 제공된다.

| 이미지 이름 | 컨테이너의 내용 | 컨테이너 실행에 주로 사용되는 옵션 및 인자|
| :---------: | :-------------: |:---:|
|   openjdk    |     java 런타임      |`-d`를 사용하지 않고 인자로 java 명령 등을 지정해 도구 형태로 사용한다.|
|   python    |     python 런타임      |`-d`를 사용하지 않고 인자로 python 명령 등을 지정해 도구 형태로 사용한다.|
|   php    |     PHP 런타임      |웹 서버가 포함된 것과 실행 명령만 포함된 것으로 나위어 제공된다.|
|   ruby    |     ruby 런타임      |웹 서버가 포함된 것과 실행 명령만 포함된 것으로 나위어 제공된다.|
|   perl   |     perl 런타임     |`-d`를 사용하지 않고 인자로 perl 명령 등을 지정해 도구 형태로 사용한다|
|   gcc    |     C/C++ 컴파일러      |`-d`를 사용하지 않고 인자로 gcc 명령 등을 지정해 도구 형태로 사용한다|
|   node    |     Node.js      |`-d`를 사용하지 않고 인자로 app 명령 등을 지정해 도구 형태로 사용한다|
|   registry    |     도커 레지스트리      |`-d`옵션을 사용해 백그라운드로 실행한다. `-p` 옵션으로 포트 번호를 지정한다.|
|   wordpress    |     WordPress      |`-d`옵션을 사용해 백그라운드로 실행한다. `-p` 옵션으로 포트 번호를 지정한다. MySQL 또는 MariaDB가 필요하다. 접속에 필요한 패스워드는 `-e` 옵션으로 지정한다.|
|   nextcloud   |     NextCloud     |`-d`옵션을 사용해 백그라운드로 실행한다. `-p` 옵션으로 포트 번호를 지정한다.|
|   redmine   |     Redmine     |`-d`옵션을 사용해 백그라운드로 실행한다. `-p` 옵션으로 포트 번호를 지정한다. PostgreSQL 또는 MySQL이 필요하다|

## [실습] 아파치 컨테이너를 여러 개 실행하기

```bash
# 여러 개의 아파치 container 실행
PS C:\Users\Kwon> docker run --name apa000ex3 -d -p 8081:80 httpd
e21699803316c50412fefe15c8083f002b13f6a77a3fe44425f64df804622b0e
PS C:\Users\Kwon> docker run --name apa000ex4 -d -p 8082:80 httpd
fcfaac356cc40277c0855211b0394593db58bbae4c5e70bfb4cf60f10a47fdcb
PS C:\Users\Kwon> docker run --name apa000ex5 -d -p 8083:80 httpd
d656af9f2f9bc5c7326efe9c765888586c3564022b13ab14cd8348c8187c5460

PS C:\Users\Kwon> docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS                  NAMES
d656af9f2f9b   httpd     "httpd-foreground"   10 seconds ago   Up 10 seconds   0.0.0.0:8083->80/tcp   apa000ex5
fcfaac356cc4   httpd     "httpd-foreground"   16 seconds ago   Up 16 seconds   0.0.0.0:8082->80/tcp   apa000ex4
e21699803316   httpd     "httpd-foreground"   22 seconds ago   Up 22 seconds   0.0.0.0:8081->80/tcp   apa000ex3

# container 중지
PS C:\Users\Kwon> docker stop apa000ex3
apa000ex3
PS C:\Users\Kwon> docker stop apa000ex4
apa000ex4
PS C:\Users\Kwon> docker stop apa000ex5
apa000ex5

PS C:\Users\Kwon> docker ps -a
CONTAINER ID   IMAGE     COMMAND              CREATED              STATUS                      PORTS     NAMES
d656af9f2f9b   httpd     "httpd-foreground"   56 seconds ago       Exited (0) 10 seconds ago             apa000ex5       
fcfaac356cc4   httpd     "httpd-foreground"   About a minute ago   Exited (0) 13 seconds ago             apa000ex4       
e21699803316   httpd     "httpd-foreground"   About a minute ago   Exited (0) 17 seconds ago             apa000ex3       

# cooontainer 삭제
PS C:\Users\Kwon> docker rm apa000ex3
apa000ex3
PS C:\Users\Kwon> docker rm apa000ex4
apa000ex4
PS C:\Users\Kwon> docker rm apa000ex5
apa000ex5

PS C:\Users\Kwon> docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## [실습] Nginx 컨테이너 실행

```bash
# Nginx container 실행
PS C:\Users\Kwon> docker run --name nginx000ex6 -d -p 8084:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
171eebbdf235: Download complete
9ad567d3b8a2: Download complete
9b1039c85176: Download complete
773c63cd62e4: Download complete
4b0adc47c460: Download complete
1d2712910bdf: Download complete
Digest: sha256:bc5eac5eafc581aeda3008b4b1f07ebba230de2f27d47767129a6a905c84f470
Status: Downloaded newer image for nginx:latest
eb1e2ba350487505a13e5eda7c72690ca7e999d97d811635d5045c0495e6223f

PS C:\Users\Kwon> docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                  NAMES
eb1e2ba35048   nginx     "/docker-entrypoint.…"   11 seconds ago   Up 9 seconds   0.0.0.0:8084->80/tcp   nginx000ex6     

# container 중지/삭제
PS C:\Users\Kwon> docker stop nginx000ex6
nginx000ex6

PS C:\Users\Kwon> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                     PORTS     NAMES
eb1e2ba35048   nginx     "/docker-entrypoint.…"   33 seconds ago   Exited (0) 5 seconds ago             nginx000ex6      

PS C:\Users\Kwon> docker rm nginx000ex6
nginx000ex6
```

## [실습] MySQL 컨테이너 실행

```bash
# MySQL container 실행
PS C:\Users\Kwon> docker run --name mysql000ex7 -dit -e MYSQL_ROOT_PASSWORD=1234 mysql
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
7030c241d9b8: Pulling fs layer
f1a9f94fc2db: Download complete
c0fb96d14e5b: Download complete
f98254a2b688: Download complete
5f31e56c9bea: Download complete
d57074c62694: Download complete
6ad83e89f981: Download complete
a42d733ea779: Download complete
6fd1af2601dd: Download complete
0233a63dc5cd: Download complete
Digest: sha256:2be51594eba5983f47e67ff5cb87d666a223e309c6c64450f30b5c59a788ea40
Status: Downloaded newer image for mysql:latest
58f387c6520598518264e9f0a70ac6e6d9830bf381157aa96063f9eccfc867e6

PS C:\Users\Kwon> docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                 NAMES
58f387c65205   mysql     "docker-entrypoint.s…"   20 minutes ago   Up 20 minutes   3306/tcp, 33060/tcp   mysql000ex7

# container 중지/삭제
PS C:\Users\Kwon> docker stop mysql000ex7
mysql000ex7
PS C:\Users\Kwon> docker rm mysql000ex7
mysql000ex7
```

# SECTION 6. 이미지 삭제

container를 여러 번 만들다 보면 image는 그대로 남아있는 문제가 발생한다.
해당 이미지로 실행한 container가 남아 있으면 image를 삭제할 수 없으므로 사전에 container를 중지 및 삭제한다.

## `docker image rm`

여러 개를 한 번에 삭제할 수도 있다.

```bash
docker image rm image1 image2 image3
```

## `docker image ls`

image를 삭제하려면 imgae 이름 또는 id를 알아야 한다.

coantainer 목록을 불러오는 `docker ps`와 같이 `docker container ls`로 image 목록을 불러올 수 있다.
축약형은 `docker ls`이다.

```sh
PS C:\Users\Kwon> docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mysql        latest    2be51594eba5   4 weeks ago    825MB
nginx        latest    bc5eac5eafc5   6 weeks ago    279MB
httpd        latest    6bdbdf5ac16a   4 months ago   221MB
```

### image version

`image_name:version`으로 이미지의 버전을 지정할 수 있다.

```sh
# 아파치 2.2 버전을 지정해 실행
docker run --name apa000ex2 -d -p 8080:80 httpd:2.2
```

## [실습] 이미지 삭제

```sh
PS C:\Users\Kwon> docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mysql        latest    2be51594eba5   4 weeks ago    825MB
nginx        latest    bc5eac5eafc5   6 weeks ago    279MB
httpd        latest    6bdbdf5ac16a   4 months ago   221MB

# 축약형
PS C:\Users\Kwon> docker rmi httpd
Untagged: httpd:latest
Deleted: sha256:6bdbdf5ac16ac3d6ef543a693fd5dfafae2428b4b0cdc52a480166603a069136

PS C:\Users\Kwon> docker image ls 
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
mysql        latest    2be51594eba5   4 weeks ago   825MB
nginx        latest    bc5eac5eafc5   6 weeks ago   279MB

# image rm으로 삭제
PS C:\Users\Kwon> docker image rm nginx mysql
Untagged: nginx:latest
Deleted: sha256:bc5eac5eafc581aeda3008b4b1f07ebba230de2f27d47767129a6a905c84f470
Untagged: mysql:latest
Deleted: sha256:2be51594eba5983f47e67ff5cb87d666a223e309c6c64450f30b5c59a788ea40

PS C:\Users\Kwon> docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```