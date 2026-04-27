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

## 3. 레포 관리 메모

이 프로젝트는 메인 서비스 운영 레포라기보다, 백엔드 개발 생애주기와 협업 경험을 보여주는 포트폴리오 성격이 강합니다. 따라서 레포 정리는 과도하게 깊게 하기보다, 채용자가 빠르게 이해할 수 있는 수준의 문서 보강이 우선입니다.

## 4. 정리하면 좋은 항목

### README 보강

현재 README는 프로젝트 설명이 충분하지 않으므로 다음 항목을 추가하면 좋습니다.

```text
- 프로젝트 개요
- 주요 기능
- 기술 스택
- 실행 방법
- 인프라 실행 방법
- 테스트 실행 방법
- CI 설명
- 모니터링 실행 방법
- 개인 기여 문서 링크
```

### 업로드 파일 정리

`src/main/resources/static/uploads/products/` 하위에 실제 업로드 결과물 성격의 이미지 파일이 포함되어 있습니다.

포트폴리오용 프로젝트라 큰 문제로 보지 않더라도, 레포를 깔끔하게 보이게 하려면 다음을 고려할 수 있습니다.

```text
- 업로드 결과물은 .gitignore 처리
- 샘플 이미지는 docs/sample-assets 또는 seed 전용 경로로 분리
```

### Generated QClass 정리

QueryDSL generated QClass가 커밋되어 있습니다.

정리 방향:

```text
- src/main/generated/를 .gitignore 처리
- Gradle build 시 QClass가 생성되도록 유지
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

## 5. 민감 정보 메모

이 프로젝트에는 포트폴리오/학습용 프로젝트라는 맥락에서 설정 파일에 테스트용 값이 남아 있습니다.

다만 실제 운영 프로젝트라면 다음과 같이 관리하는 것이 적절합니다.

```text
- application-prod.yml의 DB password / JWT secret 환경변수화
- application-prod.example.yml 제공
- 공개된 secret은 폐기/재발급
```

현재 포트폴리오 설명에서는 이 부분을 과하게 강조하지 않고, 운영 프로젝트로 전환할 때 개선해야 할 항목 정도로만 관리합니다.

## 6. 추천 우선순위

```text
1. README.md 보강
2. PORTFOLIO.md 링크 추가
3. docs/monitoring.md 작성
4. static uploads / generated QClass / 빈 테스트 파일 정리 여부 결정
5. application example 파일 분리 여부 결정
```
