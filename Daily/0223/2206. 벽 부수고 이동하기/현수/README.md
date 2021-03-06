# 문제
- [2206. 벽 부수고 이동하기](https://www.acmicpc.net/problem/2206)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

class Pos{
    int x, y, d;
    int crush; // 0 : 기회 x, 1 : 기회 o

    public Pos(int x, int y, int d, int crush) {
        this.x = x;
        this.y = y;
        this.d = d;
        this.crush = crush;
    }
}
public class Q2206 {
    static int n, m, dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
    static char arr[][];
    private static boolean[][][] visited;

    public static void main(String[] args) throws IOException {
        init();
        solution();
    }

    private static void solution() {
        Queue<Pos> q = new LinkedList<>();
        q.add(new Pos(1, 1, 1,1));
        visited[1][1][0] = visited[1][1][1] = true;
        int ans = 987654321;

        while (!q.isEmpty()) {
            Pos here = q.poll();
            int x = here.x, y = here.y, d = here.d, crush = here.crush;
            visited[x][y][crush] = true;
            if(x == n && y == m){
                ans = Math.min(ans, d);
                continue;
            }
            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i], ny = y + dy[i];
                if(!isBorder(nx, ny) || visited[nx][ny][crush])
                    continue;

                if(arr[nx][ny] == '1'){
                    if(crush == 1) {
                        visited[nx][ny][crush] = true;
                        q.add(new Pos(nx, ny, d + 1, 0));
                    }
                    continue;
                }
                visited[nx][ny][crush] = true;
                q.add(new Pos(nx, ny, d + 1, crush));
            }
        }
        if (ans == 987654321)
            System.out.println(-1);
        else {
            System.out.println(ans);
        }
    }

    private static boolean isBorder(int x, int y) {
        return (x >= 1 && x <= n && y >= 1 && y <= m);
    }

    private static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        arr = new char[n + 1][m + 1];
        visited = new boolean[n + 1][m + 1][2];
        for (int i = 1; i <= n; i++) {
            String line = br.readLine();
            for (int j = 1; j <= m; j++) {
                arr[i][j] = line.charAt(j - 1);
            }
        }
    }
}
```

</details>

## ⭐️느낀점⭐️
> 사소한 예외 처리를 간과하지 말자 -> 입력이 1 1 0인 경우
>
> 로직이 맞는데 틀릴 경우, 변수 선언 부분도 검토해보자 -> 3차원 visited ! 


<hr/>

## 풀이 📣


1️⃣ x, y 좌표, 이동 횟수, 벽을 부술 수 있는지를 저장하는 클래스 객체를 사용한다.

    - 벽 부술 수 있는지 여부는 정수형태로 0과 1로 저장.


2️⃣ BFS로 4가지 방향을 탐색하면서 벽을 부수고 이동해야할 경우에는 0으로 표시 후 큐에 저장한다.

    - 그렇지 않을 경우 큐에 저장되어 있던 값 그대로 좌표와 이동횟수만 변경해서 저장한다.

    - 방문 체크 : visited[x][y][벽 부수기 가능 여부]


3️⃣ 목표 지점에 도달하는 경우 가장 최소값이 될 수 있도록 비교 후 답을 저장해준다.

    - 이전에 도달한 적이 있어도 현재 도달할 때 까지 걸린 이동횟수와 비교연산을 해준다. 

<hr/>

## 실수 😅
- 벽 부수기라는 조건이 추가되었지만 방문체크를 2개의 조건(x, y) 에 대해서만 진행해서 실패

- 여태 BFS를 nx, ny 로 목표지점 확인을 했었는데, 그럼 `1 1 0`과 같은 입력에서는 올바른 답을 못구한다.

- 이제 큐에서 꺼낸 직후에 `x`, `y` 에 대해서 목표 지점을 확인해야겠다.