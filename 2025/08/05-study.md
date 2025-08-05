# 📖 코딩 테스트 문제 풀며 공부

## 1. StringBuilder
자바에서 문자열을 효율적으로 조립하거나 수정할 때 사용하는 클래스

> 🚨 자바에서 String은 **불변(immutable)** 이라서 한 번 생성되면 내부값이 바뀌지 않음. 그래서 문자열을 +로 계속 붙이면 매번 새로운 String 객체를 만들게됨  
> ❗️ 메모리 낭비가 생기고 성능도 떨어짐.

### 기본 사용법
```Java
StringBuilder sb = new StringBuilder();

// 문자열 추가
sb.append("Hello");
sb.append(" ");
sb.append("World");

// 문자열 완성
String result = sb.toString(); // "Hello World"
```

| 메서드                                     | 설명                       |
| --------------------------------------- | ------------------------ |
| `append(String s)`                      | 문자열 뒤에 붙이기               |
| `insert(int offset, String s)`          | 특정 위치에 문자열 삽입            |
| `delete(int start, int end)`            | start부터 end 전까지 삭제       |
| `replace(int start, int end, String s)` | start\~end 범위 문자열을 s로 교체 |
| `reverse()`                             | 문자열 뒤집기                  |
| `toString()`                            | 최종 문자열로 변환               |

### ✅ StringBuffer 와의 차이
| 항목        | `StringBuilder`                     | `StringBuffer`          |
| --------- | ----------------------------------- | ----------------------- |
| **동기화**   | ❌ 비동기 (Thread-safe 아님)              | ✅ 동기화됨 (Thread-safe)    |
| **성능**    | 빠름 (싱글 스레드에 적합)                     | 느림 (멀티 스레드에서 안전하게 동작)   |
| **사용 시기** | 일반적인 상황, 단일 스레드 작업에 적합              | 멀티 스레드 환경에서 문자열 조작 시 사용 |
| **기능**    | 동일 (`append`, `insert`, `delete` 등) | 동일                      |

🖊️ StringBuilder는 싱글 스레드 환경일 때 가볍고 빠름. StringBuffer는 멀티 스레드 환경이고 동시에 문자열을 조작할 때 사용