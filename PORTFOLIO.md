# Cou-Commerce - 이커머스 백엔드 개발 전주기 프로젝트

> 팀 협업 프로젝트에서 담당한 백엔드 기여 내용을 정리한 포트폴리오 문서입니다.  
> 기준 브랜치: `develop`  
> 보조 참고 브랜치: `feature/70-Cart-Order-Payment-loging`, `feature/74-elk-prometheus-grafana`

## 1. 프로젝트 개요

Cou-Commerce는 고객 요구사항을 기반으로 이커머스 백엔드의 요구사항 정의, 설계, 개발, 테스트, CI, 운영 관측성 구성을 경험한 팀 협업 프로젝트입니다.

- 기간: 2025.08 - 2025.09
- 역할: 백엔드 개발자 / 팀장
- 팀명: 백수엔드
- 주요 담당: Cart, Order, Payment, Auth/Security, CI, Logging/Monitoring
- Repository: https://github.com/backsuend/Cou-commerce

## 2. 담당 역할 요약

| 영역 | 담당 내용 |
|---|---|
| 프로젝트 리딩 | 요구사항 정의, 기능 분해, GitHub Issue/PR 기반 작업 관리, 발표 산출물 정리 |
| Cart | Redis Hash 기반 장바구니 API, TTL 정책, 총액 계산, DTO 검증, 에러 응답 표준화 |
| Order | 장바구니 기반 주문 생성, 가격/재고 재검증, 셀러별 주문 분할, 주문 취소/배송/완료 상태 전이 |
| Payment | Mock 결제 승인/실패, 중복 결제 방지, 결제 금액 검증, 환불 요청 처리 |
| Auth/Security | Spring Security Filter Chain, JWT 인증 필터, BUYER/SELLER/ADMIN RBAC, Redis 기반 Refresh Token 관리 |
| CI | GitHub Actions에서 MySQL/Redis 컨테이너 기동 후 Gradle build/test 수행 |
| Observability | MDC 기반 요청 추적, 도메인 이벤트 로그, Actuator/Prometheus/Grafana 수집 환경 구성 |

## 3. 주요 구현 내용

### 3.1 Redis 기반 Cart API

Redis Hash 기반으로 사용자별 장바구니를 관리했습니다.

- `cart:{memberId}` 키 기반 사용자별 장바구니 관리
- 동일 상품 추가 시 수량 가산
- 수량 0 이하 수정 시 삭제 처리
- TTL 30일 갱신
- 장바구니 총액 계산
- DTO 검증 및 바인딩 오류 응답 표준화
- BUYER Role 기반 Cart API 접근 제어
- 단위 테스트 작성

관련 작업:

- Issue #18: https://github.com/backsuend/Cou-commerce/issues/18
- Issue #57: https://github.com/backsuend/Cou-commerce/issues/57
- Issue #65: https://github.com/backsuend/Cou-commerce/issues/65
- PR #36: https://github.com/backsuend/Cou-commerce/pull/36
- PR #72: https://github.com/backsuend/Cou-commerce/pull/72

### 3.2 Cart → Order → Payment 구매 흐름

장바구니에서 주문을 생성하고, 결제로 이어지는 이커머스 핵심 흐름을 구현했습니다.

- Cart 조회 후 주문 생성
- Product DB 기준 가격/재고 재검증
- 셀러별 주문 분할
- 주문 상품 가격 스냅샷 저장
- 주문 생성 후 장바구니 초기화
- 주문 취소 시 재고 복구
- 배송/완료 상태 전이
- Mock Payment 승인/실패 처리
- 결제 성공 시 Payment와 Order 상태 동기화
- 중복 결제 방지, 결제 금액 검증, 환불 요청 처리

관련 작업:

