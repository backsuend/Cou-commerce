# Cou-Commerce

고객 요구사항 기반 이커머스 백엔드 개발 전주기 협업 프로젝트입니다.

이 저장소는 학습/포트폴리오 목적의 팀 프로젝트이며, 요구사항 정의부터 설계, 개발, 테스트, CI, 운영 관측성 구성까지 백엔드 개발 흐름을 경험하기 위해 제작했습니다.

## 프로젝트 정보

| 항목 | 내용 |
|---|---|
| 프로젝트명 | Cou-Commerce |
| 팀명 | 백수엔드 |
| 프로젝트 유형 | 팀 협업 백엔드 프로젝트 |
| 주요 도메인 | 이커머스, 장바구니, 주문, 결제, 인증/인가 |
| 기준 브랜치 | `develop` |
| 포트폴리오 문서 브랜치 | `docs/cou-commerce-portfolio` |

## 주요 기능

- Redis Hash 기반 Cart API
- Cart → Order → Payment 구매 흐름
- Mock Payment 승인/실패 처리
- 주문 취소, 배송, 완료, 환불 요청 흐름
- Spring Security + JWT 기반 인증 구조
- BUYER / SELLER / ADMIN Role 기반 접근 제어
- Redis 기반 Refresh Token 저장, 로테이션, 재사용 감지
- GitHub Actions 기반 CI
- MDC 기반 요청 추적 및 도메인 이벤트 로깅
- Actuator / Prometheus / Grafana 모니터링 수집 환경

## 기술 스택

- Java 24
- Spring Boot 3.5.5
- Spring Security
- Spring Data JPA
- MySQL
- Redis
- JWT / JJWT
- Swagger / Springdoc OpenAPI
- GitHub Actions
- Docker Compose
- Checkstyle
- Actuator
- Micrometer
- Prometheus
- Grafana
- Logstash / Elasticsearch

## 로컬 실행 개요

인프라는 `infra/` 디렉터리의 Docker Compose 파일을 기준으로 구성했습니다.

```bash
# MySQL / Redis 실행
./infra/scripts/start.sh

# 애플리케이션 실행
./gradlew bootRun

# 테스트 실행
./gradlew test

# 인프라 종료
./infra/scripts/stop.sh
```

## CI

GitHub Actions에서 PR이 `main` 또는 `develop`을 대상으로 열릴 때 CI가 실행됩니다.

CI는 다음 흐름으로 동작합니다.

```text
Checkout
→ Docker network 생성
→ MySQL 컨테이너 실행
→ Redis 컨테이너 실행
→ JDK 24 설정
→ Gradle build/test
→ Checkstyle/Test Report artifact 업로드
→ MySQL/Redis 종료
```

## 포트폴리오 문서

개인 기여 중심의 상세 정리는 아래 문서를 참고하세요.

- [PORTFOLIO.md](./PORTFOLIO.md)
- [docs/repo-notes.md](./docs/repo-notes.md)

## 브랜치 메모

포트폴리오 문서는 `develop` 브랜치를 기준으로 작성했습니다.

보조 참고 브랜치:

- `feature/70-Cart-Order-Payment-loging`
- `feature/74-elk-prometheus-grafana`

위 브랜치에는 로깅/모니터링 관련 추가 작업 흔적이 남아 있으나, 최종 포트폴리오 설명은 `develop` 기준으로 확인 가능한 범위만 사용했습니다.

## 참고

이 프로젝트는 학습/포트폴리오용 팀 프로젝트입니다. 일부 설정값과 테스트 자산은 운영 서비스 기준이 아니라 프로젝트 실습과 협업 검증을 위해 남아 있을 수 있습니다.
