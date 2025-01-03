# 1. 도커 응용

## 도커 응용 기술들

- 컨테이너와 호스트 사이 파일 복사(6-2)
    - 컨테이너에서 호스트로, 호스트에서 컨테이너로 파일을 복사하는 방법
- 볼륨 마운트(6-3)
    - 바인드 마운트
    - 볼륨 마운트
- 컨테이너 이미지 만들기(6-4)
    - 컨테이너를 다른 컴퓨터나 서버로 복사하거나 똑같은 컨테이너를 여러 개 만들고 싶을 때
    - 개발환경에서 운영환경으로 컨테이너 배포할 때
- 컨테이너 개조(6-5)
- 도커 허브 로그인(6-6)
    - 자신이 직접 만든 컨테이너를 다른 사람에게 공개
    - 회사에서는 사내 레지스트리를 이용
- 도커 컴포즈(7)
    - 여러 컨테이너를 함께 실행하거나, 환경을 대량으로 생산할 때
    - 컨테이너와 주변 환경을 한꺼번에 만들고 종료하고 삭제
- 쿠버네티스(8)
    - 여러 대의 서버에서 컨테이너를 실행할 때 사용하는 컨테이너 오케스트레이션 도구
    - 주로 대규모 시스템 운영 시 사용

# 2. 컨테이너와 호스트 간에 파일 복사하기

## 파일 복사

- `컨테이너 → 호스트`, `호스트 → 컨테이너` 양방향 모두 가능
- 도커에서는 커맨드를 이용해 파일 복사
    
    ```bash
    docker cp <원본_경로> <복사할_경로>
    
    # 호스트 -> 컨테이너
    docker cp <호스트_경로> <컨테이너_이름:컨테이너_경로>
    
    # 컨테이너 -> 호스트
    docker cp <컨테이너_이름:컨테이너_경로> <호스트_경로>
    ```
    

## [실습] 호스트 파일을 컨테이너 속으로 복사

### 실습 내용

- 아파치 컨테이너 생성 및 실행 → 파일 복사 → 확인
- 아파치에 접근하면 초기 화면이 표시되는데, index.html 파일을 만들면 이 파일의 내용이 초기 화면보다 우선해 표시됨

### 0) 사전 준비

- mac에서 nano로 `index.html` 파일 생성하기

### 1) 아파치 컨테이너 생성

### 2) 호스트에서 컨테이너로 파일 복사

### 3) 확인

## [실습] 컨테이너 파일을 호스트로 복사

# 3. 볼륨 마운트

## 볼륨과 마운트

- 볼륨
    - 스토리지의 한 영역을 분할한 것
- 마운트
    - 대상을 연결해 운영체제 또는 소프트웨어의 관리하에 두는 것

## 데이터 퍼시스턴시

- ‘쓰고 버리는’ 도커의 성격 상, 컨테이너는 언젠가는 삭제됨(ex. 소프트웨어 업데이트)
    - 컨테이너 속에 데이터가 있다면 데이터도 함께 소멸됨
- 컨테이너는 생성 및 폐기가 매우 빈번하므로 매번 데이터를 옮기는 대신 처음부터 컨테이너 외부에 둔 데이터에 접근해 사용하는 것이 일반적임
- 이때 데이터를 두는 장소가 마운트된 스토리지 영역

## 스토리지 마운트 종류

### 볼륨 마운트

- 도커 엔진이 관리하는 영역 내에 만들어진 볼륨을 컨테이너에 디스크 형태로 마운트
- 이름만으로 관리가 가능하므로 다루기 쉬움 but 직접 조작하기 어려움
- 임시 목적의 사용, 자주 쓰지 않지만 지우면 안되는 파일을 두는 목적으로 많이 사용함

### 바인드 마운트

- 도커 엔진에서 관리하지 않는 영역의 기존 디렉터리를 컨테이너에 마운트
- 디렉터리 또는 파일 단위로 마운트
- 폴더(디렉터리) 속에 파일을 직접 두거나 열어볼 수 있기 때문에 자주 사용하는 파일을 두는 데 사용

### [참고] 임시 메모리(tmpfs) 마운트

- 디스크가 아닌 주 메모리 영역을 마운트
- 디스크보다 훨씬 빠른 속도로 읽고 쓰기 가능 → 접근 속도를 높일 목적으로 사용
- but 도커 엔진이 정지되거나 호스트가 재부팅되면 소멸함

## 두 가지 마운트 방식의 차이점

- 간단한지 복잡한지
- 호스트 컴퓨터에서 파일을 다룰 필요가 있는지
- 환경의 의존성을 배제해야 하는지

| 항목 | 볼륨 마운트 | 바인드 마운트 |
| --- | --- | --- |
| 스토리지 영역 | 볼륨 | 디렉터리 또는 파일 |
| 물리적 위치 | 도커 엔진의 관리 영역 | 아무 곳이나 가능 |
| 마운트 절차 | 볼륨 생성 → 마운트 | 기존 파일 또는 폴더를 마운트 |
| 내용 편집  | 도커 컨테이너를 통해 편집 | 일반적인 파일과 동일 |
| 백업 | 복잡한 절차 존재 | 일반적인 파일과 동일 |
- 볼륨 마운트
    - 도커 제작사에서 권장하는 방법
    - 장점
        - 도커 엔진 관리 하에 있으므로 사용자가 파일 위치 신경쓸 필요 없음
        - 실수로 지워버릴 일이 없다
        - 환경에 따라 경로가 바뀌는 일이 없다
        - 익숙해지면 손쉽게 사용할 수 있음
    - 단점
        - 컨테이너를 경유하지 않고 직접 볼륨에 접근할 방법이 없고, 억지로 볼륨을 수정하려 하면 볼륨 자체가 깨질 우려가 있다
        - 백업에도 복잡한 절차가 필요
