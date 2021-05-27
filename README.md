# Docker
<img src="https://user-images.githubusercontent.com/62991586/119464616-dfe7bc80-bd7d-11eb-9b94-0e139f96bae4.png" width=500 height=500>

# 목차
1. [Docker&Kubernetes?](#docker&kubernetes?)
2. [Docker 설치부터 실행까지](#docker-설치부터-실행까지)

# Docker&Kubernetes?

## 🐳 Docker? 🐳
- 도커는 **컨테이너 기반의 오픈소스 가상화 플랫폼** 이다.  
- 계속해서 바뀌는 서버 관리/개발 방식을 편리하게 함  
- 예를 들어 jenkins, WordPress 와 같은 것들을 설치 시 디렉토리 생성과 **"docker-compose up"** 명령어를 통해? 쉽게 서버에 실행 가능

### 가상머신?
- 가상머신처럼 독립적으로 실행되지만 가상머신보다 **빠르고** 가상머신보다 **쉽고** 가상머신보다 **효율적**이다.

### ⚡️ 도커의 특징 ⚡️
- 확장성/이식성
  - 도커가 설치되어 있다면 어디서든 컨테이너를 실행할 수 있음
  - 아마존 webservice, azure 등 **어디서든** 돌아가 특정 회사나 서비스에 종속적이지 않다.  
  - 쉽게 개발 서버를 만들 수 있고 테스트서버 생성도 간편하다.
- 표준성
  - 도커를 사용하지 않는 경우 ruby, nodejs, go, php 로 만든 서비스들의 배포 방식은 제각각 다름
  - 컨테이너라는 표준으로 서버를 배포하므로 모든 서비스들의 배포과정이 동일해짐
  - ❌ capistrano? fabric? ftp? ❌
- 이미지
  - 이미지에서 컨테이너를 생성하기 때문에 반드시 이미지를 만드는 과정이 필요
  - Dockerfile 이라는 스크립트를 이용하여 이미지를 만들고 처음부터 재현 가능
  - 빌드 서버에서 이미지를 만들면 표준에 따라 해당 이미지를 이미지 저장소에 저장하고 운영서버에서 이미지를 불러옴
- 설정관리
  - 설정은 보통 환경변수로 제어함
  - MYSQL_PASS = password 와 같이 컨테이너를 띄울 때 환경변수를 같이 지정
  - 하나의 이미지가 환경변수에 따라 동적으로 설정파일을 생성하도록 만들어져야함
- 자원관리
  - 컨테이너는 삭제 후 새로 만들면 모든 데이터가 초기화됨
  - 업로드 파일을 외부 스토리지와 링크하여 사용하거나 S3같은 별도의 저장소가 필요
  - 세션이나 캐시를 memcached 나 redis 와 같은 외부로 분리

## Kubernetes
- **여러 대의 서버**와 **여러 개의 서비스**를 관리하기 쉽게
- 도커가 하나의 서버/서비스를 관리한다면 쿠버네티스는 여러 개의 서버/서비스를 관리
- 쉽게 말해서, 쿠버네티스 안에 **여러 개의 Docker가 존재**하는 것!

### ⚡️ 쿠버네티스의 특징 ⚡️
- 스케줄링
  - 컨테이너를 적당한 서버에 배포해주는 작업
  - 여러 대의 서버 중 가장~ 할 일 없는 서버에 배포하거나 그냥 차례대로 배포 또는 아예 랜덤하게 배포
  - 컨테이너 개수를 여러 개로 늘리면 적당히 나눠서 배포하고 서버가 죽으면 실행중이던 컨테이너를 다른 서버에 띄워준다
- 클러스터링
  - 여러 개의 서버를 하나의 서버처럼 사용
  - 작게는 몇 개 안되는 서버부터 많게는 수천 대의 서버를 하나의 클러스터로
  - 여기저기 흩어져있는 컨테이너도 가상 네트워크를 이용하여 마치 같은 서버에 있는 것처럼 쉽게 통신
- 서비스 디스커버리
  - 서비스를 찾아주는 기능!
  - 클러스터 환경에서 컨테이너는 어느 서버에 생성될지 알 수 없고 다른 서버로 이동할 수도 있음
  - 따라서 컨테이너와 통신을 하기 위해서 어느 서버에서 실행중인지 알아야 하고 컨테이너가 생성되고 중지될 때 어딘가에 IP 와 Port 같은 정보를 업데이트 해줘야 함
  - key-value 스토리지에 정보를 저장할 수도 있고 내부 DNS 서버를 이용한다.


# Docker 설치부터 실행까지

### 설치와 확인
- MacOS 설치 링크: https://hub.docker.com/editions/community/docker-ce-desktop-mac/
- 터미널에서 'docker version' 으로 버전 확인.
- 확인 시에 'Client' 와 'Server' 두 가지의 버전이 모두 나온다.
- 내가 **Client** 이고, localhost 에 Docker 가 떠 있는 것!
- 즉 **나 = Client 가 명령어를 입력하면 host 에 설치된 Docker daemon 이 명령어를 처리하고 그 결과를 화면에 출력**하는 것

### 도커 기본 명령어

- <details>
  <summary>run</summary>
  
  - 컨테이너를 실행
  
  ```
  docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
  ```
  |옵션|설명|
  |---|---|
  |-d|detached mode(백그라운드 모드)|
  |-p|호스트와 컨테이너의 포트를 연결|
  |-v|호스트와 컨테이너의 디렉토리를 연결|
  |-e|컨테이너 내에서 사용할 환경변수 설정|
  |--name|컨테이너 이름 설정|
  |--rm|프로세스 종료 시 컨테이너 자동 제거|
  |-it|-i 와 -t 를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
  |--network|네트워크 연결|
   
  - ubuntu 20.04 컨테이너 만들기
    ```
    docker run ubuntu:20.04
    ```
    - 터미널에 위 명령 입력 시 사용할 이미지(ubuntu)가 저장되어 있는지 학인하고 없다면 다운로드(pull) 한 후 컨테이너를 생성(create)하고 시작(start)한다.
    - 컨테이너는 정상적으로 실행됐지만 무엇을 할 지에 대해 명령어를 전달하지 않았기 때문에 컨테이너는 생성되자마자 종료. 컨테이너는 프로세스이기 때문에 실행중인 프로세스가 없으면 컨테이너는 종료됨.
  - /bin/sh 실행하기
    ```
    docker run --rm -it ubuntu:20.04 /bin/sh
    ```
    - 컨테이너 내부에 들어가기 위해서 sh로 쉘을 실행하고 쉘에서 키보드 입력을 위해(명령하기 위해) -it 옵션을 준다.
    - 추가적으로 프로세스가 종료되면 컨테이너가 자동적으로 삭제되도록 --rm 옵션도 추가
    - --rm 옵션이 없다면 컨테이너가 종료되더라도 삭제되지 않고 남아 있어 수동으로 삭제해야 한다.
  - 웹 어플리케이션 실행하기
    ```
    docker run --rm -p 5678:5678 hashicorp/http-echo -text="hello world"
    ```
    - hashicorp/http-echo 라는 컨테이너 띄움
    - detached mode(백그라운드 모드)로 실행하기 위해 -d 옵션을 추가하고 -p 옵션을 추가하여 컨테이너 포트를 호스트의 포트로 연결
    - 브라우저를 열고 localhost:5678 에 접속하면 메시지를 볼 수 있다.
    - 5678:5678 -> 5679:5678 로 변경 시 5679로 연결됨
  - Redis 실행하기
    ```
    docker run --rm -p 1234:6379 redis
    ```
    - Redis 라는 메모리기반 데이터베이스 실행
  - MySQL 실행하기
    ```
    docker run -d -p 3306:3306  \
      -e MYSQL_ALLOW_EMPTY_PASSWORD=true  \
      --name mysql  \
      mysql:5.7
    ```
    - MySQL 데이터베이스 실행
    ```
    docker exec -it mysql mysql
    create database wp CHARACTER SET utf8;
    grant all privileges on wp.* to wp@'%' identified by 'wp';
    flush privileges;
    quit
    ```
</details>

- <details>
  <summary>exec</summary>
  
  - exec 명령어는 run 명령어와 달리 실행중인 도커 컨테이너에 접속할 때 사용하며 컨테이너 안에 ssh server 등을 설치하지 않고 exec 명령어로 접속

</details>

- <details>
  <summary>ps</summary>
  
  - 실행중인 컨테이너 목록을 확인하는 명령어
  ```
  docker ps
  ```
  - 중지된 컨테이너도 확인하려면 -a 옵션 붙이기
  ```
  docker ps -a
  ```
  
</details>
- <details>
  <summary>stop</summary>
  
  - 실행중인 ***컨테이너를 중지*** 하는 명령어
  실행중인 컨테이너를 하나 또는 여러 개 중지 가능
  ```
  docker stop [OPTIONS] CONTAINER [CONTAINER...]
  ```
  
</details>
- <details>
  <summary>rm</summary>
  
  - 종료된 ***컨테이너를 완전히 제거***하는 명령어
  ```
  docker rm [OPTIONS] CONTAINER [CONTAINER...]
  ```
  
</details>
- <details>
  <summary>logs</summary>
  
  - 컨테이너가 정상적으로 동작하는지 확인하는 좋은 방법!
  - 기본 옵션과 -f, --tail 옵션 살펴보기
  ```
  docker logs [OPTIONS] CONTAINER(ID)
  ```
  - 로그를 출력하고 끝나는 기본과 달리 -f 옵션의 경우 페이지를 새로고침 할 때와 같이 변화가 있으면 대기하다가 로그를 새로 출력해줌.
  ```
  docker logs -f CONTAINER(ID)
  ```
  
</details>
- <details>
  <summary>images</summary>
  
  - 도커가 다운로드한 이미지 목록을 보는 명령어
  ```
  docker images [OPTIONS] [REPOSITORY[:TAG]]
  ```
  
</details>
- <details>
  <summary>pull</summary>
  
  - 이미지를 다운로드하는 명령어
  ```
  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
  ```
  
</details>
- <details>
  <summary>rmi</summary>
  
  - ***이미지를 삭제***하는 명령어
  - images 명령어를 통해 얻는 이미지 목록에서 이미지 ID를 입력하면 삭제가 된다.
  - 단, ***컨테이너가 실행중인 이미지는 삭제되지 않는다***
  ```
  docker rmi [OPTIONS] IMAGE [IMAGE...] 
  ```
  
</details>
- <details>
  <summary>network create</summary>
  
  - 도커 컨테이너끼리 이름으로 통신할 수 있는 가상 네트워크를 만든다.
  ```
  docker network create [OPTIONS] NETWORK
  ```
  - app-network 라는 이름으로 wordpress 와 mysql 이 통신할 네트워크를 만든다.
  ```
  docker network create app-network
  ```
  
</details>
- <details>
  <summary>volume mount(-v)</summary>
  
  - mysql을 삭제후에 (wordpress 를?) 다시 실행하면 컨테이너에 저장된 데이터베이스도 사라져서 오류가 발생 가능하다.
  ```
  docker stop mysql
  docker rm mysql
  docker run -d -p 3306:3306  \
      -e MYSQL_ALLOW_EMPTY_PASSWORD=true  \
      --network=app-network  \
      --name mysql  \
      mysql:5.7
  ```
  - 현재 디렉토리(pwd 명령어)와 mysql 디렉토리를 연결!
  ```
  docker stop mysql
  docker rm mysql
  docker run -d -p 3306:3306  \
      -e MYSQL_ALLOW_EMPTY_PASSWORD=true  \
      --network=app-network  \
      --name mysql  \
      -v [현재 디렉토리]/mysql:/var/lib/mysql  \
      mysql:5.7
  ```
  - ***컨테이너를 없애면 그 안에 있는 데이터가 날아가***기 때문에 ***중요한 데이터의 경우 -v 옵션으로 연결***해야 함!!
  
</details>
- <details>
  <summary>주요 정리!</summary>
  
  - 컨테이너 삭제 후 이미지까지 삭제!
  - mysql 삭제 시
  ```
  docker stop mysql
  docker rm mysql
  docker images  //image id 확인
  docker rmi [IMAGE ID]
  ```
  
</details>
