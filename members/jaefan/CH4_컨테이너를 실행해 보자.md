CH4. 컨테이너를 실행해 보자
---

## SECTION 01. 도커 엔진 시작하기/종료하기 

### 도커 엔진을 시작/종료하는 방법

- 도커 엔진은 설치와 함께 실행된다.
- 컨테이너를 실행 중이 아니라면, 리소스를 거의 차지하지 않는다. (별 문제 없음)
- 실제 서버는 24/7 실행되어야 한다. 
- 따라서, 도커 엔진은 자동 실행이 기본 설정 값이다.

다만, 컨테이너는 이야기가 다르다.
- 도커 엔진이 한 번 종료되면 모든 컨테이너는 정지 상태가 된다. 
- 컨테이너에는 자동 실행 설정이 없다. 

-> 도커 엔진은 컴퓨터를 켰을 때 함께 자동으로 실행할 수 있지만, 컨테이너는 그렇지 않다.

#### macOS 
1. 도커 엔진 시작 
   - Docker Desktop 애플리케이션 시작 
2. 도커 엔진 종료 
   - Docker Desktop 좌측 하단의 engine status 옆 케밥 메뉴 아이콘 (⁝) 클릭 후 'Quit Docker Desktop' 선택 
3. 자동 실행 설정 
   - 좌측 하단 케밥 메뉴 아이콘 (⁝) 클릭  -> Settings 클릭  
  -> General 선택 -> 'Start Docker Desktop when you sign in to your computer' 체크 (체크 해제 시 자동 실행 비활성화 )

## SECTION 02. 컨테이너의 기본적인 사용 방법

### 컨테이너 사용의 기본은 도커 명령어 

- 컨테이너를 다루는 모든 명령은 `docker` 명령어로 시작한다.

#### 명령어와 대상 
- docker 명령어 뒤에 오는 '무엇을', '어떻게'에 해당하는 부분을 '커맨드' 라고 한다. 
- 커맨드는 '상위 커맨드'와 '하위 커맨드'로 구성된다.
  - 상위 커맨드 -> '무엇을'
  - 하위 커맨드 -> '어떻게'
- '대상'에는 컨테이너명 또는 이미지명 등 구체적인 이름이 들어간다.

```shell
docker '무엇을' '어떻게' '대상' 
```

- 예를 들어, 이름이 penguin인 이미지를 컨테이너(container)로 실행(run) 하려면 아래와 같은 명령어가 필요하다.
```shell
docker container run penguin
```

#### 옵션과 인자 

- 명령어의 기본적인 형태는 `docker [command] [target]` 이지만 커맨드에는 앞서 설명한 대상 외에도 '옵션'과 '인자'라는 추가 정보가 붙는다.
- 예를 들면 다음과 같다.
```shell
docker container run -d penguin --mode=1
```
1. -d 라는 옵션은 '백그라운드로 실행하라' 라는 옵션이고,
2. --mode=1 이라는 인자는 '모드 1로 실행하라'라는 의미이다. ('컨테이너 내부 동작을 1번 모드로 동작해라.')

- 하지만 모든 명령어에 옵션이나 인자가 붙는 것은 아니다.

### 기본적인 명령어 - 정리 

- 정리하자면 기본적인 명령어는 다음과 같은 형태를 띤다.
```shell
docker 명령어 (옵션) 대상 (인자)
```

#### 커맨드
- 도커 명령어의 커맨드는 '무엇을', '어떻게' 할 것인지 지정하는 부분이다. 
- 컨테이너를 실행하고 싶다면, `container run` 명령어를 사용하면 된다.

  ```shell
  docker container run penguin 
  # 동일한 명령 
  docker run penguin
  ```
#### 옵션
- 옵션은 커맨든에 대한 세세한 설정을 지정하는 용도로 사용된다.
- 백그라운드 실행: -d
- 키보드 조작: -i / -t 등 
- 옵션은 '-' 또는 '--'로 시작하는 것이 일반적이나, '-' 기호를 붙이지 않는 경우도 존재한다.

    ```shell
    # 옵션 예 
    -d
    -all

    # --name 옵션 값의 예 
    --name penguin

    # -d, -i, -t 옵션을 합친 예 
    -dit  
    ``` 
    