- 바인드 마운트
    - 장점
        - 기존과 동일한 방식으로 파일 사용 가능
        - 도커 엔진과 무관하게 파일을 다룰 수 있음
- 파일을 직접 편집할 일이 많다면(ex. 워드프레스) 바인드 마운트를, 그렇지 않다면 볼륨 마운트를 사용하자

## 스토리지 영역 마운트

- 마운트하려는 스토리지의 경로가 컨테이너 속 특정 경로와 연결되도록 설정
    - run 커맨드의 옵션 형태로 지정해야 함

### 스토리지 마운트 절차

- 마운트될 스토리지 생성
    - 볼륨 마운트의 경우 마운트와 동시에 볼륨을 만들 수 있긴 하지만 권장하지는 않음
    - 마운트 전에 별도로 볼륨을 먼저 생성하는 것이 좋다

### 스토리지 영역 만들기

- 바인드 마운트
    - 원본이 될 폴더(디렉터리)나 파일 먼저 생성
- 볼륨 마운트
    - 볼륨 상위 커맨드를 사용해 먼저 볼륨을 생성

```bash
docker volume create <볼륨_이름>  # 볼륨 생성
docker volume rm <볼륨_이름>  # 볼륨 삭제
```

### 스토리지 마운트 커맨드

| command | 내용 |
| --- | --- |
| create | 볼륨 생성 |
| inspect | 볼륨 상세 정보 출력 |
| ls | 볼륨 목록 출력 |
| prune | 현재 마운트되지 않은 볼륨을 모두 삭제 |
| rm | 지정한 볼륨을 삭제 |

## [실습] 바인드 마운트

## [실습] 볼륨 마운트

## [참고] 볼륨 마운트 확인

### 운영 환경에서 확인하는 방법

- 별도의 컨테이너에 해당 볼륨을 마운트하고 이 컨테이너에서 볼륨의 내용을 보는 방법
- 원래 컨테이너에서 읽고쓰기한 데이터를 다른 컨테이너에서 확인하는 것으로 마운트를 확인

### 학습 환경에서 확인하는 방법

- 볼륨과 컨테이너는 별개의 요소로, 컨테이너를 폐기해도 볼륨은 그대로 남음
- 볼륨에 읽고 쓰기를 마친 후 해당 컨테이너를 삭제하고, 동일한 볼륨을 마운트해 새로운 컨테이너 생성

## [참고] 볼륨 백업

- 바인드 마운트라면 파일 복사만으로 백업이 끝나지만, 볼륨 마운트는 백업이 까다로움
- 볼륨 자체를 복사할 수 없기 때문에 별도의 리눅스 컨테이너를 연결해 볼륨의 내용을 압축하여 저장한다
    - 컨테이너 생성(run)과 함께 tar 명령어로 백업을 수행
    - 압축 파일을 컨테이너 밖에 저장해야 함

# 4. 컨테이너로 이미지 만들기

## 컨테이너로 이미지 만드는 방법

### commit 커맨드로 컨테이너를 이미지로 변환

- 컨테이너만 있으면 명령어 한 번으로 이미지를 만들 수 있어 간편함
- 컨테이너를 먼저 만들어야 한다

```bash
docker commit <컨테이너_이름> <새로운_이미지_이름>
```

### Dockfile 스크립트로 이미지 만들기

- Dockfile 스크립트를 작성하고 이 스크립트를 빌드해 이미지 만들기
- Dockfile로는 이미지 만드는 일 밖에 할 수 없다

```bash
docker build -t <생성할_이미지_이름> <재료_폴더_경로>
```

```docker
FROM 이미지_이름
COPY 원본_경로 대상_경로
RUN 리눅스_명령어
...
```

- 주요 Dockfile 인스트럭션

| 인스트럭션 | 내용 |
| --- | --- |
| FROM | 토대가 되는 이미지 지정 |
| ADD | 이미지에 파일이나 폴더 추가 |
| COPY | 이미지에 파일이나 폴더 추가 |
| RUN | 이미지를 빌드할 때 실행할 명령어 지정 |
| CMD | 컨테이너 실행할 때 실행할 명령어 지정 |
| ENTRYPOINT | 컨테이너 실행할 때 실행할 명령어 강제 지정 |
| ONBUILD | 이 이미지를 기반으로 다른 이미지를 빌드할 때 실행할 명령어 지정 |
| EXPOSE | 이미지가 통신에 사용할 포트를 명시적으로 지정 |
| VOLUME | 퍼시스턴시 데이터를 지정할 경로를 명시적으로 지정 |
| ENV | 환경 변수를 정의 |
| WORKDIR | RUN, CMD, ENTRYPOINT, ADD, COPY에 정의된 명령어를 실행하는 작업 디렉터리 지정 |
| SHELL | 빌드 시 사용할 쉘 변경 |
| LABEL | 이름이나 버전, 저작자 정보 설정 |
| USER | RUN, CMD, ENTRYPOINT, ADD, COPY에 정의된 명령어를 실행하는 사용자 또는 그룹 지정 |
| ARG | docker build 커맨드 사용할 때 입력받을 수 있는 인자 선언 |
| STOPSIGNAL | docker stop 커맨드를 사용할 때 컨테이너 안에서 실행 중인 프로그램에 전달되는 시그널 변경 |
| HEALTHCHECK | 컨테이너 헬스체크 방법을 커스터마이징 |