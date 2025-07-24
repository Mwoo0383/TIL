# 🏠 Room91 프로젝트

### @PostConstruct
- 해당 클래스의 빈 등록 직후 한 번 실행됨
- 스프링 컨텍스트에 정상 등록됐는지 파악하려고 로그로 사용해봄

#### NewsContoller
```Java
@PostConstruct
    public void init() {
        this.videoPath = Paths.get(videoDir);
        System.out.println("✅ NewsController 등록됨");
    }
```