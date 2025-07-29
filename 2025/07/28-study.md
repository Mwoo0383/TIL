# 🏠 Room91

## 도메인 생성 후 연결 -> room91.org

### 1. WebConfig에 Origin 허용 설정
### 2. nginx에도 도메인 이름 추가

## Https 설정

### 1. nginx에 https 서버 블록 및 SSL 인증 추가
```bash
sudo apt update
sudo apt install certbot
sudo apt install python3-certbot-nginx

# 도메인으로 인증서 발급
sudo certbot certonly --standalone -d room91.org -d www.room91.org
```
- 발급 성공하면 /etc/letsencrypt/live/room91.org/ 경로에 인증서가 생김

### 2. server.config에 SSL 설정 추가
```nginx
server {
    listen 443 ssl;
    server_name room91.org www.room91.org;

    ssl_certificate     /etc/letsencrypt/live/room91.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/room91.org/privkey.pem;

    ...
}
```

### 3. docker-compose의 nginx컨테이너에 볼륨 추가
```yml
volumes:
  - /etc/letsencrypt:/etc/letsencrypt:ro
```