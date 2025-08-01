# 🏠 Room91 프로젝트

## application.yml dev와 prod로 분리
- .env를 dev와 prod로 나눠 환경변수 관리
```yml
spring:
  application:
    name: BuDongSan

  profiles:
    active: ${SPRING_PROFILES_ACTIVE}
```
- SPRING_PROFILES_ACTIVE 변수로 환경 관리

# 📖 코딩테스트(프로그래머스)
## 배열 5문제 기초 풀이
```Java
// 값 넣는대로 배열의 길이가 됨
List<Integer> list = new ArrayList<>();

list.add(1); // 값 1 추가
list.get(0); // list의 인덱스 0인 값 확인
list.add(2); // 값 2 추가
list.remove(0); // 인덱스 0인 요소 제거
list.remove(Integer.valuOf(2)); // 값 2 제거

int last = list.size()-1; // list의 마지막 인덱스

// Integer타입의 배열을 int타입의 배열로 바꿈
list.stream().mapToInt(Integer::intValue).array();
```
