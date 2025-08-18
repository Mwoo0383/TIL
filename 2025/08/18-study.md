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

## Map이란?

Map은 키(Key)와 값(Value)의 쌍으로 데이터를 저장하는 자료구조입니다. 키는 중복될 수 없고, 각 키는 하나의 값에만 매핑됩니다.

## 주요 Map 구현체

### 1. HashMap
- 해시 테이블 기반
- 순서 보장 안됨
- null 키와 값 허용
- 가장 빠른 성능 (O(1))

```java
Map<String, Integer> hashMap = new HashMap<>();
```

### 2. LinkedHashMap
- HashMap + 연결 리스트
- 삽입 순서 유지
- null 키와 값 허용

```java
Map<String, Integer> linkedMap = new LinkedHashMap<>();
```

### 3. TreeMap
- Red-Black 트리 기반
- 키 기준으로 자동 정렬
- null 키 불허용 (값은 허용)
- 성능: O(log n)

```java
Map<String, Integer> treeMap = new TreeMap<>();
```

### 4. Hashtable
- HashMap과 유사하지만 동기화됨
- null 키와 값 불허용
- 레거시 클래스

```java
Map<String, Integer> hashtable = new Hashtable<>();
```

## 기본 메서드

### 데이터 추가/수정

```java
Map<String, Integer> map = new HashMap<>();

// 값 추가
map.put("apple", 100);
map.put("banana", 200);

// 모든 엔트리 추가
Map<String, Integer> otherMap = Map.of("orange", 300, "grape", 400);
map.putAll(otherMap);

// 키가 없을 때만 추가
map.putIfAbsent("apple", 150); // 이미 존재하므로 무시됨
map.putIfAbsent("kiwi", 250);  // 새로 추가됨
```

### 데이터 조회

```java
// 값 가져오기
Integer applePrice = map.get("apple");

// 키가 없을 때 기본값 반환
Integer lemonPrice = map.getOrDefault("lemon", 0);

// 키 존재 여부 확인
boolean hasApple = map.containsKey("apple");

// 값 존재 여부 확인
boolean hasPrice100 = map.containsValue(100);

// 크기 확인
int size = map.size();

// 비어있는지 확인
boolean isEmpty = map.isEmpty();
```

### 데이터 삭제

```java
// 키로 삭제
Integer removedValue = map.remove("apple");

// 키-값 쌍이 일치할 때만 삭제
boolean removed = map.remove("banana", 200);

// 모든 데이터 삭제
map.clear();
```

## 반복 및 탐색

### 1. Entry Set 이용

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println(key + " : " + value);
}
```

### 2. 키 집합 이용

```java
for (String key : map.keySet()) {
    Integer value = map.get(key);
    System.out.println(key + " : " + value);
}
```

### 3. 값 집합 이용

```java
for (Integer value : map.values()) {
    System.out.println("Value: " + value);
}
```

### 4. forEach (Java 8+)

```java
map.forEach((key, value) -> 
    System.out.println(key + " : " + value)
);
```

## 고급 메서드 (Java 8+)

### compute 계열

```java
Map<String, Integer> map = new HashMap<>();

// 키에 대한 값을 계산하여 저장
map.compute("apple", (key, value) -> 
    value == null ? 1 : value + 1
);

// 키가 있을 때만 계산
map.computeIfPresent("apple", (key, value) -> value * 2);

// 키가 없을 때만 계산
map.computeIfAbsent("banana", key -> key.length());
```

### merge

```java
// 키가 있으면 기존 값과 새 값을 결합, 없으면 새 값 저장
map.merge("apple", 10, (oldValue, newValue) -> oldValue + newValue);

// 단어 개수 세기 예제
String[] words = {"apple", "banana", "apple", "cherry", "banana"};
Map<String, Integer> wordCount = new HashMap<>();

for (String word : words) {
    wordCount.merge(word, 1, Integer::sum);
}
```

### replace 계열

```java
// 값 교체
map.replace("apple", 100, 150); // 기존 값이 100일 때만 150으로 변경
map.replace("banana", 200);     // 무조건 200으로 변경

// 모든 값 교체
map.replaceAll((key, value) -> value * 2);
```

## 실용적인 사용 예시

### 1. 단어 빈도 계산

```java
public Map<String, Integer> countWords(String[] words) {
    Map<String, Integer> wordCount = new HashMap<>();
    
    for (String word : words) {
        wordCount.merge(word, 1, Integer::sum);
    }
    
    return wordCount;
}
```

### 2. 그룹핑

```java
// 학생들을 학년별로 그룹핑
class Student {
    private String name;
    private int grade;
    
    // 생성자, getter 생략...
}

List<Student> students = Arrays.asList(
    new Student("Alice", 1),
    new Student("Bob", 2),
    new Student("Charlie", 1)
);

Map<Integer, List<Student>> studentsByGrade = students.stream()
    .collect(Collectors.groupingBy(Student::getGrade));
```

### 3. 캐시 구현

```java
public class SimpleCache<K, V> {
    private final Map<K, V> cache = new HashMap<>();
    
    public V get(K key, Function<K, V> valueSupplier) {
        return cache.computeIfAbsent(key, valueSupplier);
    }
    
