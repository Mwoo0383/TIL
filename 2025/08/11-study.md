# 📌 2025-08-11 작업 내용 정리

## 1. 지도 중심 좌표 관리 로직 개선
- `HouseMapPage.jsx`에서 지도 중심 좌표(`center`)를 상태로 관리
- 지도 이동/줌 이벤트(`idle`) 발생 시 중심 좌표 자동 업데이트
- 중심 좌표 변경 시 `fetchHouseData()`로 서버 재조회 가능하게 수정
- `mapCenterLat` undefined 에러 해결

---

## 2. 서버 필터 연동
- 필터(전세, 월세, 단기임대, 보증금/월세 최소·최대)를 서버 API 호출 시 쿼리 파라미터로 전달
- 기존 클라이언트 사이드 필터링 → 서버 사이드 필터링으로 변경
- `fetchHouseData()` 함수에서 `URLSearchParams` 사용해 파라미터 구성

---

## 3. 마커 및 클러스터링 초기화 로직 수정
- 지도 이동 시 기존 마커 클러스터 초기화 후 새로 생성
- 빈 데이터, `latitude`/`longitude` 없는 데이터는 마커 생성에서 제외

---

## 4. 전세만 선택 시 월세 검색창 비활성화
- `MapFilterBar.jsx`에서 전세만 선택된 경우 월세 최소·최대 입력창 `disabled` 처리
- `toggleFilter()`에서 전세만 선택 상태일 경우 `minRent`, `maxRent` 값 자동 초기화

```jsx
// HouseMapPage.jsx
const toggleFilter = (key) => setFilters(prev => {
    const next = { ...prev, [key]: !prev[key] };
    const isJeonseOnly = next.jeonse && !next.monthly && !next.short;
    if (isJeonseOnly) {
        next.minRent = "";
        next.maxRent = "";
    }
    return next;
});
