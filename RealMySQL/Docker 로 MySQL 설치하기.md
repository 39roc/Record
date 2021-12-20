# Docker 로 MySQL 설치하기

프로젝트별로 RDMBS 종류가 다른 경우, 인메모리 디비를 사용하는게 아닌 경우

로컬 개발 환경에서 특정 RDMBS 서버를 설치하기 귀찮을때가 종종 있다.

이럴때 도커를 사용하면 귀찮은 과정을 거치지 않고 이미지 받아서 쉽게 사용할 수 있어서 불편함을 해결할 수 있다.



Docker 로 MySQL 이미지를 받아서 컨테이너 실행하는 과정을 살펴본다.



```
docker pull mysql

docker images

docker 에 mysql 과 같은 DB를 설치하는 경우 컨테이너 삭제와 함께 데이터도 날라가므로, 저장소는 반드시 외부 저장소를 사용한다.

docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --name jmlim-mysql -v /Users/jmlim/datadir:/var/lib/mysql mysql:8.0.17

위 명령어를 실행하여 컨테이너를 올려도 되지만 docker-compose.yml 파일로 만들어서 실행가능

- 장황한 도커 옵션을 한눈에 볼 수 있어 편하다.

version: "3" # 파일 규격 버전
services: # 이 항목 밑에 실행하려는 컨테이너 들을 정의
  db: # 서비스 명
    image: mysql:8.0.17 # 사용할 이미지
    container_name: jmlim-mysql # 컨테이너 이름 설정
    ports:
      - "3306:3306" # 접근 포트 설정 (컨테이너 외부:컨테이너 내부)
    environment: # -e 옵션
      MYSQL_ROOT_PASSWORD: "password"  # MYSQL 패스워드 설정 옵션
    command: # 명령어 실행
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_unicode_ci
    volumes:
      - /Users/jmlim/datadir:/var/lib/mysql # -v 옵션 (다렉토리 마운트 설정)
      
      
docker-compose up -d
- docker-compose.yml 작성한 위치에서 실행
- 백그라운드로 실행 시 옵션 -d 붙이면됨

docker ps -a

docker exec -it [container_name] bash


```



 

[도커mysql언결](http://jmlim.github.io/docker/2019/07/30/docker-mysql-setup/)