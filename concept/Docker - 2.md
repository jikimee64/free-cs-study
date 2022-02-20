## Docker 실습

- 도커와 관련된 기본 명령어를 실습
- 필자는 윈도우의 WSL2(Ubuntu 20.04.2 LTS) + docker + dockerhub 기반으로 실습할 예정이며 도커 설치와 관련된건 생략

### 기본 명령어

#### docker search [이미지명]
- Docker hub에서 사용가능한 docker image 목록을 가져옴
![Untitled](https://user-images.githubusercontent.com/52563841/154679582-0edfbb4d-f813-45f3-a2ec-8001fc9deba4.png)

#### docker pull [이미지명]
- docker search 를 통해 찾은 이미지 중 사용할 이미지를 다운 받는다.
- 별도의 버전을 명시하지 않는 경우 최신 버전을 다운 받는다.
![Untitled](https://user-images.githubusercontent.com/52563841/154679791-fe4b4b1e-5457-40ca-8dfc-ba17d862e558.png)

#### docker run [이미지명]
- 다운받은 이미지를 기반으로 컨테이너를 생성 & 실행하기 위한 명령어
- 별도의 버전을 명시하지 않는 경우 최신 버전을 다운 받는다.
![Untitled](https://user-images.githubusercontent.com/52563841/154680320-b6bf223f-d920-4543-a14a-59f7dfaeff22.png)
- 브라우저를 통해 ```http://localhost``` 으로 접속을 시도하면 접속이 되지 않는다.
- 사유는 컨테이너의 특성으로, **컨테이너는 다른 프로세스들과 별도의 공간에서 격리되어 실행**되기 때문에
서비스를 사용하기 위해서는 컨테이너로 접속하기 위한 정보가 필요하다.
- 아래 명령어를 통해 접속을 위한 포트를 열어준다.
![Untitled](https://user-images.githubusercontent.com/52563841/154685995-4e582c0c-74b5-47ed-98e3-cb0967a2d2ce.png)
- 아래와 같이 접속에 성공한 것을 알 수 있다.
![Untitled](https://user-images.githubusercontent.com/52563841/154686165-8176ead9-5595-46ec-bcab-8f084be422cc.png)
- ```-p``` 옵션을 사용하면 컨테이너에서 생성된 네트워크와 우리가 사용되는 네트워크를 통신하기 위해
포트포워딩을 하는 개념이다.

※ 컨테이너는 격리되어 있어 컨테이너마다 네트워크가 다르기 때문에 컨테이너 내부 포트는 같은 포트를 사용해도 되지만
외부포트는 같은 네트워크이므로 이미 사용중인 포트가 있을수도 있기 때문에 포트 충돌로 실행이 정상적으로 안될 수도 있다.   

![Untitled](https://user-images.githubusercontent.com/52563841/154686840-82b2d3f5-6e0a-4ae0-bbb9-0797d8541b62.png)
- 하지만 실행중인 터미널읕 종료할 경우 컨테이너도 종료되므로 백그라운드로 돌리기 위해선 ```-d```옵션을 사용한다.
- 위에 나온 값은 해당 컨테이너의 고유 ID로, 컨테이너를 실행할 때 마다 다른값을 가진다.

#### docker ps
- 현재 실행중인 컨테이너의 프로세스 목록을 확인할 수 있다.
![Untitled](https://user-images.githubusercontent.com/52563841/154687275-a79cf78b-bfa4-4e6c-a191-43c08da95e4c.png)
  - CONTAINER ID: 컨테이너의 고유한 아이디 해쉬값, 일부만 표출
  - COMMAND: 컨테이너 시작시 실행될 명령어(이미지안에 존재)
  - STATUS: 컨테이너의 상태 / Up: 실행중, Exited: 종료, Pause: 일시정지 
  - PORTS: 컨테이너가 개방한 포트와 호스트에 연결한 포트
  - NAME: 컨테이너의 고유한 이름, 컨테이너 생성시 --name으로 이름 부여 가능, 부여하지 않을 경우 도커엔진이
  임의로 부여, id와 마찬가지로 중복이 안됨
![Untitled](https://user-images.githubusercontent.com/52563841/154701914-8d5ce6d5-8011-4c59-ba1e-bea3598b7c67.png)
- ```-a``` 옵션을 통해서 이전에 중지된 컨테이너 기록도 확인할 수 있다.
- 중요한 점은 각각의 컨테이너 ID와 NAMES 값이 같은 이미지라도 다른값인 점이다.

#### docker stop
- 도커 컨테이너를 중지하기 위해 사용되는 명령어
- 컨테이너 ID 혹은 이름을 지정해서 중지할 수 있다.
![Untitled](https://user-images.githubusercontent.com/52563841/154702676-e7ec448b-31ba-4569-a58a-2f7de05b6737.png)
※ docker kill도 컨테이너를 중지하기 위해 사용되는 명령어이다. stop은 gracefully하게 종료, kill은 즉시 종료한다는
차이점이 있다.

#### docker start
- 중단된 도커를 다시 실행하기 위한 명령어이다.
- 컨테이너 ID 혹은 NAMES으로 중단된 컨테이너를 다시 시작할 수 있다.
![Untitled](https://user-images.githubusercontent.com/52563841/154704381-0c71a01d-7db6-4afd-a438-7a9eca593fb4.png)
- docker run과 혼동하면 안된다. docker run은 실행한 적이 없는 컨테이너 이미지를 실행하기 위한 명령어다.
- 만약 실행 시 자신의 컴퓨터에 컨테이너 이미지가 없는 경우 docker pull 명령이 같이 실행되어 새로운 컨테이너가
실행된다.
- 하지만 docker start의 경우 **이미 자신의 컴퓨터에 컨테이너가 존재하여 컨테이너 ID와 이름이 존재하는 경우**만
사용할 수 있다.

#### docker restart
- 컨테이너를 재시작하기 위해 사용되는 명령어
- 보통 컨테이너 서비스가 오동작을 하는 경우에 많이 사용한다.
![Untitled](https://user-images.githubusercontent.com/52563841/154704900-6058d08f-6f58-4398-b80a-636da26db70a.png)

#### docker logs
- 컨테이너 실행시 발생한 로그를 확인하는 명령어
- 애플리케이션의 상태에서 오류가 발생하면 가장 먼저 확인하는 명령어
![Untitled](https://user-images.githubusercontent.com/52563841/154705265-46777d10-7a89-49fc-8658-664fcafdf345.png)
- ```-f```옵션을 붙이면 리눅스의 tail -f 처럼 중지,실행 등의 로그를 실시간으로 확인할 수 있다.

#### docker images
- 다운로드한 이미지의 목록을 보여주는 명령어
- 이미지도 고유의 ID를 가지고 있으며 컨테이너 ID와 동일하지는 않다
![Untitled](https://user-images.githubusercontent.com/52563841/154705653-485c35f1-a899-4a0a-a0ba-042c8e7f5096.png)

#### docker exec
- 실행중인 컨테이너에서 해당 컨테이너로 접속하고 싶을 때 사용하는 명령어
- 우분투 이미지를 실행하고 컨테이너에 접속하는 명령어
![Untitled](https://user-images.githubusercontent.com/52563841/154710319-c1efb459-e833-452c-9b90-32e4c5fed634.png)
- ```-it```옵션은 STDIN 표준 입출력을 열고(-i, interactive) 가상 tty(pseudo-TTY)를 통해 접속(-t)하겠다는 의미
- 즉, 터미널과 비슷한 환경을 구성해 컨테이너와 상호적으로 입출력을 주고 받겠다 >> 컨테이너에 터미널 환경으로 접속
- 마지막엔 쉘 명령어를 지정한다. (sh, bash, zsh 등)
- ```-it```옵션을 제외하고 이미지를 실행시키면 이미지가 실행되지 않고 바로 종료된다.
  ![Untitled](https://user-images.githubusercontent.com/52563841/154711377-c4fba7de-071a-4756-9fd2-c2f13dfc9b8f.png)

#### docker rm
- 실행 혹은 중지된 컨테이너를 더 이상 사용하지 않을 때, 컨테이너를 지우는 명령어
![Untitled](https://user-images.githubusercontent.com/52563841/154706553-fae4c529-983b-425e-b184-1e6e63948f80.png)
- 실행 중인 컨테이너를 지울려고 하면 아래와 같은 에러가 뜬다.  
```Error response from daemon: You cannot remove a running container 57078b1cdc867fa39d212e693a01c2ec9b2e01b0529bfae204a7f0bd2a71ddbe. Stop the container before attempting removal or force remove```
- ```docker stop```을 먼저 하거나 ```-f``` 옵션을 줘서 강제로 지울 수 있다.

#### docker rmi
- 다운로드한 이미지를 삭제하는 명령어로, rm이 컨테이너만 삭제하는 것과 대조
![Untitled](https://user-images.githubusercontent.com/52563841/154707323-3dfaaabb-c1ff-4c34-a94f-e4cbe4088881.png)
- 이미지를 삭제하기 위해서는 해당 이미지가 컨테이너와 종속관계가 없어야 한다.
- 즉, 컨테이너가 실행중이거나 종료되었다고 해도, 컨테이너를 삭제한 후에 이미지 삭제를 진행해야 정상적으로 삭제된다.
- 강제로 이미지를 지우길 원한다면 ```-f```옵션을 사용한다.

#### Docker 컨테이너 및 이미지 한 번에 삭제하기
- 본인 컴퓨터에 생성된 모든 컨테이너와 이미지를 삭제하고 싶을때
```
docker stop `docker ps -a -q`
docker rm `docker ps -a -q`
docker rmi `docker images -a -q`
```
- Docker 초기화를 하고싶을때
```
$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N]
```

### Docker Volume 사용 
- 컨테이너는 프로세스 형태로 자원을 격리하여 사용하기 때문에 컨테이너가 삭제되면 기존에 저장되었던 데이터는
사라진다.
- 이를 예방하기 위해 docker volume을 사용하거나 로컬 컴퓨터 파일에 마운트(bind mount)하여 docker 내부에 생성되는 데이터를
외부에 저장할 수 있다.

#### docker volume
- postgres DB를 통한 실습
![Untitled](https://user-images.githubusercontent.com/52563841/154783143-93f8f241-16b2-4f15-8f87-16e026254bd4.png)
- ```-e```옵션은 docker의 환경변수를 지정하는 것. 이미지마다 환경변수값이 다르다.
- 아래와 같이 사용자와 DB, 테이블을 만든다.
```
$ docker exec -it postgres /bin/bash

root@ac61c662ee4c:/# psql -U postgres
psql (13.0 (Debian 13.0-1.pgdg100+1))
Type "help" for help.

postgres=# CREATE USER seongwon PASSWORD '1q2w3e4r' SUPERUSER;
CREATE ROLE

postgres=# CREATE DATABASE test OWNER seongwon;
CREATE DATABASE

postgres=# \c test seongwon
You are now connected to database "test" as user "seongwon".
test=# CREATE TABLE star (
id integer NOT NULL,
name character varying(255),
class character varying(32),
age integer,
radius integer,
lum integer,
magnt integer,
CONSTRAINT star_pk PRIMARY KEY (id)
);
CREATE TABLE

test=# \dt
        List of relations
 Schema | Name | Type  |  Owner
--------+------+-------+----------
 public | star | table | seongwon
(1 row)
```
- 컨테이너를 종료하고 다시 시작했을 때는 데이터가 남아 있다.
- 컨테이너를 자체를 삭제한 경우 해당 베이스 이미지만 그대로 사용하기 때문에 기존 데이터는 남아있지 않다.
- DockerVolume을 통한 마운트 방법
![Untitled](https://user-images.githubusercontent.com/52563841/154783342-5cc729c5-6180-4627-8e0b-c06e878a4cfa.png)
- 생성된 볼륨을 ```-v``` 옵션을 통해 지정 할 수 있다.
- 컨테이너 디렉터리가 호스트 디렉터리 내용으로 덮어씌어지는 개념
- 컨테이너를 삭제하고 재실행해도 계정 정보가 저장되어 있다.(생략)
- 아래 명령어를 통해서 볼륨 리스트와 해당 볼륨의 위치 및 상세 정보를 확인할 수 있다.
```
$  docker volume list
local     pgdata

docker volume inspect pgdata
[
    {
        "CreatedAt": "2022-02-19T02:54:09Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/pgdata/_data",
        "Name": "pgdata",
        "Options": null,
        "Scope": "local"
    }
]
```

- $(pwd) 문법을 통해 명령어 사용 가능(리눅스) 
```
docker run -d -p 5000:8080 -v /usr/src/app/node_modules -v $(pwd):/usr/src/app dncjf64/nodejs
```

- 볼륨 삭제, 컨테이너에 마운트되어 있는 volume은 컨테이너를 먼저 삭제하고 진행해야 한다
```docker volume rm {volume명}```

- 볼륨 초기화
  ```docker volume prune ```

#### 바인드 마운트
- ```:``` 앞에 볼륨 마운트명 대신에 호스트의 경로를 지정해주는 방법 
- ```-v /home/ubuntu/wordpress_db:/var/lib/mysql```
```
$ docker inspect one
    "Mounts": [
        {
            "Type": "bind", //bind 타입
            "Source": "/root",
            "Destination": "/app",
            "Mode": "",
            "RW": true,
            "Propagation": "rprivate"
        }
    ],
```

※ Docker 볼륨 vs 바인트 마운트
- 볼륨을 사용하면 직접 생성 및 삭제하는 불편함이 있지만, 해당 볼륨은 Docker 상에서 이미지나 컨테이너,
네트워크와 비슷한 방식으로 관리되는 이점(백업 용이, OS에 비의존적 등)이 있다. Docker 공식 문서에서도 권장하는 방법
- 하지만 컨테이너화된 로컬 개발 환경에서는 바인드 마운트로 변경사항을 실시간으로 컨테이너를 통해 확인할 수
있다는 장점이 있다.
- 단점은 볼륨에 비해 기능이 제한되어 있고 호스트의 파일 시스템(디렉터리 구조)에 의존적이다. 또한 볼륨에 비해
성능이 좋지 않다.

### Dockerfile 생성
- Dockerfile은 Docker image를 만들기 위한 설정 파일로, 컨테이너가 어떻게 행동해야 하는지에 대한
설정을 정의해주며 image를 만들어서 Docker Hub 등 에 업로드 할 수 있다.
- Dockerfile 명령어
- **FROM**
  - Docker image를 만드는데 필요한 가장 기본인 베이스 이미지 지정
  - ```FROM ubuntu:20.04```
- **LABEL**
  - Dockerfile을 작성한 개발자의 정보를 작성
  - ```LABEL jikimee64@gmail.com```
- **ARG**
  - Dockerfile을 빌드할 때, 설정할 수 있는 옵션을 지정
  - Dockerfile에서만 사용 가능
  - 보통 Dockerfile을 작성하는데 필요한 변수를 선언하여 사용
  - ```ARG DEBIAN_FRONTEND=noninteractive```
- **WORKDIR**
  - 명령을 실행하기 위한 디렉토리를 설정. 설정된 디렉터리가 없으면 새로 생성하고, 해당 디렉터리를 기본으로 동작한다
  - 해당 디렉터리에서 이후에 등장하는 모든 RUN, CMD, ENTRYPOINT, COPY, ADD 명령문은 해당 디렉터리를 기준으로 실행됨
  - ```WORKDIR /var/www/html```
- **RUN**
  - 이미지를 생성할 때 실행할 명령어 혹은 코드를 지정한다. 파일 권한을 변경하거나 라이브러리를 설치하는 경우에 많이 사용
  - 도커 파일로부터 도커 이미지를 빌드하는 순간에 실행
  - 자세히 말하면 이미지 레이어를 생성하는 것
  - ```RUN apt-get install -y -no-install-recommends apt-utils build-essential```
  - ```RUN yum -y install nginx(/bin/sh 예시, default)```
  - ```RUN ["/bin/bash", "-c", "yum -y install nginx"](exec 예시)```
- **ENV**
  - Dockerfile 또는 컨테이너 안에서 환경 변수로 사용
  - ```ENV TZ=Asia/Seoul```
- **COPY**
  - 이미지를 생성할 떄, 외부에서 파일을 이미지로 가져오기 위해 사용한다. 호스트에 설정된 퍼미션과 동일하게 복사
  - 왼쪽의 외부폴더(파일)을 오른쪽의 컨테이너 내부의 폴더(파일)로 복사
  - 호스트OS에서 컨테이너로 단순 복사만을 처리할 때 COPY 사용 
  - ```COPY ./pages /usr/local/apache2/htdocs/```
- **ADD**
  - COPY와 비슷하게 파일을 이미지로 가져오기 위해 사용하지만, 압축파일 인 경우 압축을 해제하고 파일 퍼미션이 지정되어 있음
  - COPY와 다르게 원격파일 다운 혹은 압축 해제 기능도 있음
  - ```ADD ./pages /usr/local/apache2/htdocs/```
- **EXPOSE**
  - 호스트와 연결할 기본 포트를 지정
  - ```EXPOSE 80```
- **CMD**
  - 컨테이너가 생성된 후 실행할 명령어 정의 
  - 여러개의 CMD 명령을 기술 할 경우 마지막의 CMD만 적용됨
  - ```CMD ["echo", "hello"]```
  - docker run 명령어에 쉘 명령어 및 인자값을 전달할 경우 **CMD에 작성된 명령어와 인자값은 무시** 된다.
  - 아래와 같이 ls -la라는 명령어를 주었을 경우 기존 CMD의 ```echo hello```는 생략되고 ls -la가 실행된다.
    ```
    docker container run -it [이미지이름] ls -la
    ```
- **ENTRYPOINT**
  - CMD와 동일하게 컨테이너가 생성된 후 실행할 명령어 정의
  - CMD와의 차이점으로는 컨테이너 실행 시 반드시 ENTRYPOINT에서 지정한 명령어를 수행한다
    - 따라서 변경되지 않을 명령어는 ENTRYPOINT로 정의하는 것이 좋다.
  - docker run 명령어로 지정한 인자값으로 변경 할려고 할시 에러가 발생하므로 컨테이너르 띄울때 변경되지 않을 명령어는
  ENTRYPOINT에 설정한다.
    - 에러가 발생하는 이유는 ENTRYPOINT는 전달된 인자와는 별개로 반드시 실행되는 명령어기 때문에 만약 인자를 전달한다면
    df -h(ENTRYPOINT) ps -aef(컨테이너 실행시 전달한 명령어)와 같은 명령어가 되기 때문에 에러가 발생한다
  - CMD와 같이 사용할 경우 ENTRYPOINT는 명령어, CMD는 매개변수 역할을 담당한다.
  - 도커 실행시 인자를 전달하면 마찬가지로 CMD에 지정한 것은 생략된다.
    ```
    ENTRYPOINT ["top"]
    CMD ["-d", "10"]
    ```

#### SpringBoot Dockerfile 예시1
```
FROM openjdk:1
LABEL seongwon "seongwon@edu.hanbat.ac.kr"

ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

WORKDIR /home/java
ARG JAR_FILE_PATH=build/libs/*.jar
COPY ${JAR_FILE_PATH} /home/java/application.jar

EXPOSE 8080

ENTRYPOINT [ "java", "-jar", "/home/java/application.jar" ]
```
- 위와 같은 방식은 프로젝트가 커질 수록 의존성 및 코드의 양이 증가되어 jar 파일이 무거워질수록 컨테이너
이미지를 만드는 시간이 오래 걸리게 된다.
- Docker 이미지는 레이어 계층으로 캐싱방식을 사용하여 효율적으로 이미지를 만든다.
- 위의 방식은 Java의 모든 구조가 jar파일로 되는 것이기 때문에 layer를 재사용하기 어렵다.
- 다시 말해서, 코드를 한 줄이라도 수정하면 캐쉬 해놓은 layer를 사용할 수 없기 때문에 처음부터 다시 만들어야 한다.

- 위의 문제를 해결하기 위해 layering feature를 사용
- 하나의 구조를 4개의 layer로 나눔으로써 변경이 적은 부분은 캐싱된 것을 사용하고, 변경이 잦은 부분만을 새로 구축해
효율적으로 컨테이너 이미지를 생성
![Untitled](https://user-images.githubusercontent.com/52563841/154732438-a86635fe-3d12-41ce-a1c7-d79c3c40c52e.png)
- dependencies가 제일 변경이 적고, application이 제일 변경이 잦은 부분
  - 위와 같은 레이어개념이 적용되게끔 빌드를 하려면 gradle에 설정을 적용해줘야 한다
    ```
    dependencies {
      implementation 'org.springframework.boot:spring-boot-gradle-plugin:2.5.0'
     }

     bootJar {
       layered()
     }
#### SpringBoot Dockerfile 예시2
- 만든 jar파일을 4가지 layer로 분리해서 확인
```
 java -Djarmode=layertools -jar my-app.jar extract
```
![Untitled](https://user-images.githubusercontent.com/52563841/154736466-a2f978f9-76ab-40c5-aa33-c8140ee71455.png)

- Dockerfile에서 아래와 같이 분할 및 실행 설정 적용&사용해서 이미지 생성 
```
FROM adoptopenjdk:11-jre-hotspot as builder
WORKDIR application
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

FROM adoptopenjdk:11-jre-hotspot
WORKDIR application
ENV server.port 8080
ENV spring.profiles.active default
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
CMD ["-spring.profiles.active=default"]
```
- 중요한 점은 COPY 명령 순서이다
- 변경빈도가 낮은 디렉토리에서 높은 디렉토리 순으로 복사를 해야 한다
- COPY 명령어 한줄한줄이 Docker 이미지에서 하나의 레이어를 차지하는데, 높은 -> 낮은 순으로 하게되면 
높은 디렉터리의 COPY에서 변경이 일어나서 새롭게 layer를 만들게 되면 그 이후부터 새로이 강제적으로 
layer가 만들어 지게 된다.

```
docker build --build-arg JAR_FILE={경로}/myapp.jar
```
- Dockerfile 실행
- jar가 있는 파일을 명시하고 싶으면 ```--build-arg```옵션을 사용해서 경로 변수 지정

#### SpringBoot 2.3이상 Dockerfile 없이 이미지 생성 
- gradle에 설정 후 Dockerfile 없이 이미지 파일을 만들려면 아래의 Gradle 명령어로 이미지 만들면 된다.
```
chmod +x gradlew (옵션: 리눅스에서 사용시)
./gradlew bootBuildImage --imageName=[원하는 이미지명]
```

#### docker build
- 생성한 Dockefile을 통해 이미지를 생성한다.
- Dockerfile이 있는 해당 디렉터리에서 아래 명령어를 실행하면 알아서 Dockerfile 이름을 가진 파일을 찾는다.
  ```
  docker build -t [출력할 이미지 이름] .
  ```
- ```-t```옵션을 사용하여 출력할 이미지 이름을 지정할 수 있다.
- ```-f```옵션을 사용하여 변경된 Dockerfile의 이름을 지정할 수 있다.

### Docker backup & restore
- Docker를 사용하다보면 이미지 혹은 컨테이너를 백업하고 복구하는 상황이 올 수 있다.

#### docker export
- 컨테이너를 백업하는 명령어
![Untitled](https://user-images.githubusercontent.com/52563841/154782823-806163ed-57d3-4f60-8886-3594b435c959.png)

#### docker import
- export한 컨테이너를 복구하는 명령어
  ![Untitled](https://user-images.githubusercontent.com/52563841/154782842-4014e0ff-66b9-4da2-9a98-c8aab691d824.png)

#### docker save
- 이미지를 백업
  ![Untitled](https://user-images.githubusercontent.com/52563841/154782948-64e7b0f2-6f32-410e-8796-532194f812fb.png)

#### docker load
- save한 이미지를 로드하는 명령어
  ![Untitled](https://user-images.githubusercontent.com/52563841/154782976-d7ca4a24-2d9a-47bf-af3e-4bc355dd8b28.png)

- export의 경우 컨테이너를 동작하기 위한 모든 파일이 압축되기 때문에 tar파일에는 루트 시스템 전체가 담겨져 있다.
- save의 경우는 이미지 레이어 구조까지 포함된 형태로 압축되기 때문에 출력된 파일이 .tar로 동일하더라도, 압축되어 있는 
파일 구조 및 디렉터리 형식이 다르기 때문에 매칭이되는 명령어로 저장/복구 해야 한다.