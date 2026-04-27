# Cou-Commerce Repo Notes

> 이 문서는 Cou-Commerce 레포를 포트폴리오 관점에서 볼 때 참고할 브랜치 기준과 정리 메모를 남기기 위한 문서입니다.

## 1. 기준 브랜치

포트폴리오 문서는 `develop` 브랜치를 기준으로 작성했습니다.

```text
기준 브랜치: develop
포트폴리오 문서 브랜치: docs/cou-commerce-portfolio
보조 참고 브랜치:
- feature/70-Cart-Order-Payment-loging
- feature/74-elk-prometheus-grafana
```

## 2. 브랜치 상태 메모

### develop

현재 포트폴리오 기준 브랜치입니다.

확인된 주요 내용:

- Cart / Order / Payment 핵심 도메인 구현
- Auth / Security / RBAC 구조
- GitHub Actions CI
- LoggingFilter / MDC 기반 요청 추적
- Actuator / Prometheus / Grafana 설정
- Logstash 수집 설정

### feature/70-Cart-Order-Payment-loging

로깅/모니터링 작업의 원본 브랜치 성격이 있습니다.

- PR #81의 작업 브랜치
- develop에 merge된 내용이 있으나, 일부 차이가 남아 있음
- 포트폴리오 기준 브랜치로 쓰기보다는 로깅 작업 확인용으로 참고

### feature/74-elk-prometheus-grafana

ELK / Prometheus / Grafana 관련 추가 작업이 남아 있을 가능성이 있는 브랜치입니다.

- develop보다 ahead 상태인 변경이 많음
- 다만 develop보다 뒤처진 커밋도 있어 최종 기준으로 삼기는 어려움
- 운영 관측성 관련 보조 참고 브랜치로만 사용

## 3. 이슈 정리 상태

포트폴리오 관점에서 실제 구현과 이슈 내용이 어긋나 보일 수 있는 항목은 기존 이슈 본문을 수정해 정리했습니다.

정리한 이슈:

- `#70` 로깅/모니터링 구현 범위 정리 및 close
- `#69` 주문 생성 파이프라인 개선 범위와 고도화 보류 항목 분리
- `#65` Cart Buyer API v1 정책 정리
- `#53` Order/Payment 테스트 근거를 단위 테스트 + APIdog/Swagger 검증 중심으로 정리
- `#68` Order/Payment 2차 개발 완료 범위와 운영 고도화 항목 분리

## 4. 레포 관리 메모

이 프로젝트는 메인 서비스 운영 레포라기보다, 백엔드 개발 생애주기와 협업 경험을 보여주는 포트폴리오 성격이 강합니다. 따라서 레포 정리는 과도하게 깊게 하기보다, 채용자가 빠르게 이해할 수 있는 수준의 문서 보강이 우선입니다.

## 5. 정리한 항목

### README 보강

`docs/cou-commerce-portfolio` 브랜치에서 README를 포트폴리오용으로 보강했습니다.

추가한 내용:

```text
- 프로젝트 개요
- 주요 기능
- 기술 스택
- 로컬 실행 개요
- CI 설명
- 포트폴리오 문서 링크
- 브랜치 기준 메모
```

### 업로드 파일 메모

`src/main/resources/static/uploads/products/` 하위 업로드 이미지는 협업 과정에서 테스트용으로 사용한 자산입니다.

따라서 이번 포트폴리오 브랜치에서는 삭제하지 않습니다.

운영 프로젝트라면 S3/MinIO/object storage 등으로 분리하는 것이 맞지만, 현재 레포에서는 학습/협업 검증용 테스트 자산으로 유지합니다.

### Generated QClass 정리 방향

QueryDSL generated QClass가 커밋되어 있습니다.

정리 방향:

```text
- src/main/generated/를 .gitignore 처리
- Gradle build 시 QClass가 생성되도록 유지
- 포트폴리오 브랜치에서는 필요 시 generated 파일 제거 가능
```

### 빈 테스트 파일 확인

`OrderPaymentCartIntegrationTest.java`는 빈 파일로 확인되었습니다.

정리 방향:

```text
- 실제 통합 테스트 구현
- 또는 빈 파일 제거
- 이력서에서는 단위 테스트 중심으로 표현
```

### 모니터링 문서 보강

Prometheus/Grafana 실행 설정은 있으나, Grafana dashboard JSON이나 캡처가 따로 정리되어 있지는 않습니다.

정리 방향:

```text
- docs/monitoring.md 작성
- Prometheus scrape target 설명
- Grafana 접속 방법 설명
- 주요 metric 목록 정리
- 가능하다면 dashboard export JSON 추가
```

## 6. 민감 정보 메모

이 프로젝트에는 포트폴리오/학습용 프로젝트라는 맥락에서 설정 파일에 테스트용 값이 남아 있습니다.

다만 실제 운영 프로젝트라면 다음과 같이 관리하는 것이 적절합니다.

```text
- application-prod.yml의 DB password / JWT secret 환경변수화
- application-prod.example.yml 제공
- 공개된 secret은 폐기/재발급
```

현재 포트폴리오 설명에서는 이 부분을 과하게 강조하지 않고, 운영 프로젝트로 전환할 때 개선해야 할 항목 정도로만 관리합니다.

## 7. 추천 우선순위

```text
1. README.md 보강 완료
2. PORTFOLIO.md 추가 완료
3. 기존 이슈 상태 정리 완료
4. docs/monitoring.md 작성 검토
5. generated QClass / 빈 테스트 파일 정리 여부 결정
6. application example 파일 분리 여부 결정
```
