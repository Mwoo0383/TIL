# 🏠 Room91 프로젝트

## ⚒️ CORS 문제 해결
- WebConfig에서 allowedOriginPatterns를 allowedOrigins 변경
- MapPage에서 CORS 대응을 위해 fetch에 credentials 옵션 추가
- nginx -> server.conf에서 proxy_pass 이름을 docker-compose에 컨테이너 이름과 맞춤

## ⚒️ 지도 클러스터러 문제 해결중 ..