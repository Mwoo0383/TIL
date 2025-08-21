## 🔹 char ↔ String

| 변환 방향       | 방법 (예시 코드)                     | 설명 |
|----------------|----------------------------------|------|
| char → String | String.valueOf(c)                | 권장 방법 |
|                | Character.toString(c)            | 동일 효과 |
|                | "" + c                           | 문자열 더하기 (편법) |
|                | new String(new char[]{c})        | 잘 안 씀 |
| String → char  | str.charAt(index)                | 특정 문자 추출 |

---

## 🔹 char[] ↔ String

| 변환 방향         | 방법 (예시 코드)              | 설명 |
|------------------|---------------------------|------|
| char[] → String  | new String(arr)           | 가장 많이 씀 |
|                  | String.valueOf(arr)       | 동일 효과 |
|                  | Arrays.toString(arr)      | 디버깅용, `[H, e, l, l, o]` 형태 |
| String → char[]  | str.toCharArray()         | 가장 간단하고 권장 |
|                  | 반복문 + str.charAt(i)    | 직접 변환 |

---

## 🔹 예시 코드

```java
public class Main {
    public static void main(String[] args) {
        char c = 'A';
        String s1 = String.valueOf(c);  // char → String
        char c2 = s1.charAt(0);         // String → char

        char[] arr = {'H','e','l','l','o'};
        String s2 = new String(arr);    // char[] → String
        char[] arr2 = s2.toCharArray(); // String → char[]

        System.out.println(s1);   // A
        System.out.println(c2);   // A
        System.out.println(s2);   // Hello
        System.out.println(arr2); // Hello
    }
}
