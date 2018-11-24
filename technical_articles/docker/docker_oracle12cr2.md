# Ubuntu에 Oracle 12c 설치

## [sath89/oracle-12c](https://hub.docker.com/r/sath89/oracle-12c/)

oracle 12c release 1 도커 이미지

### [Oracle Database 12c now available on Docker](https://sqlmaria.com/2017/04/27/oracle-database-12c-now-available-on-docker/)

---

## [Oracle Database Server Docker Image Documentation](https://store.docker.com/profiles/booiljung/content/sub-5212681b-73f5-4a5f-bb84-f018cf42567e)

## Oracke Database Server Docker Image의 특징

### 제한사항

- 단일 인스턴스 데이터베이스를 지원합니다.
-  Dataguard는 지원되지 않습니다.
- 데이터베이스 옵션 및 패치는 지원되지 않습니다.

### 리소스 요구 사항

컨테이너의 최소 요구 사항은 8GB의 디스크 공간과 2GB의 메모리입니다. 

## 설치 방법

### Docker store에 가입

1. 먼저 [Docker store](https://store.docker.com/)에 가입합니다.

2. [제품 페이지](https://store.docker.com/images/oracle-database-enterprise-edition)에서 'Process to Checkout' 버튼을 눌러서 개인정보를 제공하고 사용 허가를 취득해야 합니다.

### Docker Container 실행

먼저 도커 스토어에 로그인 합니다.

```sh
sudo docker login
```

아래와 같이 오라클 데이터베이스를 실행할 수 있습니다.

```sh
sudo docker run -d -it --name <Oracle-DB> store/oracle/database-enterprise:12.2.0.1
```

여기서, `<Oracle-DB>`는 컨테이너 이름이며 `12.2.0.1`은 도커 이미지 태그입니다. 

아래와 같이 데이터베이스 컨테이너의 상태를 체크 할 수 있습니다.

```sh
sudo docker ps -a
```

### 데이터베이스 서버 컨테이너에 접속

 `sys` 유저의 기본 비밀번호 `Oradoc_db1` 로 데이터베이스에 접속 할 수 있습니다. 다음은 컨테이너 내부의 sqlplus 앱을 사용하여 데이터베이스 서버에 접속 합니다.

```sh
sudo docker exec -it <oracle_db_container_name> bash -c "source /home/oracle/.bashrc; sqlplus /nolog"
```

### 컨테이너 밖에서 접속

데이터베이스 서버는 SQLNet 프로토콜을 통한 Oracle 클라이언트 연결의 경우 포트 1521을, Oracle XML DB의 경우 포트 5500을 노출합니다. SQLPlus 또는 모든 JDBC 클라이언트를 사용하여 컨테이너 바깥 쪽에서 데이터베이스 서버에 연결할 수 있습니다. 컨테이너 외부에서 연결하려면 -P 또는 -p 옵션을 사용하여 컨테이너를 시작합니다. 

```sh
sudo docker run -d -it --name <oracle_db_container_name> -P store/oracle/database-enterprise:12.2.0.1
```

옵션 `-P`는 포트가 Docker에 의해 할당되었음을 나타냅니다. 매핑 된 포트는 다음을 실행하여 발견 할 수 있습니다.

```sh
sudo docker port <oracle_db_container_name> 1521/tcp -> 0.0.0.0:<mapped host port>
```

여기서 `<mapped_host_port>`와 `<ip_address_of_host>`를 사용하여 환경 변수 `TNS_ADMIN`이 가리키는 디렉토리에 `tnsnames.ora`를 작성하면 됩니다.

```tns
ORCLCDB=(
	DESCRIPTION=(
		ADDRESS=(PROTOCOL=TCP)(HOST=<ip_address_of_host>)(PORT=<mapped_host_port>)
	)(
		CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=ORCLCDB.localdomain)
	)
)
ORCLPDB1=(
	DESCRIPTION=(
		ADDRESS=(PROTOCOL=TCP)(HOST=<ip_address_of_host>)(PORT=<mapped_host_port>)
	)(
		CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=ORCLPDB1.localdomain)
	)
)
```

다음은 컨테이너 밖에서 sql*plus를 통해 데이터베이스에 접속합니다.

```sh
sqlplus sys/Oradoc_db1@ORCLCDB as sysdba
```

## 환경변수

Oracle 데이터베이스 컨테이너는 컨테이너를 시작시 지정할 수 있는 사용자 정의 구성 파라미터를 제공하며 모두 선택 사항입니다. 사용자 지정 구성 파라미터 목록은 `ora.conf`로 지정 할 수 도 있습니다.

|     ENV     | 설명                                                         |
| :---------: | ------------------------------------------------------------ |
|  `DB_SID`   | 데이터베이스의 `ORACLE_SID`를 변경합니다. 기본값은 `ORCLCDB`  입니다. |
|  `DB_PDB`   | PDB의 이름을 수정합니다. 기본값는 `ORCLPDB1` 입니다.         |
| `DB_MEMORY` | 오라클 서버에 필요한 메모리 요구량을 설정합니다. 이 값은 `SGA` 및 `PGA`에 할당 할 메모리 양을 결정합니다. 기본값은 2GB로 설정됩니다. |
| `DB_DOMAIN` | 데이터베이스 서버에 사용할 도메인을 설정합니다. 기본값은`localdomain`입니다. |

다음은 사용자 정의 구성 매개 변수로 Oracle 데이터베이스 서버를 시작하려면

```sh
sudo docker run -d -it --name <oracle_db_container_name> -P --env-file ora.conf store/oracle/database-enterprise:12.2.0.1
```

`tnsnames.ora`에서 `DB_SID`, `DB_PDB` 및 `DB_DOMAIN`에 대한 사용자 정의 값이 업데이트되었는지 확인합니다.

### `sys` 사용자의 기본 암호 변경

Oracle 데이터베이스 기본 암호는 `Oradoc_db1`로 시작됩니다. 컨테이너 생성시 사용 된 암호는 안전하지 않으므로 변경해야합니다. 암호를 변경하려면 SQL*Plus를 사용하여 데이터베이스에 연결하고 다음 명령을 실행합니다.

```sql
alter user sys identified by <new_password>;
```

### 로그 조회

```sh
sudo docker logs <oracle_db_container_name>
```

### 기존 데이터베이스 재사용

이 Oracle 데이터베이스 서버 이미지는 Docker 데이터 볼륨을 사용하여 데이터 파일, 다시 실행 로그, 감사 로그, 경고 로그 및 추적 파일을 저장합니다. 데이터 볼륨은 컨테이너 내부의 `/ORCL` 안에 마운트됩니다. `docker run` 명령을 사용하여 데이터 볼륨으로 데이터베이스를 시작하려면,

```sh
sudo docker run -d -it --name <oracle_db_container_name> -v <oracle_db_data>:/ORCL store/oracle/database-enterprise:12.2.0.1
```

여기서, `<oracle_db_data>`는 Docker에 의해 작성되고 `/ORCL`에있는 컨테이너 내부에 마운트 된 데이터 볼륨입니다. 지속 된 데이터 파일은 `<oracle_db_data>`데이터 볼륨을 재사용하여 다른 컨테이너와 재사용 할 수 있습니다.

### 데이터 볼륨에 호스트 시스템 디렉토리 사용

데이터 볼륨에 대한 호스트 시스템의 디렉토리를 사용하려면,

```sh
sudo docker run -d -it --name <oracle_db_container_name> -v /data/OracleDBData:/ORCL store/oracle/database-enterprise:12.2.0.1
```

여기서 `/data/OracleDBData`는 호스트 시스템의 디렉토리입니다.

### Oracle Database Server 12.2.0.1 엔터프라이즈 에디션 Slim Variant

EE의 Slim Variant (12.2.0.1-slim 태그)는 디스크 공간 (4GB) 요구 사항을 줄이고 컨테이너를 더 빨리 시작합니다. 이  이미지는 Analytics, Oracle R, Oracle Label Security, Oracle Text, Oracle  Application Express 및 Oracle DataVault와 같은 기능을 지원하지 않습니다. 슬림 변형을 사용하려면

```sh
sudo docker run -d -it --name <oracle_db_container_name> store/Oracle/database-enterprise:12.2.0.1-slim
```

여기서 `<oracle_db_container_name>`는 컨테이너의 이름이고 `12.2.0.1-slim`은 Docker 이미지 태그입니다.  

##### 참조

- [sath89/oracle-12c](https://hub.docker.com/r/sath89/oracle-12c/)
- [oracle-12c를 docker를 이용하여 구동하기 ](http://hellogohn.com/post_one261)

- [Oracle Database Server Docker Image Documentation](https://store.docker.com/profiles/booiljung/content/sub-5212681b-73f5-4a5f-bb84-f018cf42567e)

- [Docker를 기반으로 다양한 데이터베이스 환경 통합하기](https://medium.com/chequer/docker%EB%A5%BC-%EA%B8%B0%EB%B0%98%EC%9C%BC%EB%A1%9C-%EB%8B%A4%EC%96%91%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%99%98%EA%B2%BD-%ED%86%B5%ED%95%A9%ED%95%98%EA%B8%B0-96aa68363775)

- [Oracle Database 12c now available on Docker](https://sqlmaria.com/2017/04/27/oracle-database-12c-now-available-on-docker/)

