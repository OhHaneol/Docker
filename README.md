# Docker
<img src="https://user-images.githubusercontent.com/62991586/119464616-dfe7bc80-bd7d-11eb-9b94-0e139f96bae4.png" width=500 height=500>

# 목차
1. [Docker&Kubernetes?](#Docker&Kubernetes?)
2. [Docker 설치부터 실행까지](#Docker 설치부터 실행까지)

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
