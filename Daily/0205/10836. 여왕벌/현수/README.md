# 문제
- [10836. 여왕별](https://www.acmicpc.net/problem/10836)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.io.*;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Q10836 {
    static int m, n, worm[][];
    public static void main(String[] args) throws IOException {
        // init
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        m = Integer.parseInt(st.nextToken());
        n = Integer.parseInt(st.nextToken());
        worm = new int[m][m];
        for (int i = 0; i < m; i++) Arrays.fill(worm[i], 1);

        // calc
        int grow[] = new int[3], growAround[][] = new int[m][m];
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < 3; j++)
                grow[j] = Integer.parseInt(st.nextToken());
            fillGrowAround(growAround, grow);
        }
        solution(growAround);
    }

    private static void solution(int growArround[][]) throws IOException {
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                if(i == 0 || j == 0)
                    bw.write((growArround[i][j] + 1) + " ");
                else
                    bw.write((growArround[0][j] + 1) + " ");
            }
            bw.newLine();
        }
        bw.close();
    }

    private static void fillGrowAround(int[][] growAround, int grow[]) {
        int row = m - 1, col = 0;
        for (int growth = 0; growth < 3; growth++) {
            int count = grow[growth], idx = 0;
            if(count == 0) continue;
            for (idx = 0; idx < count && row > 0; idx++)
                growAround[row--][0] += growth;
            for (int i = idx; i < count; i++)
                growAround[0][col++] += growth;
        }
    }
}
```

</details>

## ⭐️느낀점⭐️
> 문제의 조건을 따져보고 알고리즘을 설계해야했는데, 그냥 보자마자 "아, 시뮬레이션 + 구현이구나!" 하고 덤벼들었다가 시간초과가 떴다.
>
> 이제 문제를 읽고 알고리즘 유형을 떠올릴 순 있는데 조건 판별에 신경을 더 써야겠다.

## 풀이 📣
<hr/>

1️⃣ 가장 왼쪽 열과 가장 위쪽 행의 성장속도가 입력으로 주어진다.

    - 가장 왼쪽 아래부터 차례대로 0, 1, 2 의 성장속도가 주어지는데, 오름차순으로 진행되며 각 성장량의 횟수가 주어진다.

    - 여기서 가장 핵심 조건인 "오름차순"에 주목한다. 

2️⃣ 그 외의 애벌레의 성장속도는 `왼쪽`, `왼쪽 위`, `위쪽` 애벌레의 성장속도 중 가장 큰 값을 가진다.

    - 하지만 왼쪽 끝, 위쪽 끝의 애벌레는 성장속도가 오름차순으로 주어지므로 !반드시! 자신의 위쪽 애벌레의 성장속도가 가장 클 수밖에 없다.

    - 따라서 나머지 비교 연산을 할 필요가 없다.

3️⃣ 시간이 경과할 수 있는 일수가 ` 1 <= N < 1000,1000` 이므로 매일 매일 변화를 저장하면 안된다.

    - 매일 저장하고 값을 갱신하면 700 * 700 의 크기를 1000,000 번 바꿔야해서 당연히 시간초과가 날 수 밖에 없다.

    - 따라서 경과하는 날짜 수만큼 성장속도에 따라 총 성장량을 저장해서 보관한다.

4️⃣ 총 성장량을 각 애벌레에게 적용시켜주는데, 시작이 1 이었으므로 +1 만큼 한 값을 모두 출력해주면 된다.

<hr/>
- 시간초과를 예상하지 못하고 단순 구현을 해서 시도했다.

- 시간을 줄이기위해서 `BufferedWriter` 도 사용해보았으나 효과는 미미했다!

- 결국 주요 로직을 수행하는 반복문을 어떻게 줄일 수 있을까 고민하였고, 모든 계산을 그때마다 해야할 필요가 없다는 것을 깨달았다.