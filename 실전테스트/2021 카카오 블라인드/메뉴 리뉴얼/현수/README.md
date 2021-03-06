# 문제
- [카카오 2021 블라인드 공개 채용. 2번 메뉴 리뉴얼](https://programmers.co.kr/learn/courses/30/lessons/72411)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.util.*;

class Combi{
    String comb;
    int count;

    public Combi(String comb, int count) {
        this.comb = comb;
        this.count = count;
    }
}
class Solution {
    static List<String> ans = new ArrayList<>();
    static List<Combi> combi[] = new List[11];
    static int popular[] = new int[11];
    static String[] orders;
    static int[] course;
    static boolean visited[] = new boolean[26];

    public String[] solution(String[] o, int[] c) {
        String[] answer = {};
        orders = o; course = c;

        for (int count : course)
            combi[count] = new ArrayList<>();

        for (String order : orders) {
            for (int i = 0; i < order.length(); i++) {
                visited[order.charAt(i) - 'A'] = true;
            }
        }

        for (int count : course) {
            getCombination(count, 0, 0, 0);
            Collections.sort(combi[count], (a, b) -> (b.count - a.count));
            
            int max = combi[count].get(0).count;
            String maxWord = combi[count].get(0).comb;
            if(max < 2) continue;
            ans.add(maxWord);
            for (Combi comb : combi[count]) {
                if(max == comb.count) {
                    if(maxWord.equals(comb.comb)) continue;
                    ans.add(comb.comb);
                }
                else break;
            }
        }
        Collections.sort(ans);
        answer = new String[ans.size()];
        for (int i = 0; i < ans.size(); i++) {
            answer[i] = ans.get(i);
        }
        return answer;
    }

    private void getCombination(int max, int start, int made, int cnt) {
        if(max == cnt){
            int count = 0;
            for (String order : orders) {
                int mask = makeMask(order);
                if((mask & made) == made)
                    count += 1;
            }
            String comb = findComb(made);
            combi[max].add(new Combi(comb, count));
            return;
        }

        for (int i = start; i < 26; i++) {
            if(visited[i])
                getCombination(max, i + 1, made | (1 << i), cnt + 1);
        }
    }

    private int makeMask(String word) {
        int ret = 0;
        for (int i = 0; i < word.length(); i++) {
            int pos = word.charAt(i) - 'A';
            ret |= (1 << pos);
        }
        return ret;
    }

    private String findComb(int made) {
        String ret = "";
        for (int i = 0; i < 26; i++) {
            if(((1 << i) & made) > 0)
                ret += (char)('A' + i);
        }
        return ret;
    }
}
```

</details>

## ⭐️느낀점⭐️
> 처음에 문제 이해가 안되서 한 30분 쓴거같다.. 알고 보니 `가장 많이 함께 주문한 단품메뉴` 를 코스 메뉴로 구성하는 것이었다.
>
> 효율성 테스트를 안하는 문제는 그냥 브루트포스로 풀어도 되는가보다.

## 풀이 📣
<hr/>

1️⃣ 등장하는 단품 메뉴들을 체크해놓고 그 중 `course` 의 개수만큼 조합을 만들어서 확인한다.

    - getCombination(코스요리 개수, 시작 인덱스, 현재까지 선택한 메뉴들, 선택한 메뉴의 개수)

    - 선택한 메뉴들은 비트마스킹을 통해 관리한다.


2️⃣ 코스요리 개수별로 가장 많이 선택되는 조합을 저장해둔다.

    - 내림차순으로 정렬해서 첫 번째 인덱스 저장. 이후 이 값으로 다음 값들 비교.

    - 특정 코스요리 개수에서 가장 많이 선택되는 조합이 여러 개 일 경우 모두 정답에 포함해준다. 


3️⃣ 선택된 모든 코스 메뉴를 오름차순으로 정렬해서 출력해준다.

<hr/>

## 실수 😅
- 조합을 어떤 식으로 구현할지 우왕좌왕했다. 
  
- 조금이라도 효율적으로 짜려고 고민했는데, 채점해보니 8초에 1.6GB 메모리를 차지하는 테스트 케이스도 통과하는걸 보고 그냥 맞추기만 하면 되는구나 싶었다.