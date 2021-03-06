# 문제
- [1062. 가르침](https://www.acmicpc.net/problem/1062)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.awt.Point;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Q1062 {
    static int n, k, ans;
    static String words[] = new String[51];
    public static void main(String[] args) throws IOException {
        init();
        solution();
    }

    private static void solution() {
        if (k < 5) {
            System.out.println(0);
            return;
        }
        int chars = 0;
        chars |= (1 << ('a' - 'a'));
        chars |= (1 << ('n' - 'a'));
        chars |= (1 << ('t' - 'a'));
        chars |= (1 << ('i' - 'a'));
        chars |= (1 << ('c' - 'a'));

        learn(k - 5, chars, 0);
        System.out.println(ans);
    }

    private static void learn(int learnCnt, int chars, int start) {
        if(learnCnt == 0){
            ans = Math.max(ans, calc(chars));
            return;
        }
        for (int i = start; i < 26; i++) {
            if(((1 << i) & chars) > 0) continue;
            learn(learnCnt - 1, chars | (1 << i), i + 1);
        }
    }

    private static int calc(int chars) {
        int ret = 0;
        for (int i = 0; i < n; i++) {
            String word = words[i];
            boolean flag = true;
            for (int j = 0; j < word.length(); j++) {
                int pos = word.charAt(j) - 'a';
                if((chars & (1 << pos)) == 0) {
                    flag = false;
                    break;
                }
            }
            if(flag) ret += 1;
        }
        return ret;
    }

    private static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        k = Integer.parseInt(st.nextToken());
        for (int i = 0; i < n; i++) {
            String input = br.readLine();
            words[i] = input.substring(4, input.length() - 4);
        }
    }
}
```

</details>

## ⭐️느낀점⭐️
> 최대 21C10 가량의 경우의 수가 계산될 수 있어서 시간초과가 날 줄 알았는데, 정상적으로 작동했다. 
>
> 

## 풀이 📣
<hr/>

1️⃣ k 개의 글자를 선택한다.

    - 조합탐색으로 k 개의 글자를 선택한다. 


2️⃣ 선택한 글자들로 모든 단어를 확인해보고 만들 수 있는 단어면 카운트해준다.

    - 각 단어별로 1글자씩 비교해보면서 선택한 글자로 모두 구성되어 있으면 +1 해준다.


3️⃣ k 개의 조합 중에서 가장 많은 단어를 구성할 수 있는 경우를 찾아 그 때의 만들 수 있는 총 단어 개수를 출력한다.

<hr/>

## 실수 😅

- 조합탐색을 하는 과정에서 재귀가 너무 깊어져서 시간초과가 날 거 같아서 다른 방법으로 풀었었다.

- 하지만 그 외에는 정확한 풀이가 떠오르지 않았다.

- 결국 조합탐색 이외에 할 수 있는 최적화들을 모두 하고 조합으로 k개 글자를 선택해서 문제를 풀어서 통과했다.