# 문제
- [1756. 피자 굽기](https://www.acmicpc.net/problem/1756)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Q1756 {
    static int d, n, depth[], bread[];
    public static void main(String[] args) throws IOException {
        init();
        solution();
    }

    private static void solution() {
        int left = 1, right = d, ans = 987654321;
        for (int i = 1; i <= n; i++) {
            right = binSearch(left, right, bread[i]);
            if(right == -1) {
                System.out.println(0);
                return;
            }
            else ans = Math.min(ans, right);
            right -= 1;
        }
        System.out.println(ans);
    }

    private static int binSearch(int left, int right, int num) {
        int ret = -1;
        while(left <= right){
            int mid = (left + right)/ 2;
            if(num > depth[mid]) right = mid - 1;
            else {
                left = mid + 1;
                ret = mid;
            }
        }
        return ret;
    }

    private static void init() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        d = stoi(st.nextToken());
        n = stoi(st.nextToken());
        depth = new int[d + 1];
        bread = new int[n + 1];
        st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= d; i++) {
            depth[i] = stoi(st.nextToken());
            if(i != 1 && depth[i-1] < depth[i])
                depth[i] = depth[i-1];
        }
        st = new StringTokenizer(br.readLine());
        for (int i = 1; i <= n; i++) bread[i] = stoi(st.nextToken());
    }

    private static int stoi(String str) {
        return Integer.parseInt(str);
    }
}
```
</details>

## ⭐️느낀점⭐️
> 문제의 아이디어만 찾으면 구현은 쉽게 할 수 있는 것 같다.

## 풀이 📣
<hr/>

1️⃣ 위에서부터 틀의 지름에 따라 반죽을 넣을 수 있으므로 밑으로 갈 수록 아무리 넓어지더라도 위에 지름이 좁으면 들어갈 수 없다.

    - 따라서 내림차순으로 정렬해줘야한다. -> 위에 지름보다 크면 위의 지름으로 값이 뒤덮힌다.


2️⃣ 이분 탐색으로 가능한 지름의 크기 중 가장 낮은 깊이의 틀에다가 반죽을 둬야한다.

    - mid 를 움직이며 mid 보다 작거나 같을 경우 left 를 mid + 1 만큼 이동시켜준다.

    - mid 보다 클 경우 right를 mid - 1 로 움직여준다.

    - 가장 최적의 깊이를 찾아 리턴한다.


3️⃣ 더 이상 위치시킬 수 없으면 -1을 리턴하여 0을 출력하고 종료한다. 

    
4️⃣ 모든 반죽을 둘 수 있다면 마지막으로 위치시킨 높이를 출력하고 종료한다.

## 실수 😅
- 처음에 투 포인터를 사용해서 한 번 줄여진 범위에서 계속해서 범위가 줄어들면 될거라 생각했다.

- 하지만 위쪽 오븐의 지름이 더 좁고 아래쪽 오븐의 지름이 더 넓은 경우는 적용이 불가능할거라 생각했다.
  
- 그다음 이분 탐색을 떠올렸는데 마찬가지로 반죽틀의 지름을 어떻게 정렬할지 생각하지 못했다.

- 결국 핵심은 위쪽부터 오븐의 지름을 좁혀나가는 식으로 값을 바꾸는 것이었다.