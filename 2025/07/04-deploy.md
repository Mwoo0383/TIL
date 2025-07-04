# github actions 배포 자동화하기

## 📌 deploy.yml 작성 중 트러블 슈팅

### 1. secrets 변수를 읽지 못함 
- secret 변수는 **Repository secrets**에 작성해야함
- **Environment secrets**는 prod/staging 환경 구분할 때 사용

### 2. 경로 인식 문제
```
- name: Copy backend jar and static to EC2
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_KEY }}
          source: |
            BuDongSan/build/libs/BuDongSan-0.0.1-SNAPSHOT.jar
            static/
          target: /home/ubuntu/Room91
```

⭕️ 실제 내 디렉토리 구조에 맞게 변경 및 백엔드, 프론트 전송 구조를 나눔
```
      # Spring Boot jar만 전송
      - name: Copy backend jar to EC2
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_KEY }}
          source: "BuDongSan/build/libs/BuDongSan-0.0.1-SNAPSHOT.jar"
          target: /home/ubuntu/Room91

      # dist 디렉토리만 전송
      - name: Copy frontend dist to EC2
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_KEY }}
          source: "frontend/dist/*"
          target: /home/ubuntu/Room91/static
```