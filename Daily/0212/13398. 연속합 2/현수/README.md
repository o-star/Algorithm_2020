# 문제
- [13398. 연속합](https://www.acmicpc.net/problem/13398)

## 코드
``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Q13398 {
    static int n, arr[], cache[][], pSum[];
    public static void main(String[] args) throws IOException {
        init();
        solution();
    }
    static void solution() {
        cache[n-1][0] = cache[n-1][1] = arr[n-1];
        for (int i = n-2; i >= 0; --i) {
            int stop = arr[i], proceed = arr[i] + cache[i + 1][0];
            cache[i][0] = Math.max(stop, proceed);
            proceed = Math.max(cache[i + 1][0], arr[i] + cache[i + 1][1]);
            cache[i][1] = Math.max(stop, proceed);
        }
        int ans = -987654321;
        for (int i = 0; i < n; i++) {
            int max = Math.max(cache[i][0], cache[i][1]);
            ans = Math.max(ans, max);
        }
        System.out.println(ans);
    }

    static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());
        arr = new int[n]; cache = new int[n][2];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < n; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
            Arrays.fill(cache[i], 0);
        }
    }
}
```


## ⭐️느낀점⭐️
> 이전에 풀었던 1194번 문제에서 BFS 할 때 방문 체크에 3차원 배열을 썼던 것을 기억해내서 여기서는 4차원 배열로 체크하였다. <br>
> 옛날에 봤을 때는 저걸 어떻게 풀지.. 했는데 이번에는 손쉽게 풀어냈다. 후후

## 풀이 📣
<hr/>
1️⃣ 연속으로 합을 구하는 문제기 때문에 부분합을 떠올린다.


2️⃣ 수열에서 연속되는 값들의 합이 가장 큰 부분수열을 찾는다. 

    - 따라서 2차원 캐시를 이용해서 cache[몇 번째 숫자인지][이미 앞에서 1개를 지웠는지] 를 저장한다.

    - 현재 위치에서는 cache[현재 숫자의 위치][1] 은 cache[현재 숫자의 위치 + 1][0] 와 cache[현재 숫자의 위치 + 1][1] 가 될 수 있다.

    - 현재 위치에서는 cache[현재 숫자의 위치][0] 은 cache[다음 숫자의 위치][0] 와 cache[다음 숫자의 위치][1] 가 될 수 있다.


3️⃣ 가장 마지막부터 부분합을 구해나가는데, 수열에서 최대 1개를 제거할 수 있다.

    - 가능한 모든 경우는 특정 숫자의 위치에서 부분 수열을 종료하거나, 다음 번째 숫자로 계속 진행하는 경우이다.

    - 특정 위치에서 시작하는 부분 수열의 최대 연속합은 특정 위치에서 바로 종료하거나, 연속되는 숫자 중 1개를 제거하거나, 아니면 모두 더하는 경우밖에 없다.
    

4️⃣ 만약 현재 위치의 숫자를 제거하지 않는다면 현재 상태의 최대값은 `cache[idx + 1][1]` 이 된다.


5️⃣ 10번안에 빨간 구슬만 구멍에 도달하는 경우가 존재할 경우 성공.

<hr/>

## 실수 😅
- ans 를 처음에 0으로 초기화해놔서 안풀렸다.. -987654321 로 설정 후 제출하니 통과!  