- Issue #32: https://github.com/backsuend/Cou-commerce/issues/32
- Issue #33: https://github.com/backsuend/Cou-commerce/issues/33
- Issue #53: https://github.com/backsuend/Cou-commerce/issues/53
- Issue #68: https://github.com/backsuend/Cou-commerce/issues/68
- PR #55: https://github.com/backsuend/Cou-commerce/pull/55
- PR #73: https://github.com/backsuend/Cou-commerce/pull/73

### 3.3 JWT / RBAC 인증 구조

Spring Security 기반으로 JWT 인증 구조와 Role 기반 접근 제어를 구현했습니다.

- Stateless Security 설정
- JWT 인증 필터 등록
- Access Token / Refresh Token 발급
- BUYER / SELLER / ADMIN Role을 `GrantedAuthority`로 매핑
- Method Security 기반 API 접근 제어
- Redis 기반 Refresh Token 저장
- Refresh Token 로테이션
- Refresh Token 재사용 감지 및 사용자 전체 토큰 무효화

관련 코드:

- `SecurityConfig.java`
- `JwtAuthenticationFilter.java`
- `JwtProvider.java`
- `AuthService.java`
- `RefreshTokenService.java`
- `UserDetailsImpl.java`

### 3.4 CI / 협업 기준

GitHub Issue/PR 기반 협업 흐름과 GitHub Actions CI를 구성했습니다.

- Feature / Bug / Refactor 이슈 템플릿 구성
- 기능 브랜치 기반 작업 관리
- PR 체크리스트 기반 리뷰 흐름
- MySQL / Redis 컨테이너 기동 후 Gradle build/test 수행
- Checkstyle / Test Report artifact 수집

관련 파일:

- `.github/workflows/ci.yml`
- `.github/ISSUE_TEMPLATE/feature_request.md`
- `.github/ISSUE_TEMPLATE/bug_report.md`
- `.github/ISSUE_TEMPLATE/refactor.md`

### 3.5 로깅 / 모니터링

운영 관점에서 요청 추적과 도메인 이벤트 로그, 모니터링 수집 환경을 구성했습니다.

- 요청 단위 `traceId` 생성
- MDC 기반 `memberId`, `memberRole`, `http.method`, `http.path`, `http.status`, `latency_ms` 기록
- Cart / Order / Payment 도메인별 LogContext 분리
- JSON 로그 출력
- 보안 로그와 애플리케이션 로그 분리
- Logstash 수집 설정
- Actuator / Prometheus / Grafana 수집 환경 구성

관련 작업:

- Issue #70: https://github.com/backsuend/Cou-commerce/issues/70
- PR #81: https://github.com/backsuend/Cou-commerce/pull/81

관련 파일:

- `LoggingFilter.java`
- `CartLogContext.java`
- `OrderLogContext.java`
- `PaymentLogContext.java`
- `logback-spring.xml`
- `infra/monitoring/docker-compose.yml`
- `infra/monitoring/prometheus.yml`
- `infra/elk/logstash/pipeline/logstash.conf`

## 4. 기술 스택

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

## 5. 이 프로젝트에서 보여줄 수 있는 역량

- 팀 협업 프로젝트에서 요구사항을 기능 단위로 분해하고 GitHub Issue/PR 흐름으로 관리한 경험
- Redis 기반 Cart와 주문/결제 상태 전이를 포함한 이커머스 핵심 도메인 구현 경험
- JWT/RBAC 기반 인증·인가 구조와 Refresh Token 로테이션 설계 경험
- CI, 테스트, 로깅, 모니터링까지 포함한 백엔드 개발 전주기 경험

## 6. 면접 설명용 요약

Cou-Commerce는 고객 요구사항 기반으로 진행한 이커머스 백엔드 협업 프로젝트입니다. 팀장 역할로 요구사항 정의와 기능 분해, GitHub Issue/PR 기반 작업 흐름을 정리했고, 백엔드에서는 Redis 기반 Cart, Cart → Order → Payment 구매 흐름, JWT/RBAC 인증 구조, GitHub Actions CI, MDC 기반 로깅과 Prometheus/Grafana 모니터링 환경 구성을 담당했습니다.
