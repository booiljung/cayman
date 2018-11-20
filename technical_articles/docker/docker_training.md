# Docker 연습

개발용 컨테이너를 구성하기 위한 목적으로 Docker 다시 공부하면서 노트 합니다. Docker가 무엇인가? 이미지를 만드는 방법은 언급하지 않습니다.

도커의 탄생과 도커의 원리는 [[번역]시작하는 이들을 위한 컨테이너, VM, 그리고 도커에 대한 이야기](https://medium.com/@jwyeom63/%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-%EC%9D%B4%EB%93%A4%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-vm-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EB%8F%84%EC%BB%A4%EC%97%90-%EB%8C%80%ED%95%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0-3a04c000cb5c)를 참조 합니다.

## Docker란

'도커는 BSD와 솔라리스(Solaris)와  같은 유닉스(Unix) 운영체제에서 수십 년 간 사용되었던 개념이 현대적으로 재탄생된 최신 개념이다. 유닉스에서 사용되었던  개념이란 특정 프로세스를 운영체제의 나머지와 일정 수준 분리해 실행시킬 수 있다는 개념이다' [원문보기](http://www.itworld.co.kr/news/110748)

'리눅스 컨테이너를 이해하기 위한 출발점은  cgroups(control groups) 및 네임스페이스(namespaces)이다. 컨테이너와 호스트에서 실행되는 다른 프로세스  사이에 벽을 만드는 리눅스 커널 기능들이다. IBM이 최초 개발한 리눅스 네임스페이스는 시스템 리소스들을 묶어, 프로세스에 전용  할당하는 방식으로 제공한다.' [원문보기](http://www.itworld.co.kr/news/110748)

## Docker Container

격리된 공간에서 프로세스가 동작하는 기술입니다. 가상화 기술의 하나지만 기존의 OS 가상화 방식과 다릅니다. Docker Container는 Docker Image를 실행한 상태입니다. 하나의 Docker Image로 여러개의 Docker Container를 만들 수 있습니다.

## Docker Image

Docker Image는 서비스 운영에 필요한 서버 프로그램, 소스코드, 컴파일된 실행파일을 묶은 형태입니다. Docker Container에 필요한 파일과 설정을 포함하며 변하지 않습니다. Docker Image는 [Docker hub](https://hub.docker.com/)에 등록하거나 [Docker Registry](https://docs.docker.com/registry/)에 저장소를 만들어 관리할 수 있습니다.

### Dockerfile

도커 이미지를 만들기 위해 Dockerfile이라는 자체 Domain-Sepecific Language를 사용하여 이미지 생성 과정을 적습니다.

### Union File System

Docker는 베이스 이미지에서 바뀐 부분만 이미지로 생성합니다. 컨테이너로 실행할때는 베이스 이미지와 바뀐 부분을 합쳐서 실행합니다. Docker Hub 및 개인 저장소에서 이미지를 공유할때 바뀐 부분만 주고 받습니다. 이미지는 의존관계를 형성합니다.

## Ubuntu나 CentOS에 도커 설치

### 셸스크립트를 사용하여 자동으로 설치

```sh
curl -fsSL https://get.docker.com/ | sudo sh
```

또는

```sh
sudo wget -q0- https://get.docker.com/ | sh
```

get.docker.com` 셸스크립트로 도커를 설치하면 hello-world 이미지도 자동 설치되므로 삭제합니다.

```
sudo docker rm 'sudo docker ps -aq'
sudo docker rmi hello-world
```

### 자동 설치 스크립트를 사용하지 않고 직접 설치.

#### 데비안 리눅스 패키지로 설치

```
sudo apt update
sudo apt install docker.io
sudo apt ln -sf /usr/bin/docker.io /usr/local/bin/docker
```

마지막 라인은 `/usr/bin/docker.io`실행 파일을 `/usr/local/bin/docker`로 링크하여 사용합니다.

#### Redhat Enterprise Linux나 CentOS에서 패키지로 직접 설치

CentOS:

```
sudo yum install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
sudo yum install docker-io
```

AWS EC2에 설치되는 Amazon Linux (RHEL 기반)은 EPEL 저장소를 바로 사용할 수 있으므로 epel-release-6-8.noarch.rpm을 설치하지 않아도 됩니다.

CentOS 7:

```
sudo yum install docker
```

Docker 서비스 실행:

```
sudo service docker start
```

부팅했을때 Docker 서비스를 자동으로 실행하기

```
sudo chkconfig docker on
```

### 배포판 저장소를 사용하지 않고 직접 설치하기.

배포판 저장소는  보수적으로 오래된 경우가 있으므로 도커 패키지 버전이 낮은 경우가 있습니다. 배포판 패키지를 사용하지 않고 빌드된 바이너리를 직접 설치 합니니다.

이미 패키지로 설치 하였을때:

```
sudo service docker stop
sudo wget https://get.docker.com/builds/Linux/x86_64/docker-lastest \
-O $(type -P docker)
sudo service docker start
```

새로 설치할때:

```
wget https://get.docker.com/builds/Linux/x86_64/docker-lastest
chmod +x docker-lastest
sudo mv docker-lastest /usr/local/bin/docker
sudo /usr/local/bin/docker -d
```

아래는 뭔지 모르겠음.

```
sudo usermod -aG docker $USER # 현재 접속중인 사용자에게 권한 부여
sudo usermod -aG docker your-user # user-user 사용자에게 권한 부여
```

사용자가 로그인 중이라면 다시 로그인 후에 권한이 적용됩니다.

## 도커의 실행

```sh
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARGS...]
```

### 도커 실행 OPTIONS

|  옵션   | 설명                                                         |
| :-----: | ------------------------------------------------------------ |
|  `-d`   | detached mode. 백그라운드 모드.                              |
|  `-p`   | 호스트와 컨테이너의 포트를 연결 (포트 포워딩)                |
|  `-v`   | 호스트와 컨테이너의 디렉토리를 연결 (마운트)                 |
|  `-e`   | 컨테이너 내에서 사용할 환경변수 설정                         |
| `-name` | 컨테이너 이름 설정                                           |
|  `-rm`  | 프로세스 종료시 컨테이너 자동 제거                           |
|  `-it`  | `-i`와 `-t`를 동시에 지정한 것으로 터미널 입력 옵션. 키보드를 사용할 수 있게 합니다. |
| `-link` | 컨테이너 연결 [컨테이너명:별칭]                              |

도커의 기본 네트워크모드는 Bridge 모드로 약간의 성능 손실이 있습니다. 성능이 중요한 프로그램에 주는 옵션

```sh
--net = host
```

을 지정합니다.

### 도커 실행 예제

아래는 우분투 16.04 컨테이너를 실행 합니다.

```sh
docker run ubuntu:16.04
```

`run` 커맨드를 사용하여 이미지가 없으면 자동으로 `pull`을 사용하여 다운로드하고 컨테이너를 `create` 생성하고 `start` 합니다. 컨테이너는 프로세스 이기때문에 실행중인 프로세스가 없으면 컨테이너는 종료 합니다.

 `/bin/bash` 명령어를 실행하며 컨테이너를 실행 합니다.

```sh
docker run --rm -it ubuntu:16.04 /bin/bash
# 컨테이너 실행
$ ls
bin  dev home lib64 mnt proc run  srv tmp var
boot etc lib  media opt root sbin sys usr
```

컨테이너 내부에 들어가기 위해 `bash`를 실행하고, 키보드를 사용하기 위해 `-it` 옵션을 주었습니다.

### redis 컨테이너 실행 예제

```sh
docker run -d -p 1234:6379 redis
# redis 실행 (컨테이너 ID)
$ telnet localhost 1234
set mykey hello
+OK
get mykey
$5
hello
```

`-d` 옵션을 주어서 백그라운드로 돌리고 셸로 돌아 옵니다. `-p` 옵션을 주어 호스트의 `123`포트를 컨테이너 `6379`포트로 포워딩 하였으므로 localhost의 `1234`포트로 접속하면 redis를 사용 할 수 있습니다.

### MySQL 5.7 컨테이너 실행 예제

```sh
docker run -d -p 3306:3306 -e MYSQL_ALLOW_PASSWORD=true --name mysql mysql:5.7
# mysql 실행
$ mysql -h127.0.0.1 -uroot
mysql> shoqdatabases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
mysql> quit
```

`-e` 옵션을 주어 환경변수를 지정하고, `-name` 옵션을 주어 컨테이너 이름을 부여 합니다. 사용자가 컨테이너 이름을 부여하지 않으면 도커가 유명한 과학자나 해커의  이름과 수식을 조합하여 랜덤으로 이름을 생성합니다.

패스워드 없이 root 계정을 만들기 위해 `MYSQL_ALLOW_EMPTY_PASSWORD` 환경변수를 설정합니다. 컨테이너 이름은 `msql`로 지정하고, `-d` 옵션을 지정하여 백그라운드로 실행합니다. 호스트 포트 `3306`을 컨테이너 포트 `3306`으로 그대로 지정합니다.

MySQL 컨테이너의 데몬이 올라오는데 시간이 걸리기때문에 기다리지 않고 명령을 보내면 오류가 발생할 수도 있습니다.

### Wordpress + MySQL  컨테이너 예제

```sh
$ mysql -h127.0.0.1 -uroot
create database wp CHARACTER SET utf8;
grant all privileges on wp.* to wp@'%' identified by 'wp';
flush privileges;
quit

# run wordpress container
docker run -d -p 8080:80 --link mysql:mysql \
  -e WORDPRESS_DB_HOST=mysql \
  -e WORDPRESS_DB_NAME=wp \
  -e WORDPRESS_DB_USER=wp \
  -e WORDPRESS_DB_PASSWORD=wp \
  wordpress
```

워드프레스는 데이터베이스를 필요로 하며, MySQL을 사용하고 싶다면 `--link`  옵션을 지정하여 MySQL 컨테이너를 연결합니다. `--link` 옵션은 deprecated 되었다고 합니다.

### Tensorflow 컨테이너 예제

```sh
docker run -d -p 8888:8888 -p 6006:6006 teamlab/pydata-tensorflow:0.1
```

## Docker 기본 명령

### `ps`: 실행중이거나 실행했던 컨테이너 목록 확인

```sh
docker ps [OPTIONS]
```

### `stop`:  실행중인 컨테이너를 종료.

```sh
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

다음은 컨테이너 목록을 확인하고 중지하는 예제입니다.

```
docker ps
docker stop <컨테이너 ID>
docker ps -a #모든 컨테이너 보기
```

도커 ID 전체 길이는 64자리이지만, 겹치지 않는다면 앞자리 일부만 지정해도 됩니다. 예를들어 컨테이너 이름이 `abcdefghijklm`이고 겹치지 않는다면 `abcd`만 지정해도 됩니다.

### `rm`: 종료된 컨테이너 제거하기

종료된 컨테이너를 완전히 제거 합니다.

```sh
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

컨테이너 제거 예제

```sh
docker ps -a
docker rm <컨테이너 ID> <컨테이너 ID> ...
docker ps -a
```

중지된 컨테이너 모두 제거

```sh
docker rm -v $(docker ps -a -q -f status=exited)
```

### `images`: 이미지 목록 확인

docker가 다운로드한 이미지를 학인 합니다.

```sh
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

간단하게 docker 이미지 목록만 확인

```sh
docker images
```

### `pull`:  이미지 다운로드

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

`ubuntu:14.04`를 다운로드

```sh
docker pull ubuntu:14.04
```

`run` 을 실행할때 이미지가 없으면 자동으로 다운로드 받아 `pull`을 사용할 경우가 없지만, 업데이트된 새로운 이미지를 다운로드 할때 `pull`을 사용하여 다운로드 합니다.

### `rmi`: 이미지 삭제

```sh
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

`rmi` 삭제 예제

```sh
docker images
docker rmi <이미지 ID>
```

이미지는 여러개의 레이어로 구성되기 때문에 다수의 레이어가 삭제 됩니다.

### `logs`: 컨테이너 로그 조회

```sh
docker logs [OPTIONS] CONTAINER
```

```sh
docker logs <컨테이너 ID>
```

를 실행하면 전체 로그가 보입니다. `--tail 숫자`를 지정하여 마지막 로그 일부만 볼 수도 있습니다.

```sh
docker logs --tail 10 <컨테이너 ID>
```

최근의 10개의 로그만 표시합니다.

`-f` 옵션을 지정하여 실시간 로그 조회를 합니다.

```sh
docker logs -f <컨테이너 ID>
```

실시간 로그를 중지하려면 `ctrl + c`를 입력합니다.

docker는 컨테이너의 프로그램이 `stdout`, `stderror`로 출력하는 스트림을 수집하여 로그로 스트림 하므로, 컨테이너에서 실행하는 프로그램의 로그는 표준 출력으로 구성해야 합니다.

컨테이너 로그 파일은 json 형식으로 저장됩ㄴ디ㅏ. 로그가 많으면 파일의 용량이 커지므로 주의해야 합니다. docker는 플러그인을 통해 json이 아닌 특정 로그 서비스에 스트림으로 로그를 전달 할 수 있습니다. 규모가 커지면 로그 서비스 이용을 고려 해야 합니다.

### `exec`: 컨테이너 명령 실행

컨테이너 명령 실행시 `SSH`로도 가능하지만 바른 방법이 아니며 권장하지 않습니다. `exec` 명령을 통해 컨테이너의 명령을 실행해야 합니다.

```sh
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

MySQL에 접속해 보겠습니다.

```sh
docker exec -it mysql /bin/bash
```

`mysql` 클라이언트를 실행하므로 `-it` 옵션을 지정하여 키보드를 사용하고, `-bash` 옵션을 지정하여 bash 셸로 접속하였습니다.

셸을 통하지 않고 직접 `mysql` 을 실행할 수도 있습니다.

```sh
docker exec -it mysql mysql -uroot
```

첫번째 `mysql` 은 컨테이너 ID이고, 두번째 `mysql`은 MySQL 클라이언트를 실행하는 명령 입니다.

## 컨테이너 업데이트

컨테이너를 업데이트 한다는 것은, 기존 컨테이너를 `stop`, `rm` 하고, 새 이미지를 `pull`하여, 새 컨테이너를 `run` 하면 되는데, 기존 컨테이너를 `rm` 하면 컨테이너가 생성한 컨테이너 내부의 파일이 모두 잃게 됩니다.

그래서 컨테이너가 삭제되어도 유지되어야 할 파일들은 반드시 컨테이너 밖에 저장해야 합니다. 클라우드 서비스를 사용중이 아니라면 '데이터 볼륨'을 컨테이너에 추가해서 사용합니다. 데이터 볼륨을 사용하면 해당 폴더는 컨테이너와 별도로 저장되고 컨테이너를 삭제해도 남아 있습니다.

데이터 보륨을 사용하는 방법은 몇가지가 있습니다.

### 호스트의 디렉토리를 마운트

데이터 볼륨을 위해 호스트의 `/my/own/datadir` 디렉터리를  컨테이너의 `/var/lib/mysql`디렉터리로 마운트 합니다.

```sh
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  -v /my/own/datadir:/var/lib/mysql \ # <- volume mount
  mysql:5.7
```

## Docker Compose

컨테이너 조합이 많아지고 설정이 추가되면 명령어가 복잡해집니다. 이를 간단하게 하기 위해 Docker Compose라는 도구를 사용합니다.

### Docker Compose 설치

OSX나 Windows는 Docker를 설치할때 함께 설치 됩니다. 리눅스의 경우 아래처럼 설치합니다.

```sh
curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose version
```

#### Docker Compose를 사용하여 Wordpress 만들기

먼저 빈 폴더를 하나 생성하고 `docker-compose.yml` 파일을 만들어 아래처럼 구성합니다.

```yaml
version: '2'

services:
	db:
		image: mysql:5.7
		volumes:
			- db_data:/var/lib/mysql
		restart: always
		envrionment:
			MYSQL_ROOT_PASSWORD: wordpress
			MYSQL_DATABASE: wordpress
			MYSQL_USER: wordpress
			MYSQL_PASSWORD: wordpress
	
	wordpress:
    	depends_on:
    		-db
    	image: wordpress:latest
    	volumes:
    		- wp_data:/var/www/html
    	port:
    		-"8000:80"
    	restart: always
    	environment:
    		WORDPRESS_DB_HOST: db:3306
    		WORDPRESS_DB_PASSWORD: wordpress
    volume:
    	db_data:
    	wp_data:    		
```

아래처럼 실행 합니다.

```sh
docker-compose up
```

명령어를 설정 파일로 바꾸었고, 가독성이 상승하고 재활용이 가능해졌습니다.



------

## 참조

- [초보를 위한 도커 안내서 - 도커란 무엇인가?](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)

- [초보를 위한 도커 안내서 - 설치하고 컨테이너 실행하기](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)

- [초보를 위한 도커 안내서 - 이미지 만들고 배포하기](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)

- [도커와 도커 컨테이너의 이해](http://www.itworld.co.kr/news/110748)
- [도커 무작정 따라하기](https://www.slideshare.net/pyrasis/docker-fordummies-44424016)
- [Docker Container로 SFTP 사용](https://m.blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220650722592&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

