# 문제
- [1043. 거짓말](https://www.acmicpc.net/problem/1043)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;

public class Q1043 {
    static int n, m, k, adj[][];
    static List<Integer> knowing = new ArrayList<>();
    static List<Integer> party[];
    public static void main(String[] args) {
        init();
        solution();
    }

    private static void solution() {
        makeFloyd();

        int ans = 0;
        for (List<Integer> p : party) {
            boolean flag = false;
            for (Integer person : p) {
                for (int i = 0; i < k; i++) {
                    if(person == knowing.get(i) || adj[person][knowing.get(i)] != 987654321) {
                        flag = true;
                        break;
                    }
                }
                if(flag) break;
            }
            if(!flag) ans += 1;
        }
        System.out.println(ans);
    }

    private static void makeFloyd() {
        for (List<Integer> p : party) {
            for (int i = 0; i < p.size() - 1; i++) {
                for (int j = i + 1; j < p.size(); j++) {
                    int u = p.get(i), v = p.get(j);
                    adj[u][v] = adj[v][u] = 1;
                }
            }
        }

        for (int k = 1; k <= n; k++) {
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    if(i == j) continue;
                    if(adj[i][j] > adj[i][k] + adj[k][j])
                        adj[i][j] = adj[i][k] + adj[k][j];
                }
            }
        }
    }

    private static void init() {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        adj = new int[n + 1][n + 1];
        m = sc.nextInt();
        k = sc.nextInt();
        party = new List[m];
        for (int i = 0; i <= n; i++)
            Arrays.fill(adj[i], 987654321);
        for (int i = 0; i < m; i++)
            party[i] = new ArrayList<>();
        for (int i = 1; i <= k; i++)
            knowing.add(sc.nextInt());

        int ans = 0;
        for (int i = 0; i < m; i++) {
            int c = sc.nextInt();
            for (int j = 0; j < c; j++)
                party[i].add(sc.nextInt());
        }
    }
}
```

</details>

## ⭐️느낀점⭐️
> 그래프에서 노드간에 연관관계를 따지기 위해서 플로이드-와샬 알고리즘을 사용하여 연관되는 경우 최단경로를 구해놓음으로써 표시하였다.

## 풀이 📣
<hr/>

1️⃣ 각 파티별로 참가하는 인원을 모두 입력받은 후, 모든 인원간에 연관관계를 정리한다.

    - 만약 한 파티에 a, b, c가 참가하면 a는 b,c 로 향하는 경로가 존재하고, b도 a, c로 향하는 경로가 존재하며, c도 a, b로 향하는 경로가 존재한다.

    - 따라서 이런 경로를 나타내기 위해 인접행렬을 이용한다.


2️⃣ 모든 연관관계가 정리된 후, 각 파티별로 모든 인원이 진실을 알고 있는 사람들과 연관관계가 있는지 확인한다. (두 사람간에 경로가 존재하는지 확인) 

    - 두 노드 간에 최단경로가 존재하는지 확인한다.

    - 만약 경로가 존재하면 진실을 알고 있으므로 해당 파티는 카운트하지 않는다.

    - 만약 파티의 모든 인원이 진실을 알고 있는 사람과의 경로가 존재하지 않으면 진실을 아는 사람과 접촉하지 않았다는 의미이므로 진실을 모르기때문에 해당 파티는 카운트해준다. 


3️⃣ 진실을 알고있는 사람과의 접촉이 없었던 사람들로 구성된 파티의 개수를 출력한다.

<hr/>

## 실수 😅