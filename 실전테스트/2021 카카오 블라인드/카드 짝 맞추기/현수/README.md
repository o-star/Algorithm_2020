# 문제
- [카카오 2021 블라인드 공개 채용. 6번 카드 짝 맞추기](https://programmers.co.kr/learn/courses/30/lessons/72415)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.awt.Point;
import java.util.Arrays;
import java.util.PriorityQueue;

class Route{
    int u, v, cost;

    public Route(int u, int v, int cost) {
        this.u = u;
        this.v = v;
        this.cost = cost;
    }
}
public class Solution {
    static int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
    static boolean pictures[] = new boolean[8];
    public static int solution(int[][] board, int r, int c) {
        int answer = getAnswer(board, r, c);
        return answer;
    }

    private static int getAnswer(int[][] board, int r, int c) {
        if(finished(board)) return 0;
        int ret = 987654321;
        for (int num = 1; num <= 6; num++) {
            Point head = null, tail = null;
            for (int i = 0; i < 4; i++) {
                for (int j = 0; j < 4; j++) {
                    if(board[i][j] == num){
                        if(head == null) head = new Point(i, j);
                        else tail = new Point(i, j);
                    }
                }
            }
            // 해당 숫자는 이미 제거됨
            if(head == null) continue;

            // 앞에꺼 먼저 뒤집기 + 각 카드마다 Enter
            int headDist = getDist(r, c, head.x, head.y, board) +
                getDist(head.x, head.y, tail.x, tail.y, board) + 2;

            // 뒤에꺼 먼저 뒤집기 + 각 카드마다 Enter
            int tailDist = getDist(r, c, tail.x, tail.y, board) +
                getDist(tail.x, tail.y, head.x, head.y, board) + 2;

            // 뒤집은 카드 제거
            board[head.x][head.y] = board[tail.x][tail.y] = 0;

            int next = Math.min(headDist + getAnswer(board, tail.x, tail.y),
                                tailDist + getAnswer(board, head.x, head.y));
            ret = Math.min(ret, next);

            // 뒤집은 카드 원상복구
            board[head.x][head.y] = board[tail.x][tail.y] = num;
        }
        return ret;
    }

    private static boolean finished(int[][] board) {
        for (int[] row : board)
            for (int v : row)
                if(v != 0) return false;
        return true;
    }

    private static int getDist(int x, int y, int px, int py, int board[][]) {
        // 4가지 방향으로 ctrl 키를 눌러서 이동해보면서,
        // 가장 적은 비용으로 목적지에 도달하는 경우를 저장한다.

        PriorityQueue<Route> pq = new PriorityQueue<>((a, b) -> (a.cost - b.cost));
        pq.add(new Route(x, y, 0));

        int dist[][] = new int[4][4];
        for (int i = 0; i < 4; i++)
            Arrays.fill(dist[i], 987654321);
        dist[x][y] = 0;

        while (!pq.isEmpty()) {
            Route e = pq.poll();
            if(e.cost > dist[e.u][e.v]) continue;
            if(e.u == px && e.v == py)
                return e.cost;

            for (int d = 0; d < 4; d++) {
                int nx = e.u, ny = e.v, moveCnt = 0;

                while(isBorder(nx + dx[d], ny + dy[d])) {
                    nx += dx[d]; ny += dy[d];
                    moveCnt += 1;

                    // 다른 숫자 카드 발견
                    if(board[nx][ny] != 0) break;

                    // Ctrl 키를 누르지 않고 하나씩 이동하는 경우도 카운트 해준다.
                    if(dist[nx][ny] > e.cost + moveCnt){
                        dist[nx][ny] = e.cost + moveCnt;
                        pq.add(new Route(nx, ny, dist[nx][ny]));
                    }
                }
                // 카드 or 벽을 만나면 Ctrl 키를 눌러 한번에 이동 가능
                if(dist[nx][ny] > e.cost + 1){
                    dist[nx][ny] = e.cost + 1;
                    pq.add(new Route(nx, ny, dist[nx][ny]));
                }
            }
        }

        return 999999;
    }

    private static boolean isBorder(int x, int y) {
        return (x >= 0 && x < 4 && y >= 0 && y < 4);
    }

    public static void main(String[] args) {
        int board1[][] = {
            {1,0,0,3},
            {2,0,0,0},
            {0,0,0,2},
            {3,0,1,0}
        };
        int board2[][] = {
            {3,0,0,2},
            {0,0,1,0},
            {0,1,0,0},
            {2,0,0,3}
        };

        System.out.println(solution(board1, 1, 0));
        System.out.println(solution(board2, 0, 1));
    }
}
```

</details>

## ⭐️느낀점⭐️
> 단순 구현 문제라서 안심하고 풀었는데, 역시 구현도 쉽지 않았다..!
> 
> 로직을 이해하고 구현을 하려는데 계속 복잡해지고 꼬였다. 처음부터 깔끔하게 짤 수 있도록 설계했으면 풀이 자체는 어렵지 않았을텐데ㅜㅜ
>
> 컨트롤 누르고 이동하는 부분이 최대 난관이었다. 이걸 다익스트라를 사용할 줄이야

## 풀이 📣
<hr/>

1️⃣ 현재 놓여있는 위치에서 모든 숫자 카드를 한 번씩 뒤집어 본다. 

    - 가깝고 멀고를 따지지 않고 모든 숫자 카드를 찾아서 뒤집어본다.

    - 숫자마다 총 2개의 카드가 있으므로, 둘 중에 어떤 것을 먼저 뒤집냐에 따라서 또 정답이 달라진다.

    - 따라서 두 가지 경우로 나눠서 더 적은 비용이 드는 답안을 백트래킹으로 계속해서 찾아나간다.


2️⃣ 카드를 뒤집으려면 그 카드가 놓여있는 위치로 이동해야 하는데, `Ctrl` 키의 사용 여부에 따라 최소비용이 달라진다.

    - Ctrl 키를 사용하는 경우가 최소비용이 될 수도 있고, 사용하지 않는 경우가 최소비용이 될 수도 있다.

    - 따라서 두 경우를 모두 파악하기 위해 다익스트라 알고리즘을 사용해서 각 지점까지의 최소비용을 갱신해나간다.

    - 만약 벽을 만나거나 다른 카드를 만나면, 그 위치까지 Ctrl 키를 누르고 한 번만에 도달할 수 있다.

    - 그렇지 않다면 특정 방향으로 한 칸씩 계속 이동하면서 이동 횟수를 증가시키며 최소비용을 갱신해나간다.

    - 벽 또는 다른 숫자 카드에 부딪힌다면 해당 위치까지 최소 비용과, 현재까지 최소비용에 +1 해준 값 중 더 작은 값으로 갱신해주고 큐에 다시 넣어준다.


3️⃣ 큐에서 꺼낸 최소비용의 위치가 저장되어있는 장소가 목적지에 도달할 때 까지 위의 과정을 반복한다. 

    - 목적지에 도달하면 그 때까지의 저장된 최소비용을 반환한다.    


4️⃣ 선택한 숫자를 모두 뒤집어서 제거하면, `board` 에서 해당 숫자카드를 0으로 지워주고 마지막으로 놓인 위치에서부터 다시 새로운 카드를 제거해나간다.

    - 선택한 숫자를 제거하기 위해서는 Enter 키를 각각 한 번씩 눌러줘야하므로 비용에 +2 를 해줘야한다.

    - 다시 백트래킹을 끝내고 돌아오면 지워줬던 숫자카드를 원래 숫자로 원상복구 해줘야한다.


5️⃣ 위의 과정(백트래킹 + 다익스트라)을 계속 반복해서 최종적으로 구할 수 있는 최소비용을 찾아 출력한다.

<hr/>

## 실수 😅
- 먼저 특정 위치까지 `Ctrl` 사용 여부에 따라 최소 비용으로 이동하는 부분을 구현하지 못했다. 다익스트라 알고리즘을 적용하는 방법이 있을 줄은 몰랐다..!

- 그 부분을 계속해서 단순하게 구현하려고만 해서 이것저것 고려해야할 점들이 많아서 생각이 너무 꼬여버렸다..

- 백 트래킹 부분에서도 설계를 어렵게해서 BFS로 현재위치에서 가장 가까운 숫자 카드 후보들을 선택해서 각 경우마다 나올 수 있는 최소비용을 찾으려고 했다.

- 여러 가지 경우의 수로 분기될 수 있어서 점점 더 코드는 복잡해져 가고 나의 멘탈은 터져나갔다..

- 가장 효율적인 방법도 좋지만, 내가 쉽게 생각할 수 있고 편하게 코드를 짤 수 있는 방법을 생각하려고 노력해야겠다
  
- (이번 문제에서는 그냥 현 위치에서 모든 숫자들을 한 번씩 다 뒤집어 보며 백트래킹하기)