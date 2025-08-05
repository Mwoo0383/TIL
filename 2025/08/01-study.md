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

### ✅ 문자열 접두사/접미사 관련 메서드

| 메서드                         | 설명                                  | 예시                                 |
| --------------------------- | ----------------------------------- | ---------------------------------- |
| `startsWith(String prefix)` | 문자열이 **특정 접두사**로 시작하는지 확인           | `"banana".startsWith("ba") → true` |
| `endsWith(String suffix)`   | 문자열이 **특정 접미사**로 끝나는지 확인            | `"banana".endsWith("na") → true`   |
| `contains(CharSequence s)`  | 문자열에 **특정 문자열이 포함되어 있는지** 확인        | `"banana".contains("nan") → true`  |
| `indexOf(String s)`         | 문자열이 처음으로 등장하는 **인덱스를 반환** (없으면 -1) | `"banana".indexOf("na") → 2`       |
| `lastIndexOf(String s)`     | 문자열이 마지막으로 등장하는 **인덱스 반환**          | `"banana".lastIndexOf("na") → 4`   |


### ✅ 기타 자주 쓰는 문자열 메서드

| 메서드                                          | 설명                                |
| -------------------------------------------- | --------------------------------- |
| `substring(int beginIndex)`                  | `beginIndex`부터 끝까지 잘라냄            |
| `substring(int beginIndex, int endIndex)`    | `beginIndex`부터 `endIndex-1`까지 잘라냄 |
| `charAt(int index)`                          | 해당 인덱스의 문자 반환                     |
| `length()`                                   | 문자열 길이 반환                         |
| `equals(String another)`                     | 문자열이 같은지 비교 (`==` 쓰면 안 됨!)        |
| `equalsIgnoreCase(String another)`           | 대소문자 무시하고 비교                      |
| `replace(String target, String replacement)` | 문자열 치환                            |
| `split(String regex)`                        | 문자열을 구분자로 나눠 배열로 반환               |
| `toLowerCase()` / `toUpperCase()`            | 소문자/대문자로 변환                       |
| `trim()`                                     | 앞뒤 공백 제거                          |
