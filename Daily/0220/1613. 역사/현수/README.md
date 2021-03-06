# 문제
- [1613. 역사](https://www.acmicpc.net/problem/1613)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Q1613 {
    static int N, K, arr[][] = new int[401][401];
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    public static void main(String[] args) throws IOException {
        init();
        solution();
    }

    static void solution() throws IOException {
        floyd();
        int c = stoi(br.readLine());
        while(c-- > 0){
            StringTokenizer st = new StringTokenizer(br.readLine());
            int s = stoi(st.nextToken()), e = stoi(st.nextToken());
            if (arr[s][e] != 987654321) {
                System.out.println(-1);
                continue;
            }
            if (arr[e][s] != 987654321) {
                System.out.println(1);
                continue;
            }
            System.out.println(0);
        }
    }

    static void floyd() {
        for (int k = 1; k <= N; k++) {
            for (int i = 1; i <= N; i++) {
                for (int j = 1; j <= N; j++) {
                    if(arr[i][j] > arr[i][k] + arr[k][j])
                        arr[i][j] = arr[i][k] + arr[k][j];
                }
            }
        }
    }

    static void init() throws IOException {
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = stoi(st.nextToken());
        K = stoi(st.nextToken());
        for (int i = 1; i <= N; i++) {
            Arrays.fill(arr[i], 987654321);
        }
        for (int i = 0; i < K; i++) {
            st = new StringTokenizer(br.readLine());
            int u = stoi(st.nextToken());
            int v = stoi(st.nextToken());
            arr[u][v] = 1;
        }
    }

    static int stoi(String str) {
        return Integer.parseInt(str);
    }
}
```

</details>

## ⭐️느낀점⭐️
> 처음에 그래프 간에 순서가 존재한다는 조건에 위상정렬을 떠올려서 풀었다가 틀렸다.
>
> 그러고나서 생각해보니, 1-3, 1-2 같은 경우 2와 3의 순서가 모호하지만 위상정렬로 풀 경우 2가 3보다 앞서게 되는 문제가 있다는 것을 알아차렸다.
> 
> 그래서 플로이드-와샬 방법으로 노드 간 단방향 간선에 대해 최소비용을 저장해서 어느 노드가 순서가 빠른지를 확인하였다.

## 풀이 📣
<hr/>

1️⃣ 플로이드-와샬 알고리즘을 사용하여 모든 노드간에 최단거리 비용을 구해놓는다.


2️⃣ 입력으로 들어오는 값으로 노드간의 순서를 따져본다.

    - u에서 v로 최단거리 존재 = u가 v보다 순서가 빠름.


3️⃣ 모든 경우에 대해 순서의 우선순위를 출력해준다.

<hr/>

## 실수 😅
- 그래프의 순서만 생각하고 위상정렬로 접근했다가 뒤늦게 문제가 있다는 것을 발견하고 다르게 풀었다.