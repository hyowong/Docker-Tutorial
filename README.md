# docker-tutorial
docker 정리 

### Docker
- 클라우드 환경의 급속한 성장
- 클라우드 환경에서의 설치 및 배포의 어려움
- 서비스 운영 환경을 이미지로 생성해서 배포한다

### Virtual Machine은 느리다
- 항상 guest OS를 설치해야 하는 VM &rightarrow; 이미지 용량이 커짐
- Docker는?
  - guest OS 필요 x
  - Docker Image에 서버 운영을 위해 필요한 것만 따로 설치
  - System Call 같은 OS 자원은 host system과 공유
  - 이미지 용량 줄어듬
  - 하드웨어 가상화 계층 x (mem, file system, network 속도 VM에 비해 매우 빠름)

### Docker Image, Container?
- base image ; 리눅스 배포판의 유저랜드만 설치된 파일
  - OS에는 **kernel / user** space 존재
  - 유저랜드란, user space에서 실행되는 파일과 라이브러리
    - (부팅에 필요한 최소 실행 파일 및 라이브러리, 패키징 시스템)
- **docker image** 라고 하면, 베이스 이미지에 필요한 프로그램과 라이브러리, 소스를 설치한 뒤 파일 하나로 만든 것
- docker image는 베이스 이미지에서 *바뀐 부분만 이미지로 생성*하고, 실행할 때는 각각을 합쳐 실행
  - 즉, 바뀐 부분만 이미지로 생성한 뒤, 부모 이미지를 계속 참조
- **docker container** 라고 하면, 이미지를 실행한 상태를 의미. 
  - 하나의 이미지로 여러개의 컨테이너 생성 가능

### 설치
- 는 알아서


### 주요 명령어들
- **[주의]** : root 권한 필요, 혹은 현재 계정을 docker 그룹에 포함
```
sudo usermod -aG docker ${USER}
sudo service docker restart
```
- docker hub로 부터 ubuntu **이미지 검색**
```
docker search ubuntu
```
- docker hub로 부터 **이미지(이미지이름:태그) 받기**
  - latest는 최신
  - 당연히 centos에서 ubuntu 이미지 실행 가능
```
docker pull ubuntu:latest
```
- **이미지 목록 출력**, 추가 옵션으로 이름 넣을 수 있음
```
docker images 
```
- **컨테이너 생성**
  - 이미지를 컨테이너화 한 이후에 bash셸 실행하기
  - docker run <옵션> <이미지 이름> <실행할 파일>
  - i, t 옵션 지정시 실행된 bash셸에 입출력 가능
```
docker run -i -t --name hello ubuntu /bin/bash
```
- **컨테이너 목록 확인**
  - a옵션 ; 정지된 컨테이너까지 보여줌
```
docker ps -a
```
- **컨테이너 시작**
  - 재시작은 restart
```
docker start hello
```
- **컨테이너에 접속**
  - 생성시 /bin/bash를 실행해야만 쉘에 접속해 명령을 입력할 수 있고
  - 그렇지 않다면 서버의 출력만 볼 수 있다.
  - 접속 후 **ctrl + D** 를 입력하면 컨테이너가 정지되고
  - **ctrl + P, ctrl + Q**를 차례대로 입력함으로써 컨테이너를 정지하지 않고 빠져나올 수 있다.
```
docker attach hello
```