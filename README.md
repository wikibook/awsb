# sbcntr-backend 

AWS 실습용 백엔드 API용 리포지토리다.


## 概要
echo 프레임워크를 이용한 Go 언어 API 서버다.
Go 언어에는 다양한 프레임워크가 있다.
REST API 서버를 구현하기 위해 충분한 기능을 가졌으며 관련 정보도 많은 echo 를 이용한다.

API 서버와 DB(MySQL)의 접속은 Go의 ORM 중 하나인 GORM[^gorm]을 이용한다.

[^gorm]: https://gorm.io/

백엔드 애플리케이션은 다음 2가지 서비스를 제공한다.
각 API 엔드포인트의 집속 접두사는 `/v1`이다.

- Item서비스
  - DB 접속 없이 화면 표시를 하기 위해 Helloworld를 반환한다(/helloworld)
  - Item테이블에 등록된 데이터를 반환한다(/Item)
  - 프론트엔드에서 입력된 정보를 이용해 Item을 등록한다(/Item)
  - Item에 찜 마크를 붙이거나 제거한다(/Item/favorite)
- Notifiation서비스
  - Notification테이블에 등록된 데이터를 반환한다(/Notifications)
    - 질의 매개변수로 id를 이용해 특정 데이터만 반환한다
  - 알림 뱃지를 표시하기 위해 읽기 않은 알림 수를 반환한다(/Notifiactions/Count)
  - 읽지 않은 알림을 일괄 읽음으로 변경한다(/Notifications/Read)

## 이용 범위
이 책의 내용에 따라 이용해주세요.

## 로컬에서 이용하는 방법

### 사전 준비
- Go 버전은 16.x를 이용한다
- GOPATH에 따라 적절한 디렉토리에 이 리포지토리를 복사한다
- 다음 명령어를 이용해 모듈을 다운로드 한다

```bash
$ go get golang.org/x/lint/golint
$ go install
$ go mod download
```

- 이 백엔드는 DB와의 접속이 필요하다. DB 접속을 위해 다음 환경 변수를 지정한다
  - DB_HOST
  - DB_USERNAME 
  - DB_PASSWORD 
  - DB_NAME

### DB 준비

로컬에 MySQL 서버를 설치한다

https://dev.mysql.com/downloads/mysql/

### 빌드 및 배포

#### 로컬에서 실행하는 경우

```bash
❯ make all
```

#### Docker를 이용하는 경우

```bash
❯ docker build -t sbcntr-backend:latest .
❯ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
sbcntr-backend                   latest              cdb20b70f267        58 minutes ago      4.45MB
:
❯ docker run -d -p 80:80 sbcntr-backend:latest
```

### 배포 후 통신 확인

```bash
❯ curl http://localhost:80/v1/helloworld
{"data":"Hello world"}

❯ curl http://localhost:80/healthcheck
null
```

## 주의사항
- 로컬 환경은 macOS Bigsur 11.6 및 Monterey 12.1 에서만 테스트됐음
