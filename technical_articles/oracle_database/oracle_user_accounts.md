# 오라클 사용자 계정

### SYS:

DBA role을 가지고 있으며, data dictionary table의 소유자.

### SYSTEM:

DBA role을 가지고 있으며, data dictionary view의 소유자.

### Oracle 접속

```sh
sqlplus '/as sysdba'
```

### username 목록

```sql
SELECT * FROM DBA_USERS;
SELECT * FROM ALL_USERS;	
```

### username의 system privilege 확인

```sql
SELECT * FROM DBA_SYS_PRIVX WHERE GRANTEE='<username>';
```

### username의 role 확인

```sql
SELECT * FROM DBA_ROLE_PRIVS WHERE GRANTEE='<username>';
```

### role이 부여된 username 확인

```sql
SELECT * FROM DBA_SYS_PRIVS WHERE GRANTEE='<role>'
```

### 소유 기준으로 username에 부여된 객체 privilege 확인

```sql
SELECT * FROM DBA_TAB_PRIVS WHERE OWNER='<username>';
```

### 부여 기준으로 username에 부여된 객체 privilege 확인

```sql
SELECT * FROM DBA_TAB_PRIVS WHERE GRANTEE='<username>';
```

### 사용자가 소유한 테이블 목록

```sql
SELECT <table_name> FROM USER_TABLES;
```

### 사용자 생성

```sql
CREATE USER '<username>'
IDENTIFIED BY '<password>';
```

### 사용자 제거

```sql
DROP USER <username> CASCADE;
```

### 사용자 비밀번호 변경

```sql
ALTER USER <username> IDENTIFIED BY '<new_password>';
```

### 사용자 privilege 부여 및 회수

```sql
GRANT CREATE <privilege> TO '<username>';
REVOKE <privilege> ON <table_name> FROM '<username>';
```

- SYSDBA: 최고 관리자.
- SYSOPER : 최고 운영자.
- CREATE USER: 사용자 생성.
- CREATE TABLE: 테이블 생성.
- CREATE SEQUENCE: 시퀀스 생성.
- CREATE ANY TABLE: 모든 사용자의 테이블 생성.
- CREATE SESSION: 접속.
- CREATE VIEW: 뷰 생성.
- CREATE MATERIALIZED VIEW:
- CREATE PROCEDURE: 프로시저 생성.
- CREATE PUBLIC SYNONYM:
- CREATE TRIGGER:
- CREATE TYPE:
- SELECT ANY TABLE: 모든 사용자의 테이블 조회.
- DROP ANY TABLE:

### 사용자 테이블 스페이스 생성

```sql
CREATE TABLESPACE <tablespace_name> DATAFILE '<dbf_file_path>';
```

### 사용자 생성 및 테이블스페이스 부여

```sql
CREATE USER <username>
IDENTIFIED BY <password>
DEFAULT TABLESPACE <tablespace_name>
TEMPORARY TABLESPACE TEMP;
```

### 새 사용자에게 모든 권한 주기

```sql
GRANT CONNECT, DBA, RESOURCE TO '<username>';
```

