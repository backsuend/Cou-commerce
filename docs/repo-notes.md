# Cou-Commerce Repository Notes

> 이 문서는 Cou-Commerce 프로젝트의 브랜치 기준과 구현 범위를 설명합니다.

## 1. 기준 브랜치

문서는 `develop` 브랜치를 기준으로 작성했습니다.

```text
기준 브랜치: develop
문서 브랜치: docs/cou-commerce-portfolio
보조 참고 브랜치:
- feature/70-Cart-Order-Payment-loging
- feature/74-elk-prometheus-grafana
```

## 2. 브랜치 기준

### `develop`

현재 문서의 기준 브랜치입니다.

확인된 주요 내용:

- Cart / Order / Payment 핵심 도메인 구현
- Auth / Security / RBAC 구조
- GitHub Actions CI
- LoggingFilter / MDC 기반 요청 추적
- Actuator / Prometheus / Grafana 설정
- Logstash 수집 설정

### `feature/70-Cart-Order-Payment-loging`

로깅/모니터링 작업의 원본 브랜치 성격이 있습니다.

- PR #81의 작업 브랜치
- develop에 병합된 구현과 일부 차이가 있음
- 로깅 작업 흐름을 확인할 때 보조로 참고 가능

### `feature/74-elk-prometheus-grafana`

ELK / Prometheus / Grafana 관련 추가 작업이 남아 있는 브랜치입니다.

- develop보다 앞선 변경이 일부 존재
- develop보다 뒤처진 커밋도 있어 최종 기준으로 사용하지 않음
- 운영 관측성 관련 추가 작업을 확인할 때 보조로 참고 가능

## 3. 구현 범위 정리

문서와 이슈를 실제 구현 상태에 맞춰 정리했습니다.

정리한 이슈:

- `#70` 로깅/모니터링 구현 범위 정리 및 close
- `#69` 주문 생성 파이프라인 개선 범위와 고도화 항목 분리
- `#65` Cart Buyer API v1 정책 정리
- `#53` Order/Payment 테스트 근거를 단위 테스트 + APIdog/Swagger 검증 중심으로 정리
- `#68` Order/Payment 2차 개발 완료 범위와 운영 고도화 항목 분리

## 4. 정리한 문서

### README

`docs/cou-commerce-portfolio` 브랜치에서 README를 프로젝트 검토용으로 보강했습니다.

추가한 내용:

```text
- 프로젝트 개요
- 주요 기능
- 기술 스택
- 로컬 실행 개요
- CI 설명
- 문서 링크
- 브랜치 기준 메모
```

### PORTFOLIO.md

프로젝트에서 담당한 주요 백엔드 구현과 협업 기여를 정리했습니다.

정리한 내용:

```text
- Cart 도메인
- Order / Payment 구매 흐름
- JWT / RBAC 인증 구조
- CI / 협업 기준
- 로깅 / 모니터링
- 기술 스택
```

## 5. 테스트 자산과 생성 파일 메모

### 업로드 이미지

`src/main/resources/static/uploads/products/` 하위 업로드 이미지는 협업 과정에서 테스트용으로 사용한 자산입니다.

운영 프로젝트라면 S3/MinIO/object storage 등으로 분리하는 것이 적절하지만, 현재 저장소에서는 테스트 자산으로 유지합니다.

### QueryDSL generated QClass

QueryDSL generated QClass가 일부 커밋되어 있습니다.

운영 수준으로 정리한다면 다음 방향이 적절합니다.

```text
- src/main/generated/를 .gitignore 처리
- Gradle build 시 QClass가 생성되도록 유지
```

### 테스트 파일

빈 통합 테스트 파일은 문서 브랜치에서 제거했습니다.

## 6. 운영 전환 시 개선 가능 항목

프로젝트를 실제 운영 서비스 수준으로 확장한다면 아래 항목을 추가로 정리할 수 있습니다.

- 환경별 설정 파일과 secret 환경변수화
- 업로드 파일 외부 스토리지 분리
- Grafana Dashboard JSON / 캡처 정리
- Prometheus alert rule 작성
- QueryDSL generated source 관리 방식 정리
- 테스트 데이터와 seed 데이터 분리

## 7. 검토 순서

코드 검토 시 아래 순서로 보면 구현 의도를 빠르게 확인할 수 있습니다.

1. `PORTFOLIO.md`
2. `src/main/java/com/backsuend/coucommerce/cart/service/CartService.java`
3. `src/main/java/com/backsuend/coucommerce/order/service/OrderService.java`
4. `src/main/java/com/backsuend/coucommerce/payment/service/PaymentService.java`
5. `src/main/java/com/backsuend/coucommerce/auth/service/AuthService.java`
6. `src/main/java/com/backsuend/coucommerce/common/config/SecurityConfig.java`
7. `src/main/java/com/backsuend/coucommerce/common/filter/LoggingFilter.java`
8. `.github/workflows/ci.yml`