#### 대상
- 커맨드와 달리 구체적인 이름을 지정한다. 
- 이름이 penguin인 이미지의 컨테이너를 실행하려면 `docker start [옵션] penguin` 명령을 사용한다.
  
#### 인자 
- 대상에 전달할 값을 지정한다. 
- 문자 코드 또는 포트 번호 등을 전달 할 수 있다. 
- 작성 방법은 옵션과 마찬가지로 '-' 또는 '--'로 시작하는 경우가 많다. 
  
    ```shell
    # 인자 예 
    --mode=1
    --style nankyoku
    ```

> **상위 커맨드는 생략 가능하다?**
> 
> 도커 1.13 부터 커맨드가 재편되면서 '상위 커맨드'와 '하위 커맨드'의 조합 형태로 일원화 되었다.  
> 이전에는 `container run` 대신 `run` 이라고만 표기해도 됐으나,  
> 이런 커맨드가 `container run`과 같이 `'상위 커맨드' + '하위 커맨드'` 형식으로 수정되었다.  
> 따라서, 상위 커맨드 + 하위 커맨드 형태로 명령어를 알아두는 것이 좋을 듯 하다. (언제 레거시가 될지 모름)

### [실습] 도커 버전 확인 
```shell
docker version
```
- 결과 
<p align="center">
<img src='./images/docker version.png' width=50%>
</p>

### 대표적인 명령어 

#### 컨테이너 조작 관련 커맨드 (상위 커맨드 `container`)

- 컨테이너를 실행하거나 종료하고, 컨테이너 목록을 확인하는 등 컨테이너를 다루기 위해 사용하는 커맨드로,  
  컨테이너를 대상으로 어떤 작업을 수행 할지는 하위 커맨드를 통해 지정한다.

    ```shell
    docker container 하위_커맨드 [옵션]
    ```
- 주요 하위 커맨드 
  - `start`: 컨테이너를 실행
  - `stop`: 컨테이너를 정지
  - `create`: 도커 이미지로부터 컨테이너를 생성
  - `run`: 도커 이미지를 내려받고 컨테이너를 생성해 실행함 (== docker image pull + docker contaier create + docker container start)
  - `rm`: 정지 상태의 컨테이너를 삭제 
  - `exec`: 실행 중인 컨테이너 속에서 프로그램을 실행 
  - `ls`: 컨테이너 목록 출력
  - `cp`: 도커 컨테이너와 도커 호스트 간에 파일을 복사 
  - `commit`: 도커 컨테이너를 이미지로 변환 

#### 이미지 조작 관련 커맨드 (상위 커맨드 `image`)

- 이미지를 내려받거나 검색하는 등 이미지와 관련된 기능을 실행하는 커맨드로,  
  이미지를 대상으로 어떤 작업을 수행 할지는 하위 커맨드를 통해 지정한다.
    ```shell
    docker image 하위_커맨드 [옵션]
    ```

- 주요 하위 커맨드 
  - `pull`: 도커 허브 등의 리포토리에서 이미지를 내려받음
  - `rm`: 도커 이미지 삭제 
  - `ls`: 내려 받은 이미지의 목록을 출력 
  - `build`: 도커 이미지 생성

#### 볼륨 조작 관련 커맨드 (상위 커맨드 `volume`)

- 볼륨 생성, 목록 확인, 삭제 등 볼륨(컨터이너에 마운트 가능한 스토리지)과 과련된 기능을 실행하는 커맨드로,  
  볼륨을 대상으로 어떤 작업을 수행 할지는 하위 커맨드를 통해 지정한다.
    ```shell
    docker volumne 하위_커맨드 [옵션]
    ```
- 주요 하위 커맨드 
  - `create`: 볼륨 생성
  - `inspect`: 볼륨 상세 정보 출력
  - `ls`: 볼륨 목록 출력
  - `prune`: 현재 마운트되지 않은 볼륨 목록 삭제 
  - `rm`: 지정한 볼륨 삭제 
  
#### 네트워크 조작 관련 커맨드 (상위 커맨드 `network`)

- 도커 네트워크의 생성, 삭제, 컨테이너의 네트워크 접속 및 접속 해제 등 도커 네트워크와 관련된 기능을 실행하는 커맨드이다.  
- 도커 네트워크란 도커 요소 간의 통신에 사용하는 가상 네트워크를 가리킨다.

    ```shell
    docker network 하위_커맨드 [옵션]
    ```
