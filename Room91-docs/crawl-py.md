# 🕷️ crawl.py 코드 흐름 정리

## 1. 네이버 부동산 매물 데이터 크롤링

### ✅ 공통 쿠키 & 헤더 설정  
- **목적**: 네이버 부동산 서버에 **정상 사용자처럼 보이기 위함**  
- **설정 방법**:  
  1. 네이버 부동산 접속  
  2. `개발자 도구` → `Network 탭`  
  3. 아무 요청 클릭 → `Request Headers` 확인  
  4. 필요한 값들을 딕셔너리 형태로 코드에 삽입  

---

### ✅ 요청 파라미터 구성  
- **기본 요청 URL**:  
  `https://new.land.naver.com/api/articles?...`

- **파라미터 설명**:  
  - `cortarNo`: 행정동 코드  
  - `page`: 페이지 번호  
  - `sameAddressGroup`: 같은 건물 주소 매물 그룹핑 여부  
    - `true`: 같은 건물 매물을 하나로 묶음  
    - `false`: 개별 매물로 모두 반환

---

## 2. 수집한 데이터를 엑셀로 저장

- `pandas`를 이용해 `DataFrame`으로 처리 후 `.xlsx` 형식으로 저장  
- 데이터 분석/검토를 위한 중간 저장 단계

---

## 3. MongoDB에 저장

- **엑셀 → DataFrame → dict 변환**  
- **전처리**:  
  - `NaN` 값 처리 (예: `rentPrc`, `elevatorCount` → 0으로 채움)  
  - `tagList`: 문자열을 리스트로 변환  
  - `location`: 위도/경도를 이용해 GeoJSON 형식의 `location` 필드 생성  

- **MongoDB 저장**:  
  - 조건 분기하여 다른 컬렉션에 저장 가능 (예: 원룸/투룸 분리 저장 등)

- **Geo 인덱스 등록**:  
  - `location` 필드에 2dsphere 인덱스 등록 → 공간 기반 쿼리 가능

---