    public void put(K key, V value) {
        cache.put(key, value);
    }
    
    public void evict(K key) {
        cache.remove(key);
    }
}
```

### 4. 설정 관리

```java
public class ConfigManager {
    private final Map<String, String> config = new HashMap<>();
    
    public void loadDefaults() {
        config.put("timeout", "30000");
        config.put("retries", "3");
        config.put("debug", "false");
    }
    
    public String getConfig(String key) {
        return config.getOrDefault(key, "");
    }
    
    public int getIntConfig(String key, int defaultValue) {
        return Integer.parseInt(config.getOrDefault(key, String.valueOf(defaultValue)));
    }
}
```

## Map 변환 및 필터링 (Stream API)

```java
Map<String, Integer> originalMap = Map.of(
    "apple", 100,
    "banana", 200,
    "cherry", 300
);

// 값 기준 필터링
Map<String, Integer> expensive = originalMap.entrySet().stream()
    .filter(entry -> entry.getValue() > 150)
    .collect(Collectors.toMap(
        Map.Entry::getKey,
        Map.Entry::getValue
    ));

// 키 변환
Map<String, Integer> upperKeys = originalMap.entrySet().stream()
    .collect(Collectors.toMap(
        entry -> entry.getKey().toUpperCase(),
        Map.Entry::getValue
    ));

// 값 변환
Map<String, String> stringValues = originalMap.entrySet().stream()
    .collect(Collectors.toMap(
        Map.Entry::getKey,
        entry -> "Price: " + entry.getValue()
    ));
```

## 성능 비교

| 구현체 | 추가/조회/삭제 | 순서 보장 | 정렬 | 동기화 | null 허용 |
|--------|---------------|-----------|------|--------|-----------|
| HashMap | O(1) | X | X | X | 키/값 모두 |
| LinkedHashMap | O(1) | O (삽입순) | X | X | 키/값 모두 |
| TreeMap | O(log n) | O (키 순) | O | X | 값만 |
| Hashtable | O(1) | X | X | O | 불허용 |

## 주의사항

### 1. null 처리

```java
Map<String, String> map = new HashMap<>();
map.put(null, "null key");
map.put("null value", null);

// TreeMap은 null 키 불허용
Map<String, String> treeMap = new TreeMap<>();
// treeMap.put(null, "error"); // NullPointerException
```

### 2. 동시성 문제

```java
// 멀티스레드 환경에서 안전한 Map
Map<String, Integer> safeMap = new ConcurrentHashMap<>();

// 또는 Collections.synchronizedMap() 사용
Map<String, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());
```

### 3. 불변 Map

```java
// Java 9+
Map<String, Integer> immutableMap = Map.of(
    "apple", 100,
    "banana", 200
);

// 또는
Map<String, Integer> immutableMap2 = Collections.unmodifiableMap(originalMap);
```

## 유용한 팁

1. **초기 용량 설정**: 대량의 데이터를 다룰 때 초기 용량을 설정하면 성능 향상
2. **적절한 구현체 선택**: 요구사항에 따라 HashMap, LinkedHashMap, TreeMap 중 선택
3. **null 체크**: null 값이 들어올 수 있는 경우 적절한 처리 필요
4. **동시성**: 멀티스레드 환경에서는 ConcurrentHashMap 사용 권장

## 📌 Java Arrays 클래스 - 코딩테스트 핵심 요약

```java
import java.util.*;

int[] arr = {5, 3, 8, 1, 2};

// ✅ 출력
System.out.println(Arrays.toString(arr));          // [5, 3, 8, 1, 2]
System.out.println(Arrays.deepToString(new int[][]{{1,2},{3,4}})); // [[1, 2], [3, 4]]

// ✅ 정렬
Arrays.sort(arr);                                  // 오름차순
Integer[] arrObj = {5,3,8,1,2};
Arrays.sort(arrObj, Collections.reverseOrder());   // 내림차순

// ✅ 복사
int[] copy1 = Arrays.copyOf(arr, 7);               // 길이 7로 복사 (부족한 건 0)
int[] copy2 = Arrays.copyOfRange(arr, 1, 4);       // [인덱스1 ~ 3] 복사

// ✅ 채우기
int[] fillArr = new int[5];
Arrays.fill(fillArr, 7);                           // [7, 7, 7, 7, 7]
Arrays.fill(fillArr, 1, 4, 9);                     // [7, 9, 9, 9, 7]

// ✅ 검색 (정렬 필수)
Arrays.sort(arr);
int idx = Arrays.binarySearch(arr, 5);             // 값 5의 인덱스

// ✅ 비교
System.out.println(Arrays.equals(new int[]{1,2}, new int[]{1,2})); // true
System.out.println(Arrays.deepEquals(new int[][]{{1}}, new int[][]{{1}})); // true

// ✅ 스트림 활용
int[] nums = {1,2,3,4,5};
int sum  = Arrays.stream(nums).sum();              // 15
double avg = Arrays.stream(nums).average().getAsDouble(); // 3.0
int max  = Arrays.stream(nums).max().getAsInt();   // 5
int min  = Arrays.stream(nums).min().getAsInt();   // 1