- 주요 하위 커맨드 
  - `connect`: 컨테이너를 도커 네트워크에 연결
  - `disconnect`: 컨테이너의 도커 네트워크 연결 해제 
  - `create`: 도커 네트워크 생성
  - `inspect`: 도커 네트워크의 상세 정보 출력
  - `ls`: 도커 네트워크 목록 출력
  - `prune`: 현재 컨테이너가 접속하지 않은 네트워크를 모두 삭제 
  - `rm`: 지정한 네트워크 삭제

#### 그 밖의 상위 커맨드 

- 위의 대표적인 상위 커맨드 외에도 다양한 상위 커맨드가 존재한다.
- 하지만, 아직까지 나와 같은 도커 초보자는 사용할 일이 없는 커맨드이기에 '그냥 이런 것도 있다' 정도만 알아두자.
- 그 밖의 상위 커맨드 
  - `checkpoint`: 현재 상태를 일시적으로 저장한 후, 나중에 해당 시점의 상태로 되돌릴 수 있다.
  - `node`: 도커 스웜의 노드를 관리하는 기능 
  - `plugin`: 플러그인 관리 기능
  - `secret`: 도커 스웜의 비밀값 정보 관리 기능 
  - `service`: 도커 스웜의 서비스 관리 기능
  - `stack`: 도커 스웜 또는 쿠버네티스에서 여러 개의 서비스를 합쳐 구성한 스택을 관리하는 기능
  - `swarm`: 도커 스웜을 관리하는 기능
  - `system`: 도커 엔진의 정보를 확인하는 기능
  
#### 단독 사용 커맨드 

- 상위 커맨드 없이 단독으로 쓰이는 커맨드가 4가지 있다. 
- 주로 도커 허브의 검색이나 로그인에 사용되는 커맨드이다.
- 단독 커맨드 
  - `login`: 도커 레지스트리에 로그인
  - `logout`: 도커 레지스트리에서 로그아웃
  - `search`: 도커 레지스트리에서 검색
  - `version`: 도커 엔진 및 명령행 도구의 버전 출력


## SECTION 03. 컨테이너의 생성과 삭제, 실행, 정지

### docker run 커맨드와 docker stop, docker rm 커맨드 

- 컨테이너의 생애주기 
  - 생성: create
  - 실행: start
  - 정지: stop
  - 폐기: rm

#### 컨테이너를 생성하고 실행하는 커맨드: docker container run
```shell
docker container run (옵션) 이미지 (인자)
```

- 컨테이너를 실행하는 커맨드
- 도커 컨테이너를 **생성**하고, **실행**하는 기능을 한다.
- 컨테이너 생성에 필요한 이미지가 없다면, **내려받는 기능**도 한다.
- `docker container create` + `docker container start` + `docker image pull` 의 기능을 모두 수행한다.
- '대상'으로는 사용할 이미지의 이름을 지정한다.
-  주요 옵션 
   -  `--name 컨테이너_이름`: 컨테이너 이름 지정
   -  `-p 호스트_포트번호:컨테이너_포트번호`: 포트 번호 지정
   -  `-v 호스트_디스크:컨테이너_디렉터리`: 볼륨 마운트 
   -  `--net=네트워크_이름`: 컨테이너를 네트워크에 연결
   -  `-e 환경변수_이름=값`: 환경변수 설정
   -  `-d`: 백그라운드로 실행
   -  `-i`: 컨테이너에 터미널(키보드) 연결
   -  `-t`: 특수 키를 사용 가능하도록 설정 
   -  `-help`: 사용 방법 안내 메시지 출력

#### 컨테이너를 정지하는 커맨드: docker container stop 
```shell
docker container stop 컨테이너_이름
```

- 컨테이너를 삭제하기 위해서는 먼저 반드시 컨테이너를 정지시켜야 한다. 
- 해당 커맨드에는 옵션이나 인자를 지정하는 경우가 많지 않다.

#### 컨테이너를 삭제하는 커맨드: docker container rm
```shell
docker container rm 컨테이너_이름
```

- 컨테이너를 삭제하는 커맨드이다.
- 정지 상태가 아닌 컨테이너를 대상으로 실행하면 오류가 발생하고, 컨테이너가 삭제되지 않는다.
- stop 커맨드와 마찬가지로 옵션이나 인자를 지정하는 경우가 거의 없다.


