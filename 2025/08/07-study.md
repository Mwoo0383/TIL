## 프로그래머스 코딩테스트 문제 풀이

### 1. 슬라이싱 문제 풀이

#### 1) 기본 풀이

```java
import java.util.*;

class Solution {
    public int[] solution(int n, int[] slicer, int[] num_list) {
        List<Integer> lst = new ArrayList<>();
        int start = slicer[0];
        int end = slicer[1];
        int step = slicer[2];
        if (n == 1) {
            for (int i = 0; i <= end; i++) lst.add(num_list[i]);
        } else if (n == 2) {
            for (int i = start; i < num_list.length; i++) lst.add(num_list[i]);
        } else if (n == 3) {
            for (int i = start; i <= end; i++) lst.add(num_list[i]);
        } else if (n == 4) {
            for (int i = start; i <= end; i += step) {
                lst.add(num_list[i]);
            }
        }
        return lst.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### 2) 개선된 풀이 (copyOf, copyOfRange 활용)

```java
import java.util.Arrays;

class Solution {
    public int[] solution(int n, int[] slicer, int[] numList) {
        int start, end, step;

        switch (n) {
            case 1 -> {
                start = 0;
                end = slicer[1] + 1; // 끝 포함이므로 +1
                return Arrays.copyOfRange(numList, start, end);
            }
            case 2 -> {
                start = slicer[0];
                end = numList.length;
                return Arrays.copyOfRange(numList, start, end);
            }
            case 3 -> {
                start = slicer[0];
                end = slicer[1] + 1;
                return Arrays.copyOfRange(numList, start, end);
            }
            case 4 -> {
                start = slicer[0];
                end = slicer[1];
                step = slicer[2];
                int size = (end - start) / step + 1;

                int[] result = new int[size];
                for (int i = 0, idx = start; i < size; i++, idx += step) {
                    result[i] = numList[idx];
                }
                return result;
            }
            default -> throw new IllegalArgumentException("Invalid n: " + n);
        }
    }
}
```

---

### 자주 사용하는 Arrays 메서드 정리

| 메서드명                           | 설명                                              | 예시                                                         |
| ----------------------------------- | ------------------------------------------------- | ------------------------------------------------------------ |
| `copyOf(array, newLength)`          | 배열을 지정한 길이만큼 복사                        | `int[] b = Arrays.copyOf(a, 5);`                             |
| `copyOfRange(array, from, to)`      | 지정한 범위만큼 배열 복사 (`from` 포함, `to` 미포함) | `int[] b = Arrays.copyOfRange(a, 2, 5);`                     |
| `sort(array)`                       | 배열 오름차순 정렬 (기본 타입)                     | `Arrays.sort(a);`                                            |
| `sort(array, Comparator)`           | 배열 정렬 (객체 타입일 때 정렬 기준 제공)           | `Arrays.sort(strArr, Comparator.reverseOrder());`             |
| `binarySearch(array, key)`          | 정렬된 배열에서 이진 탐색 수행 → 인덱스 반환        | `int index = Arrays.binarySearch(a, 10);`                    |
| `equals(arr1, arr2)`                | 두 배열이 같은지 비교 (값 비교)                    | `Arrays.equals(a, b);`                                       |
| `deepEquals(arr1, arr2)`            | 2차원 이상 배열 비교                               | `Arrays.deepEquals(a2D, b2D);`                               |
| `fill(array, value)`                | 배열의 모든 값을 지정된 값으로 채움                 | `Arrays.fill(a, 0);`                                         |
| `setAll(array, IntUnaryOperator)`    | 배열의 각 요소를 함수로 초기화                     | `Arrays.setAll(a, i -> i * 2);`                              |
| `toString(array)`                   | 배열을 문자열로 출력                               | `System.out.println(Arrays.toString(a));`                    |
| `deepToString(array)`               | 2차원 이상 배열을 문자열로 출력                    | `System.out.println(Arrays.deepToString(a2D));`              |
| `asList(T... a)`                    | 배열을 `List`로 변환 (Object 배열만 가능, 수정 불가) | `List<String> list = Arrays.asList("a", "b");`               |
