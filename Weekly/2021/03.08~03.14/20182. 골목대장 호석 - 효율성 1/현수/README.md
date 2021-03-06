# 문제
- [20182. 골목 대장 호석 - 효율성1](https://www.acmicpc.net/problem/20182)

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
    int v, cost, preCost;

    public Edge(int v, int cost, int pre) {
        this.v = v;
        this.cost = cost;
        this.preCost = pre;
    }
}
public class Q20182 {
    static int n, m, a, b, c;
    static List<Edge> adj[];
    static List<Integer> fees = new ArrayList<>();
    public static void main(String[] args) throws IOException {
        init();
        solution();
    }
    private static void solution() {
        int left = 0, right = 20, mid = 0, ans = 987654321;
        while(left <= right){
            mid = (left + right) / 2;
            if(check(mid)){
                right = mid - 1;
                ans = mid;
            }
            else left = mid + 1;
        }
        if (ans != 987654321)
            System.out.println(ans);
        else {
            System.out.println(-1);
        }
    }

    private static boolean check(int max) {
        int dist[] = new int[n + 1];
        Arrays.fill(dist, 987654321);
        dist[a] = 0;

        PriorityQueue<Edge> pq = new PriorityQueue<>((a, b) -> (a.cost - b.cost));
        pq.add(new Edge(a, 0, 0));
        while(!pq.isEmpty()){
            Edge p = pq.poll();
            if(dist[p.v] < p.cost)
                continue;
            for (Edge e : adj[p.v]) {
                int next = e.v, nextCost = e.cost;
                if(dist[next] > dist[p.v] + nextCost && max >= nextCost){
                    dist[next] = dist[p.v] + nextCost;
                    pq.add(new Edge(next, dist[next], nextCost));
                }
            }
        }
        // max 보다 작은 비용의 간선만으로 c원 이하의 최단경로를 구할 수 있는지 결정
        return dist[b] <= c;
    }

    private static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = stoi(st.nextToken());
        m = stoi(st.nextToken());
        a = stoi(st.nextToken());
        b = stoi(st.nextToken());
        c = stoi(st.nextToken());
        adj = new List[n + 1];
        for (int i = 0; i <= n; i++)
            adj[i] = new ArrayList<>();


        for (int i = 0; i < m; i++) {
            st= new StringTokenizer(br.readLine());
            int s = stoi(st.nextToken()), e = stoi(st.nextToken()), cost = stoi(st.nextToken());
            adj[s].add(new Edge(e, cost, 0));
            adj[e].add(new Edge(s, cost, 0));
            fees.add(cost);
        }
    }
    private static int stoi(String str) {
        return Integer.parseInt(str);
    }
}
```
</details>

## ⭐️느낀점⭐️
> 다익스트라 방식에서 조금 변형해서 풀 수 있을거라 생각했는데, 결정 문제로 바꿔서 다익스트라를 써서 풀어야했다.

## 풀이 📣
<hr/>

1️⃣ 간선의 비용은 1에서 20 사이로 가능하므로, 결정문제로 바꿔서 모든 경우에 대해 탐색해본다.

    - 이분탐색으로 범위 내에 mid 값을 잡아서 최소 비용이 C 이하로 나오면 right 을 줄이고, C보다 커지면 left 를 키워준다.

    - right을 줄이는 경우는, 더 작은 최대 이동가능 비용을 의미한다.

    - left를 키우는 경우는, 더 큰 최대 이동가능 비용을 의미한다.


2️⃣ mid 보다 작은 비용을 갖는 간선만 이동할 수 있도록 다익스트라 최단 경로 알고리즘을 사용한다.

    - 해당 경우에 a에서 b까지 c 이하의 비용으로 이동할 수 있다면 true를 반환한다.

    - 최소 비용이 c 보다 크다면 false 를 반환한다.


3️⃣ 모든 경우를 따졌을 때, 최대 비용`이동가능한 간선의 최대값, 즉 mid`이 최소가 되는 경우를 찾아 출력한다.

## 실수 😅
<hr/>

- 노드의 개수와 간선의 개수를 O(ElogV) 의 시간을 갖는 다익스트라 알고리즘을 떠올렸다.

- 다익스트라 알고리즘만으로는 통과할 수 없었다.

- 사실 이 문제는 파라메트릭 서치를 사용하는 결정 문제로 바꿔서 풀어야만 했던 것이다.