### docker ps 커맨드 
```shell
# 실행 중인 컨테이너 목록 출력 
docker ps 
# 현재 존재하는 모든 컨테이너 목록 출력
docker container ls
```
- `docker container ls` 와 동일 
- 현재 실행 중인 컨테이너 목록을 출력하는 커맨드
- -a 옵션을 추가하면 현재 존재하는 컨테이너(정지 상태 컨테이너 포함) 목록을 모두 출력한다. 
- 확인 가능한 정보  
    - CONTAINER ID: 컨테이너 식별자
    - IMAGE: 컨테이너 생성 시 사용한 이미지 이름
    - COMMAND: 컨테이너 실행 시 실행하도록 설정된 프로그램 이름
    - CREATED: 컨테이너 생성 후 경과 시간 
    - STATUS: 컨테이너 현재 상태
    - PORTS: 컨테이너에 할당 된 포트 번호 (host port num -> container port num)
    - NAMES: 컨테이너 이름

>### [실습] 컨테이너를 생성하고, 실행, 상태 확인, 종료, 삭제 해보자
>
>- 실습 대상: 아파치 컨테이너 
>- 아파치: 웹 서버 기능을 제공하는 소프트웨어
>- 아파치 이미지 이름: httpd
>
>#### step 01. run (container run)
>```shell
>docker run --name apa000ex1 -d httpd
>```
>- 결과  
>    <img src="./images/docker run.png" width=50%>
>
>#### step 02. ps (container ls)
>```shell
>dockr ps
>```
>
>- 결과  
>    <img src="./images/docker ps.png" width=70%>
>
>  - STATUS 가 'Up' 이므로 해당 컨테이너는 실행 중이다.
>
>#### step 03. stop (container stop)
>```shell
>docker stop apa000ex1
>```
>- 결과  
>    <img src="./images/docker stop.png" width=30%>
>
>#### stpe 04. ps (container ls)
>```shell
>dockr container ls
>```
>
>- 결과  
>    <img src="./images/docker ps02.png" width=60%>
>  
>  - apa000ex1 컨테이너가 목록에 보이지 않음 -> 정상 종료 
>
>#### step 05. ps with '-a' option
>```shell
>dockr ps -a
>```
>
>- 결과  
>    <img src="./images/docker ps-a.png" width=80%>
>
>  - STATUS가 'Exited'이므로 컨테이너는 존재하지만 종료된 상태이다.
>
>#### step 06. rm 
>```shell
>docker rm apa000ex1
>```
>- 결과  
>    <img src="./images/docker rm.png" width=30%>
>
>
>#### step 07. ps -a
>```shell
>docker ps -a
>```
>
>- 결과  
>    <img src="./images/docker ps-a02.png" width=60%>

## SECTION 04. 컨테이너의 통신 

### 아파치란?

- 웹 서버 기능을 제공하는 소프트웨어 
- 아파치가 동작 중인 서버에 파일을 두면 이 파일을 웹 사이트 형태로 볼 수 있다.  
(대부분의 웹 사이트는 HTML, 이미지 그리고 프로그램 파일로 구성)

- 웹 브라우저를 통해 컨테이너에 접근이 가능하게 하려면 컨테이너를 실행할 때 설정이 필요하다. 
- 또한, 이 설정은 컨테이너 생성 후 기본적으로 변경이 불가능하다.
- 따라서, docker run 커맨드에 옵션 형태로 설정한다.

### 컨테이너와 통신하려면 

- 웹 브라우저를 통해 컨테이너에 접근하려면 외부와 접속하기 위한 설정이 필요하다. 
- 이를 위해 포트(port)를 설정한다.
    > **포트란? portran?**
    > 통신 내용이 드나드는 통로를 의미한다.
- 아파치는 서버에서 정해둔 포트(80번 포트)에서 웹사이트에 대한 접근을 기다리다가  
  사용자가 이 포트를 통해 접근해 오면 요청에 따라 웹사이트의 페이지를 제공한다.
- 하지만 컨테이너 속에서 실행 중인 아파치는 외부와 직접적인 연결 상태가 아니기 때문에 외부에서 접근이 불가능하다.
- 따라서, 컨테이너를 실행 중인 물리적인 서버(컴퓨터)가 외부의 접근을 대신 받아 전달해준다.

