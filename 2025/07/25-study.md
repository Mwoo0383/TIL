# 🏠 Room91 프로젝트

## ⁉️ 백엔드에 새로운 컨트롤러 추가했는데  EC2 환경에서 계속 api를 찾지 못함
- 로컬에서는 적용되는데 EC2에서는 api를 찾지 못함
```Java
    @PostConstruct
    public void init() {
        this.videoPath = Paths.get(videoDir);
        System.out.println("✅ NewsController 등록됨");
    }
````
- 로그를 추가해서 확인해보니 로그도 확인이 안됨

### 원인
- Dockerfile로 초기 이미지를 만든 후 계속 사용함
- docker-compose.yml에서 한번 이미지 생성한 파일을 그대로 계속 사용
```yml
  spring-app:
    image: housing-image:latest
    container_name: Housing-app
````
- 기존에 있던건 계속 덮어씌워지고 새로 만든건 반영이 안된 것!

### 해결
```yml
  spring-app:
    build:
      context: .
      dockerfile: Dockerfile
````
- image는 삭제하고 dockerfile을 docker-compose.yml 실행이 빌드되도록 사용함
