# 문제
- [3665. 최종 순위](https://www.acmicpc.net/problem/3665)

## 코드
``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Scanner;
import java.util.StringTokenizer;

public class Q3665 {
    final static int INF = 987654321;
    static int n, m, arr[], inDegree[];
    static List<Integer> order[];
    static Scanner sc = new Scanner(System.in);
    public static void main(String[] args){
        int tc = sc.nextInt();
        while(tc-- > 0) {
            if(init())
                solution();
        }
    }

    private static void solution() {
        int head = 0;
        for (int i = 1; i <= n; i++) {
            if(inDegree[i] == 0) {
                head = i;
                break;
            }
        }
        Queue<Integer> q = new LinkedList<>();
        List<Integer> ans = new ArrayList<>();
        if(head != 0) {
            ans.add(head);
            q.add(head);
        }
        while (!q.isEmpty()) {
            if(q.size() >= 2){
                System.out.println("?");
                return;
            }
            head = q.poll();
            for (int i = 0; i < order[head].size(); i++) {
                int next = order[head].get(i);
                inDegree[next] -= 1;
                if(inDegree[next] == 0){
                    q.add(next);
                    ans.add(next);
                }
            }
        }
        // 방향 그래프에서 사이클 : 진입차수가 0인 지점이 더 이상 없는 경우 -> 모든 지점 탐색 불가
        if(ans.size() != n){
            System.out.println("IMPOSSIBLE");
            return;
        }
        for (Integer an : ans) {
            System.out.print(an + " ");
        }
        System.out.println();
    }

    private static boolean init() {
        n = sc.nextInt();
        arr = new int[n + 1];
        order = new List[n + 1];
        inDegree = new int[n + 1];
        for (int i = 1; i <= n; i++)
            order[i] = new ArrayList<>();

        // original rank
        for (int i = 1; i <= n; i++) {
            arr[i] = sc.nextInt();
            inDegree[arr[i]] = i - 1;
        }
        for (int i = 1; i <= n - 1; i++) {
            for (int j = i + 1; j <= n; j++) {
                order[arr[i]].add((Integer)arr[j]);
            }
        }

        // changed rank
        m = sc.nextInt();
        for (int i = 0; i < m; i++) {
            Integer pre = sc.nextInt();
            Integer post = sc.nextInt();
            int preIdx = getIndex(pre), postIdx = getIndex(post);
            if(preIdx == INF || postIdx == INF) {
                System.out.println("IMPOSSIBLE");
                return false;
            }

            if(preIdx < postIdx){
                order[pre].remove(post);
                order[post].add(pre);

                inDegree[pre] += 1;
                inDegree[post] -= 1;
            }
            else{
                order[pre].add(post);
                order[post].remove(pre);

                inDegree[pre] -= 1;
                inDegree[post] += 1;
            }
        }
        return true;
    }

    private static int getIndex(int num) {
        for (int i = 1; i <= n; i++) {
            if(arr[i] == num)
                return i;
        }
        return INF;
    }

    private static int stoi(String str) {
        return Integer.parseInt(str);
    }
}
/*
6
2
1 2
0
2
1 2
1
1 2
3
3 1 2
0
3
3 1 2
1
1 2
3
3 1 2
1
1 3
3
3 1 2
2
1 2
1 3
 */
```


## ⭐️느낀점⭐️

> 그래프에서 순서를 푸는 문제는 위상 정렬로 풀면 쉬운 것 같다.
>
> 처음에 순위를 정할 수 없는 경우가 뭔지 이해가 안되서 조금 헤맸다.
> 
> 그런 경우도 일단 진행시키다 보니까 안되는 경우가 어떤건지 깨달았다. -> 사이클 생성하는 경우

## 풀이 📣

1️⃣ 지난번 대회에서 얻었던 순위대로 입력을 받아서 순위가 높은 팀에서 낮은 팀으로 간선을 연결해준다.

    - 이 때, 순위가 높은 팀은 자신보다 순위가 낮은 팀들 모두에 간선을 연결해준다.

    - 왜냐하면 위상정렬을 할 때 모든 팀은 각각 진입차수를 갖는데, 특정 팀의 순서가 뒤바뀌더라도 기존에 팀 순서가 여전히 유효하기 때문이다.


2️⃣ 순서가 역전되는 정보를 입력받는다.

    - 순위가 낮아지는 팀의 진입차수를 늘려주고, 순위가 높아지는 팀으로의 간선을 제거해준다.

    - 순위가 높아지는 팀의 진입차수를 낮춰주고, 순위가 낮아지는 팀으로의 간선을 추가해준다.


3️⃣ 진입차수가 0인 팀부터 큐에 넣고 탐색을 시작한다.

    - 해당 팀에서 가지고있는 다른 팀으로의 간선을 모두 살펴본다.

    - 연결된 팀의 진입차수를 1 만큼 감소시켜준다.

    - 만약 진입차수가 0이 된다면 큐와 정답을 저장하는 리스트에 해당 순서를 저장해준다.

    - 큐가 비어있지 않다면 계속해서 탐색을 진행한다.

4️⃣ 모든 노드를 탐색하였다면 위상정렬된 순서대로 출력해준다.

    - 순서가 보관된 리스트를 순차적으로 출력해준다.


5️⃣ 확실한 순위를 찾을 수 없다는 것은 진입차수가 0인 지점이 동시에 여러 개가 존재하는 경우다.

    - 이렇게 될 경우 어디를 먼저 진입하더라도 조건에 위배되지 않는다.

    - 따라서 여러 답안이 존재하게 되는데, 이럴 경우 팀 간에 확실한 순서를 찾을 수 없게 된다.

    - 이 경우 ? 을 출력하고 종료한다.


6️⃣ 모든 노드를 탐색할 수 없다면 탐색 중간에 사이클이 발생해서 진입차수가 0이 아닌 팀이 있었다는 것이다.

    - 따라서 해당 경우는 순서를 명확히 정의할 수 없기 때문에 조건에 따라 "IMPOSSIBLE" 을 출력하고 종료해준다.

<hr/>

## 실수 😅

- 처음에는 일관성이 없어 순서를 정의할 수 없는 경우가 뭔지 몰랐다.

- 근데 디버깅하다보니까 아직 다 탐색하지 않았는데 큐가 비어서 더이상 꺼내오지 못하는 현상을 발견하고 수정할 수 있었다.