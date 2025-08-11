# 📌 프로그래머스 코딩 테스트 문제 풀기

```java
import java.util.*;

class Solution {
    public String[] solution(String[] todo_list, boolean[] finished) {
        List<String> list = new ArrayList<>();
        
        for (int i = 0; i < finished.length; i++) {
            if (!finished[i]) { // false인 경우만
                list.add(todo_list[i]);
            }
        }
        
        return list.toArray(new String[0]); // List → String[]
    }
}
```

- String 배열을 리스트로 만든 후에는 toArray(new String[0]) 사용