# 로컬 개발 인프라

애플리케이션, MySQL, Redis를 각각 독립된 Compose 스택으로 실행하고 공통 외부 네트워크 `cou-commerce-net`으로 연결합니다.

## 1. 공통 네트워크 생성

```bash
docker network create cou-commerce-net
```

이미 생성된 경우의 오류는 무시해도 됩니다.

## 2. MySQL 환경 변수 준비

```bash
cd infra/mysql
cp .env.example .env
```

복사한 `.env`에서 데이터베이스 사용자 비밀번호와 root 비밀번호를 서로 다른 로컬 전용 값으로 변경합니다. `.env`는 Git 추적 대상이 아니며 커밋하거나 공유하지 않습니다.

## 3. MySQL 실행

```bash
docker compose up -d
```

기본 데이터베이스명과 사용자명은 `.env`에서 변경할 수 있습니다. 애플리케이션에는 동일한 접속 정보를 환경 변수로 전달합니다.

```bash
export SPRING_DATASOURCE_URL='jdbc:mysql://localhost:3306/coucommercedb?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Seoul&characterEncoding=UTF-8'
export SPRING_DATASOURCE_USERNAME='app'
export SPRING_DATASOURCE_PASSWORD='<infra/mysql/.env에 설정한 값>'
```

## 4. Redis 실행

```bash
cd infra/redis
docker compose up -d
```

애플리케이션 컨테이너에서 접근할 때는 Compose 서비스명인 `mysql`, `redis`를 호스트명으로 사용하고, 로컬에서 직접 실행할 때는 `localhost`를 사용합니다.

## 보안 원칙

- 실제 비밀번호, JWT 서명 키, 운영 접속 정보는 저장소에 커밋하지 않습니다.
- 로컬 비밀번호와 운영 비밀번호를 재사용하지 않습니다.
- 노출이 의심되는 값은 파일 삭제보다 먼저 폐기·재발급합니다.
- 예제 파일에는 실제 값이 아니라 교체가 필요한 자리표시자만 둡니다.
