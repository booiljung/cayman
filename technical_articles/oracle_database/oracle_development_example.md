#  Oracle 개발용 준비

## 조회

DBMS에 접속

```sh
sqlplus /as sysdba
```

사용자 보기

```sql
SHOW USER
```

데이터 파일 목록 조회

```sql
SELECT * FROM DBA_DATA_FILES;
SELECT FILE_NAME, BYTES, STATUS FROM DBA_DATA_FILES;
```

테이블 스페이스 조회

```sql
SELECT * FROM DBA_TABLESPACES;
SELECT TABLESPACE_NAME, STATUS, CONTENTS FROM DBA_TABLESPACES;
```

## 테이블 스페이스

테이블스페이스 생성

```sql
CREATE TABLESPACE <tablespace_name>
DATAFILE <dbf_data_file_path>
SIZE 200m;
```

자동으로 확장하도록 지정.

```sql
CREATE <tablespace_name>
DATAFILE <dbf_data_file_path>
SIZE <dbf_data_file_size>
AUTOEXTEND ON;
```

자동으로 확장 + 확장 크기

```sql
CREATE TABLESPACE <tablespace_name>
DATAFILE <dbf_data_file_path>
SIZE <default_size> AUTOEXTEND ON NEXT <extend_size>;
```

임시 테이블 스페이스 생성 + 자동 확장 + 확장 크기

```
CREATE TEMPORARY TABLESPACE TEMP테이블스페이스명
DATAFILE <dbf_data_file_path>
SIZE <default_size> AUTOEXTEND ON NEXT <extend_size>;
```

디테일하게 파일 확장 지정

```sql
CREATE TABLESPACE <tablespace_name>
DATAFILE <dbf_data_file_path>
SIZE <dbf_data_file_size>
DEFAULT STORAGE(
    INITIAL		80k
    NEXT		80k
    MINEXTENTS	1
    MAXEXTENTS	121
    PCTINCREASE	80
) ONLINE;
```

## 사용자

사용자 생성

```
CREATE USER <username>
IDENTIFIED BY <password>;
```

사용자 생성하고 테이블스페이스 권한 부여

```
CERATE USER <username>
IDENTIFIED BY <password>
DEFAULT TABLESPACE <tablespace_name>;
```

사용자 생성하고 테이블스페이스 부여하고 임시 테이블 스페이스 부여 (***)

```sql
CREATE USER <username>
IDENTIFIED <password>
DEFAULT TABLESPACE <tablespace_name>
TEMPORARY TABLESPACE <temp_tablespace_name>;
```

생성한 사용자에게 권한 부여하고 연결 

```
GRANT RESOURCE, CONNECT TO <username>;
GRANT DBA TO <username>;
GRANT CONNECT, RESOURCE, CREATE VIEW, CREATE PROCEDURE, CREATE SEQUENCE TO <username>;
```

## 관리

테이블스페이스 온라인 오프라인 변경

```
ALTER TABLESPACE <tablespace_name> OFFLINE;
ALTER TABLESPACE <tablespace_name> ONLINE;
```

생성된 테이블스페이스의 추가 및  크기 지정

```sql
ALTER TABLESPACE <tablespace_name>
ADD DATAFILE <dbf_data_file_path>
SIZE 100m;
```

생성된 테이블스페이스 크기 변경

```sql
ALTER DATABASE DATAFILE <dbf_data_file_path>
RESIZE 200M;
```

기존 테이블스페이스에 자동확장 변경하기

```sql
ALTER DATABASE DATAFILE <dbf_data_file_path>
AUTOEXTEND ON NEXT 10m
MAXSIZE 100m;
```

테이블스페이스 삭제

```sql
DROP TABLESPACE <tablespace_name>
INCLUDING CONTENTS
CASCADE CONSTRAINTS;
```

데이타 파일까지 삭제

```sql
DROP TABLESPACE <tablespace_name>
INCLUDING CONTENTS AND DATAFILES
CASCADE CONSTRAINTS;
```