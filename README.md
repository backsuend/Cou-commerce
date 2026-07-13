# Cou-commerce

협업을 통해 이커머스 백엔드의 장바구니, 주문, 결제, 인증·인가와 운영 환경을 구현한 팀 프로젝트입니다.

## 로컬 실행 전 필수 환경 변수

개발 프로필은 저장소에 비밀번호나 JWT 서명 키를 저장하지 않습니다. 실행 전에 다음 환경 변수를 설정해야 합니다.

```bash
export SPRING_DATASOURCE_URL='jdbc:mysql://localhost:3306/coucommercedb?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Seoul&characterEncoding=UTF-8'
export SPRING_DATASOURCE_USERNAME='app'
export SPRING_DATASOURCE_PASSWORD='<로컬 데이터베이스 비밀번호>'
export JWT_SECRET="$(openssl rand -base64 64)"
```

MySQL과 Redis의 실행 절차는 `infra/README.md`를 참고합니다. 실제 비밀번호와 서명 키가 포함된 `.env` 또는 개인 설정 파일은 커밋하지 않습니다.

## 테스트

테스트 프로필도 데이터베이스 비밀번호를 환경 변수로 받습니다.

```bash
SPRING_PROFILES_ACTIVE=test \
SPRING_DATASOURCE_PASSWORD='<로컬 테스트 데이터베이스 비밀번호>' \
./gradlew test
```

GitHub Actions는 PR마다 임시 데이터베이스 비밀번호를 생성해 사용합니다.
