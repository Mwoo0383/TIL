# Java Integer compare() 함수 설명

Java의 Integer 클래스에는 두 개의 compare() 메서드가 있습니다:

## 1. 인스턴스 메서드: compareTo()

```java
public int compareTo(Integer anotherInteger)
```

Integer 객체를 다른 Integer 객체와 비교합니다.

```java
Integer a = 10;
Integer b = 20;
Integer c = 10;

int result1 = a.compareTo(b); // -1 (a < b)
int result2 = b.compareTo(a); // 1 (b > a)  
int result3 = a.compareTo(c); // 0 (a == c)
```

## 2. 정적 메서드: compare()

```java
public static int compare(int x, int y)
```

두 int 값을 직접 비교하는 유틸리티 메서드입니다.

```java
int result1 = Integer.compare(10, 20); // -1
int result2 = Integer.compare(20, 10); // 1
int result3 = Integer.compare(10, 10); // 0
```

## 반환값 규칙

- **음수 (-1)**: 첫 번째 값이 두 번째 값보다 작을 때
- **0**: 두 값이 같을 때  
- **양수 (1)**: 첫 번째 값이 두 번째 값보다 클 때

## 실용적인 사용 예시

### 정렬에서 활용

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5);

// 오름차순 정렬
numbers.sort(Integer::compare);
// 결과: [1, 1, 3, 4, 5]

// 내림차순 정렬
numbers.sort((a, b) -> Integer.compare(b, a));
// 결과: [5, 4, 3, 1, 1]
```

### Stream에서 활용

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6);

// 최댓값 찾기
Optional<Integer> max = numbers.stream()
    .max(Integer::compare);

// 최솟값 찾기
Optional<Integer> min = numbers.stream()
    .min(Integer::compare);
```

### 커스텀 객체 정렬

```java
class Person {
    private String name;
    private int age;
    
    // 생성자, getter 생략...
    
    public int getAge() {
        return age;
    }
}

List<Person> people = Arrays.asList(
    new Person("Alice", 30),
    new Person("Bob", 25),
    new Person("Charlie", 35)
);

// 나이순으로 정렬
people.sort((p1, p2) -> Integer.compare(p1.getAge(), p2.getAge()));
```

## 성능 및 장점

- 정적 메서드인 `Integer.compare()`는 박싱/언박싱을 피할 수 있어서 성능상 더 효율적
- 메서드 레퍼런스로 사용하기 편리 (`Integer::compare`)
- null 체크가 필요 없음 (primitive int 값 사용)
- 코드가 더 간결하고 읽기 쉬움

## 주의사항

```java
// 잘못된 사용 예시
Integer a = null;
Integer b = 10;

// NullPointerException 발생
int result = a.compareTo(b);

// 안전한 사용 방법
int result = Objects.compare(a, b, Integer::compareTo);
```

null 값이 있을 수 있는 경우에는 `Objects.compare()` 메서드를 사용하는 것이 안전합니다.