```shell
# 포트 설정 방법
 -p host_port_num:container_port_num

# 실제 예시 
-p 8080:80
```

- 이때, 여러 웹서버를 사용할 예정이거나, host server의 Port가 이미 사용 중인 경우 겹치지 않게 포트번호를 설정해주어야 한다.

>### [실습] 통신이 가능한 컨테이너 생성 
>
>#### step 01. run 
>```shell
>docker container run --name apa000exe2 -d -p 8080:80 httpd
>```
>- 결과  
>  
>    <img src='./images/docker run apa000exe2.png' width=70%>
>
>#### step 02. ps 
>```shell
>docker container ls
>```
>- 결과  
>  
>    <img src='./images/docker container ls.png' width=80%>
>
>#### step 03. 웹 브라우저 확인
>- http://localhost:8080
>
>- 결과  
>  
>    <img src='./images/it works.png' width=10%>
>
>#### step 04. stop 
>```shell
>docker container stop apa000ex2
>```
>- 결과  
>  
>    <img src='./images/docker stop 02.png' width=40%>
>
>#### step 05. rm
>```shell
>docker container rm apa000ex2
>```
>- 결과  
>  
>    <img src='./images/docker rm 02.png' width=40%>
>
>#### step 06. ps -a
>```shell
>docker container ps -a
>```
>- 결과  
>  
>    <img src='./images/docker ps-a03.png' width=60%>

## SECTION 05. 컨테이너 생성에 익숙해지기 

(시간 상 스킵 ..)

## SECTION 06. 이미지 삭제 

### 이미지 삭제 

- 컨테이너를 삭제해도 이미지는 남는다. (빵을 다 먹어도 빵틀은 남아있다.)
- 이미지가 늘어나면 스토리지 용량에 부담이 된다. 
- 따라서, 필요 없어진 이미지는 바로바로 처리해주는 것이 좋다.
- 이미지를 삭제할 때는 `이미지 ID` 또는 `이미지 이름`으로 지정한다.
- 이미지로 실행한 컨테이너가 남아 있으면 이미지 삭제가 불가능하므로, 우선적으로 '컨테이너를 종료->제거' 한 후 이미지를 삭제해야 한다.

### docker image rm 커맨드 

- 이미지 삭제를 위해서는 `docker image rm` 커맨드를 사용한다.
- 여기서, `image` 커맨드는 생략이 불가능하다. (생략할 경우 `docker container rm` 커맨드가 실행됨)

```shell
# 이미지 이름을 이용한 단일 이미지 삭제
docker image rm 이미지_이름

# 이미지 ID를 이용한 단일 이미지 삭제 
docker image rm 이미지_ID

# 이미지 이름을 이용한 여러 이미지 삭제 
docker image rm 이미지_이름01 이미지_이름02 이미지_이름03
```

### docker image ls 커맨드 

- 이미지를 삭제하려면 이미지 이름 또는 이미지 ID를 알아야 한다.
- 컨테이너 목록을 출력하는 `docker container ls` (`docker ps`)가 있듯이, 이미지 목록을 확인하는 커맨드도 존재한다. 
- `docker container ls`와 달리 `-a` 옵션은 사용이 불가능하다. (이미지는 컨테이너와 달리 상태가 없는 정적인 존재)

- 이미지 목록의 정보 
  - REPOSITORY: 이미지 이름 
  - TAG: 버전 정보 
  - IMAGE ID: 이미지 식별자
  - CREATED: 이미지 생성 후 경과된 시간
  - SIZE: 이미지 전체 용량
  
>### [실습] 이미지 삭제하기
>
>#### step 01. 잔여 컨테이너 확인
>```shell
>docker container ls -a
>```
>
>- 결과 
>  
>  <img src='./images/docker container ls -a.png' width=70%>
>
>#### step 02. image ls
>```shell
>docker image ls
>```
>
>- 결과 
>  
>  <img src='./images/docker image ls.png' width=60%>
>
>#### step 03. image rm
>```shell
>docker image rm httpd
>```
>
>- 결과 
>  
>  <img src='./images/docker image rm httpd.png' width=60%>
>
>#### step 04. image ls
>```shell
>docker image ls
>```
>
>- 결과 
>  
>  <img src='./images/docker image ls 02.png' width=50%>
