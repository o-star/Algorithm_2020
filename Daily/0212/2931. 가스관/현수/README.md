# 문제
- [2931. 가스관](https://www.acmicpc.net/problem/2931)

## 코드
``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.StringTokenizer;


public class Q2931 {
    static int sx, sy, ansR, ansC, r, c, dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
    static char board[][], pipe[] = {'|', '-', '+', '1', '2', '3', '4'}, ans = ' ';
    static boolean visited[][];

    public static void main(String[] args) throws IOException {
        init();
        solution(sx, sy);
    }

    private static void solution(int x, int y) {
        for (int d = 0; d < 4; d++) {
            int nx = x + dx[d], ny = y + dy[d];
            if(!isBorder(nx, ny) || board[nx][ny] == '.') continue;
            if(dfs(nx, ny)) break;
        }
    }

    private static boolean dfs(int x, int y) {
        char deli = board[x][y];
        if(!isBorder(x, y) || visited[x][y]) return false;
        if(deli == '.'){
            ansR = x;
            ansC = y;
            ans = findPipe(x, y);
            System.out.println(ansR + " " + ansC + " " + ans);
            return true;
        }
        visited[x][y] = true;
        if(deli == '|'){
            if(dfs( x - 1, y)) return true;
            if(dfs( x + 1, y)) return true;
        }
        else if(deli == '-'){
            if(dfs( x, y - 1)) return true;
            if(dfs( x, y + 1)) return true;
        }
        else if(deli == '+'){
            for (int d = 0; d < 4; d++)
                if(dfs(x + dx[d], y + dy[d]))
                    return true;
        }
        else if(deli == '1'){
            if(dfs( x, y + 1)) return true;
            if(dfs( x + 1, y)) return true;
        }
        else if(deli == '2'){
            if(dfs( x - 1, y)) return true;
            if(dfs( x, y + 1)) return true;
        }
        else if(deli == '3'){
            if(dfs( x, y - 1)) return true;
            if(dfs( x - 1, y)) return true;
        }
        else if(deli == '4'){
            if(dfs(x + 1, y)) return true;
            if(dfs(x, y - 1)) return true;
        }
        return false;
    }

    private static char findPipe(int x, int y) {
        boolean up, right, down, left, flag = true;
        up = right = down = left = false;

        for (int d = 0; d < 4; d++) {
            int nx = x + dx[d], ny = y + dy[d];
            if(!isBorder(nx, ny)) continue;
            char deli = board[nx][ny];
            if(d == 0){
                if(deli == '|' || deli == '+' || deli == '1' || deli == '4')
                    up = true;
            }
            else if(d == 1){
                if(deli == '-' || deli == '+' || deli == '3' || deli == '4')
                    right = true;
            }
            else if(d == 2){
                if(deli == '|' || deli == '+' || deli == '2' || deli == '3')
                    down = true;
            }
            else if(d == 3){
                if(deli == '-' || deli == '+' || deli == '1' || deli == '2')
                    left = true;
            }
        }

        if(up && left && down && right) return '+';
        if(up && down) return '|';
        if(left && right) return '-';
        if(down && right) return '1';
        if(up && right) return '2';
        if(up && left) return '3';
        if(down && left) return '4';
        return 'X';
    }

    private static boolean isBorder(int x, int y) {
        return (x >= 1 && x <= r && y >= 1 && y <= c);
    }

    private static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        r = Integer.parseInt(st.nextToken());
        c = Integer.parseInt(st.nextToken());
        board = new char[r + 1][c + 1];
        visited = new boolean[r + 1][c + 1];
        for (int i = 0; i <= r; i++)
            Arrays.fill(board[i], '.');
        for (int i = 1; i <= r; i++) {
            String line = br.readLine();
            for (int j = 1; j <= c; j++) {
                board[i][j] = line.charAt(j-1);
                if(board[i][j] == 'M'){
                    sx = i;
                    sy = j;
                }
            }
        }
    }
}
```


## ⭐️느낀점⭐️
> 굉장히 빡센 구현이었다. 조건입력을 하드코딩으로 적어줘야해서 어떻게 해주면 더 효율적일지 고민을 많이한 문제이다.
> 

## 풀이 📣
<hr/>

1️⃣ 시작지점부터 dfs로 연결된 가스관을 따라 이동한다. 


2️⃣ 정해진 경로대로 이동하였는데 가스관이 없는 칸에 도달하였다면 그 블록이 사라진 블록이다.


3️⃣ 해당 블록에서부터 주변에 연결되어 있는 파이프가 있는지 확인한다.

    - 만약 네 방향(up, right, down, left) 모두 파이프가 연결되어 있다면 사라진 블록에 가스관은 + 모양일 것이다.

    - 그렇지 않다면 다른 파이프가 연결되야함을 유추해볼 수 있다.

    - 1. 좌/우 로 연결된 파이프가 있다면 ㅡ 모양

    - 2. 상/하 로 연결된 파이프가 있다면 | 모양

    - 3. 우/하 로 연결된 파이프가 있다면 r 모양

    - 4. 우/상 로 연결된 파이프가 있다면 ㄴ 모양

    - 5. 상/좌 로 연결된 파이프가 있다면 ㄴ 옆으로 뒤집은 모양

    - 6. 좌/하 로 연결된 파이프가 있다면 ㄱ 모양


4️⃣ 주변 파이프와의 연결 상태로 유추한 결과로 나타나는 파이프의 모양을 좌표와 함께 출력해준다.

<hr/>

## 실수 😅
- 파이프 모양에 따른 이동방향을 어디로 들어가느냐에 따라서 모두 컬렉션으로 저장해놨었다.

- 그 방법으로는 일단 구현이 굉장히 헷갈렸으며 정상적으로 작동하지 않았다.

- 컬렉션으로 저장하지 말고 단순히 조건대로 시뮬레이션 해보는 것도 좋은 방법이라는 것을 배웠다. 