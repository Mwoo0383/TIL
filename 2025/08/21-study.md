# Room91 CI/CD & 배포 환경 정리 (Staging/Prod)

## 0) 브랜치 전략 & 전체 흐름
- 작업: `feature/*` → **PR** → `develop` 머지 ⇒ **Staging 배포**
- 검증 후: **PR** → `main` 머지 ⇒ **Production 배포**(환경 승인자 권장)

---

## 1) CI (PR 검증) — `.github/workflows/ci.yml`
- 트리거: `pull_request` (대상: `main`, `develop`)
- 잡 분리:
  - **backend**: JDK 17, `./BuDongSan/gradlew clean build` (필요 시 `-x test`)
  - **frontend**: Node 20, `npm ci` → lint/test(옵션) → build  
    - PR 비밀값 제한 대응: **더미 `.env.production`** 생성
- 문제/해결:
  - Spring `@SpringBootTest`가 외부 인프라 의존으로 실패 → 임시 `-x test` 또는 `@Disabled`
  - ESLint v9 기본설정 제거로 실패 → **`eslint.config.mjs`(Flat Config)** 추가
  - 설치 속도: `npm ci --no-audit --no-fund --prefer-offline`로 최적화

**이유**: PR 단계에서 배포 없이 **빌드 가능성**만 보장, 백/프론트 분리로 실패 지점 명확화.

---

## 2) Staging 배포 — `.github/workflows/deploy-staging.yml`
- 트리거: **`push` to `develop`** + `workflow_dispatch`
- 주요 단계:
  1. **Gradle** 빌드(JAR 산출), **Vite** 빌드(dist 산출)
  2. 프런트 `.env.production`(stg) 생성:  
     `VITE_API_BASE_URL_STG`, `KAKAO_JS_API_KEY_STG`
  3. 산출물 업로드(SCP): `frontend_output/*`, `.env`(ENV_STAGING), `*.jar`
  4. **compose/nginx 설정 업로드**  
     - `source: "docker-compose.staging.yml,nginx/server-staging.conf"` ← **콤마 한 줄** (빈 아카이브 방지)
  5. 원격에서 디렉터리 준비:  
     `/home/ubuntu/Room91-staging/{frontend,images,news_videos,nginx}`
  6. **Docker Compose 재기동**  
     - `docker compose -f docker-compose.staging.yml down || true`  
     - `docker compose -f docker-compose.staging.yml up -d --build`
- 디버그 스텝 추가: `Verify staging files exist` (체크아웃 파일 존재 확인)

**이유**: develop에 머지될 때만 리허설 배포가 자동으로 일어나도록 분리.  
**해결한 에러**:
- `Can't find a suitable configuration file` → `-f docker-compose.staging.yml`로 지정
- `tar: empty archive` → `scp-action source`를 **콤마 한 줄**로 수정
- 키 혼용 → 시크릿을 **`STG_EC2_*`로 통일**

---

## 3) Production 배포 — `.github/workflows/deploy-prod.yml`
- 트리거: **`push` to `main`** + `workflow_dispatch`
- 흐름은 staging과 동일하되 `ENV_PROD`, prod 도메인 사용
- 권장: GitHub **Environment `production`**에 **Required reviewers**로 승인 게이트

---

## 4) 시크릿(Secrets) 구성
- **Staging**: `STG_EC2_HOST`, `STG_EC2_USER`, `STG_EC2_KEY`, `VITE_API_BASE_URL_STG`, `KAKAO_JS_API_KEY_STG`, **`ENV_STAGING`(멀티라인 .env)**
- **Production**: `EC2_HOST`, `EC2_USER`, `EC2_KEY`, `VITE_API_BASE_URL`, `KAKAO_JS_API_KEY`, **`ENV_PROD`**

**이유**: 환경별 격리 및 안전한 자격증명 관리.

---

## 5) Docker/Compose
### 5-1) `docker-compose.staging.yml`
- 컨테이너/네트워크/볼륨 **접미사 `-stg`**로 분리
- 포트 분리: `app 8081:8080`, `nginx 8082:80`, `postgres 5433`, `mongo 27019`, `redis 6385`
- `nginx` 마운트: `./nginx/server-staging.conf:/etc/nginx/conf.d/default.conf:ro`

### 5-2) 서버 경로
- Staging: `/home/ubuntu/Room91-staging/`
- Production: `/home/ubuntu/Room91/`

**이유**: prod와 **충돌/데이터 섞임 방지**, 병행 운영 가능.

---

## 6) Nginx
### 6-1) Staging — `nginx/server-staging.conf` (HTTP/8082)
```nginx
server {
  listen 80;
  server_name _;
  root /var/www/html; index index.html;
  location /api/      { proxy_pass http://Housing-app-stg:8080; }
  location /uploads/  { proxy_pass http://Housing-app-stg:8080; }
  location ~*\.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ { try_files $uri =404; }
  location / { try_files $uri /index.html; }
}
