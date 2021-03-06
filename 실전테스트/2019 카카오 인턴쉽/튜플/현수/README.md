# 문제
- [카카오 2019 개발자 겨울 인턴십. 2번 튜플](https://programmers.co.kr/learn/courses/30/lessons/64065?language=java)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.util.Stack;

public class intern20192 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String line = sc.next();
        Solution sol = new Solution();
        System.out.println(sol.solution(line));
    }
}
class Solution {
    public int[] solution(String s) {
        int[] answer = {};
        boolean flag = false;
        List<String> ans = new ArrayList<>();
        List<List<String>> ansList = new ArrayList<>();
        List<String> temp = new ArrayList<>();

        for (int i = 1; i < s.length() - 1; i++) {
            char deli = s.charAt(i);
            if (deli == '{') {
                i += 1;
                int idx = 0;
                temp = new ArrayList<>();
                while (s.charAt(i + idx) != '}') {
                    if(s.charAt(i + idx) == ',') {
                        temp.add(s.substring(i, i + idx));
                        i = i + idx + 1;
                        idx = 0;
                    }
                    idx += 1;
                }
                temp.add(s.substring(i, i + idx));
                i = i + idx + 1;
                ansList.add(temp);
            }
        }
        flag = true;
        while(flag) {
            flag = false;
            String stand = "";
            for (List<String> integers : ansList) {
                if (integers.size() == 1) {
                    stand = integers.get(0);
                    ans.add(stand);
                    flag = true;
                    break;
                }
            }
            for (List<String> integers : ansList) {
                integers.remove(stand);
            }
        }
        answer = new int[ans.size()];
        for (int i = 0; i < ans.size(); i++) {
            answer[i] = Integer.parseInt(ans.get(i));
        }
        for (int i = 0; i < ans.size(); i++) {
            System.out.print(answer[i] + " ");
        }
        //System.out.println();
        return answer;

    }
}
```

</details>

## ⭐️느낀점⭐️
> 문자열을 파싱하는 내용은 처음 풀어보는 문제 유형이어서 낯설었다.
> 
> 확실히 자바를 써서 문자열 다루기가 편리하긴 했다. 하지만 좀더 익숙해질때까지 연습해봐야할듯하다.

## 풀이 📣
<hr/>

1️⃣ 괄호로 둘러쌓여 있는 문자들을 모두 정리해서 문자열 리스트로 변환해준다.

    - 큰 {} 안에 작은 {} 들을 각각의 리스트로 만든 후, 이 리스트들을 저장하는 리스트를 만든다.

    - 작은 리스트 중 `,`를 기준으로 구분된 숫자들을 한 개의 리스트로 반환해야한다.


2️⃣ 크기가 1인 리스트를 찾아서 해당 리스트에 저장되어 있는 독립적인 숫자를 찾아낸다.

    - 순서에 상관없이 모든 리스트들은 크기가 1개인 리스트에 저장되어 있는 값을 가지고 있다.


3️⃣ 찾아낸 숫자를 모든 리스트에서 제거하고 `ans` 리스트에 정답을 저장해둔다. 


4️⃣ `ans` 리스트에 저장되어 있는 값들을 출력하고 종료한다.

<hr/>

## 실수 😅
- 문자열 파싱하는 것이 조금 헤맸다. 처음에는 파싱하는 것이 문제를 풀기 위해 가장 기초적인 단계인데 이것도 헤매서 어떡하지 라는 자괴감이 들었으나, 파싱이 문제의 핵심 알고리즘이었던 것..

- 정답자의 코드를 보고 공부한 후, 관련 문제를 더 풀어봐야겠다.