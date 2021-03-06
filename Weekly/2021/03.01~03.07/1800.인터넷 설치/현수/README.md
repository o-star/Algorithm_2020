# 문제
- [1800. 인터넷 설치](https://www.acmicpc.net/problem/1800)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

class Edge{
    int dst, cost;

    public Edge(int dst, int cost) {
        this.dst = dst;
        this.cost = cost;
    }
}
public class Q1800 {
    static int n, p, k;
    static List<Edge> adj[] = new List[1001];
    static int minValue[] = new int[1001];
    public static void main(String[] args) throws IOException {
        init();
        solution();
    }

    private static void solution() {
        int left = 0, right = 1000001, ans = -1;
        while(left <= right){
            int mid = left + right >> 2;
            if(isOk(mid)) {
                right = mid - 1;
                ans = mid ;
            }
            else left = mid + 1;
        }
        System.out.println(ans);
    }

    private static boolean isOk(int mid) {
        Arrays.fill(minValue, 987654321);
        PriorityQueue<Edge> pq = new PriorityQueue<>((a, b) -> (a.cost - b.cost));
        pq.add(new Edge(1, 0));
        minValue[1] = 0;
        while (!pq.isEmpty()) {
            Edge now = pq.poll();
            int here = now.dst, curCost = now.cost;
            if(curCost > minValue[here]) continue;

            for (Edge nextEdge : adj[here]) {
                int next = nextEdge.dst, nextCost = nextEdge.cost;
                if(nextCost <= mid) nextCost = 0;
                else nextCost = 1;
                if(minValue[next] > curCost + nextCost){
                    minValue[next] = curCost + nextCost;
                    pq.add(new Edge(next, minValue[next]));
                }
            }
        }
        return minValue[n] <= k;
    }

    private static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = stoi(st.nextToken());
        p = stoi(st.nextToken());
        k = stoi(st.nextToken());
        for (int i = 1; i <= n; i++)
            adj[i] = new ArrayList<>();
        for (int i = 0; i < p; i++) {
            st = new StringTokenizer(br.readLine());
            int u = stoi(st.nextToken());
            int v = stoi(st.nextToken());
            int c = stoi(st.nextToken());
            adj[u].add(new Edge(v, c));
            adj[v].add(new Edge(u, c));
        }
    }

    private static int stoi(String str) {
        return Integer.parseInt(str);
    }
}
```

</details>

## ⭐️느낀점⭐️
> 다익스트라 + 이분 탐색이라는 아주 색다른 조합의 결정문제였다.
> 
> 처음엔 결정 문제로 바꿔서 풀어야 하는 것을 몰라서 완전탐색으로 시간초과 판정을 받았다...
>
> X 원보다 비싼 간선을 k개 이하로 목표 지점까지 연결한다 = x원 보다 비싼 간선의 가중치는 1, 그렇지 않은 간선의 가중치는 0 으로 생각하고 다익스트라 최단경로 구하기. K 이하면 X원 가능! 
>
> 최대값의 최소화, 최소값의 최대화 와 같은 정해진 형태의 이분탐색 조건을 숨겨놓은 문제였다. K개의 인터넷 선은 무료 -> 가장 비싼 K개 제외 최소값 구하기 

## 풀이 📣
<hr/>

1️⃣ 양방향 간선이므로 인접 리스트에 u 와 v에 해당 간선을 저장해준다.


2️⃣ 가능한 금액 `1 ~ 1000000` 을 파라메트릭 서치를 통해 최적의 값을 구한다.

    - mid 를 구해서 mid 원으로 가능하다면 right를 줄이고, 불가능하면 left 를 늘려서 이분탐색을 진행


3️⃣ 기준값보다 큰 간선의 가중치는 1, 작은 간선의 가중치는 0 으로 생각하고 1부터 N 까지의 최단경로를 구한다.

    - 최단 경로는 다익스트라 알고리즘을 사용해서 구한다.


4️⃣ 1부터 N 까지 최단경로가 k 개 이하 = 비용 X 보다 큰 간선을 k 개 이하로만 사용한다. = k 개의 인터넷 선은 무료로 이용가능하므로 X원에 설치 가능


5️⃣ 위의 과정을 반복해서 최소의 X원을 찾아서 출력한다. 

<hr/>

## 실수 😅

- 결정 문제로 바꿔 푸는 부분을 떠올리지 못했다.

- 문제에서 비용의 범위가 `1~1000000` 임을 나중에 결정 문제라는 것을 보고나서 봤다.. 나와있는 조건좀 제대로 읽자!!! 