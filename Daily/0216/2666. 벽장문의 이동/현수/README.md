# 문제
- [2666. 벽장문의 이동](https://www.acmicpc.net/problem/2666)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Q2666 {
    static int n, a, b, m, arr[], cache[][][];
    public static void main(String[] args) throws IOException {
        init();
        System.out.println(solution(a, b, 0));
    }

    static int solution(int x, int y, int count) {
        if(count >= m) return 0;
        if(cache[count][x][y] != -1) return cache[count][x][y];
        return cache[count][x][y] = Math.min(
                Math.abs(x - arr[count]) + solution(arr[count], y, count + 1),
                Math.abs(y - arr[count]) + solution(x, arr[count], count + 1));
    }

    static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        st = new StringTokenizer(br.readLine());
        a = Integer.parseInt(st.nextToken());
        b = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(br.readLine());
        arr = new int[m]; cache = new int[m][n + 1][n + 1];
        for (int i = 0; i < m; i++)
            for (int j = 0; j <= n; j++) {
                Arrays.fill(cache[i][j], -1);
            }

        for (int i = 0; i < m; i++)
            arr[i] = Integer.parseInt(br.readLine());
    }
}
/*
8
3 8
2
5
1
 */
```

</details>

## ⭐️느낀점⭐️
> DP는 계속 풀어도 계속 어렵다..ㅜㅜ


<hr/>

## 풀이 📣

1️⃣ 열려 있는 두 지점을 인덱스로 가지는 3차원 캐시를 이용한다.

    - cache[타겟 문][왼쪽 열린 문][오른쪽 열린 문] = 왼쪽과 오른쪽 문이 열려있을 때 타겟 문을 열기 위한 최소 이동 횟수


2️⃣ 목표로하는 위치로 이동을 시키는데, 가능한 경우는 왼쪽에서 옮기는 경우와 오른쪽에서 옮기는 경우 2가지가 존재한다.

    - 왼쪽을 옮길 경우 - (target, y)

    - 오른쪽을 옮길 경우 - (x, target)

    - 두 가지 경우에 타겟 인덱스를 하나 증가시켜서 재귀적으로 탐색한다.
    
    - 현재 이동한 횟수는 각 인덱스의 변화량이 된다. - 각각 abs(x - target), abs(y - target)


3️⃣ 모든 가능한 위치에서 최소 이동 횟수를 계산하여 최종적으로 최소 이동 총 횟수를 출력한다.

<hr/>

## 실수 😅

- 문이 열려있는 위치를 일차원 배열로 저장해서 각 지점을 `left`와 `right` 로 포인터를 놓고 풀었었는데, 완전탐색이 안되었다.

- 1차원 배열 + 포인터 -> 2차원 배열 인덱스로 나타낼 수 있었는데 이걸 몰랐다.