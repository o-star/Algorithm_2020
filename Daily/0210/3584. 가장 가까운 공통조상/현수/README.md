# 문제
- [3584. 가장 가까운 공통 조상](https://www.acmicpc.net/problem/3584)

## 코드
``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Q3584 {
    static int n, parent[];
    static boolean cache[];
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int tc = Integer.parseInt(st.nextToken());
        while(tc-- >0){
            n = Integer.parseInt(br.readLine());
            parent = new int[n + 1]; Arrays.fill(parent, -1);
            cache = new boolean[n + 1]; Arrays.fill(cache, false);
            for (int i = 0; i < n - 1; i++) {
                st = new StringTokenizer(br.readLine());
                int p = Integer.parseInt(st.nextToken());
                int c = Integer.parseInt(st.nextToken());
                parent[c] = p;
            }
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            System.out.println(getRoot(a, b));
        }
    }

    private static int getRoot(int a, int b) {
        cache[a] = true;
        while(parent[a] != -1) {
            a = parent[a];
            cache[a] = true;
        }
        while(parent[b] != -1){
            if(cache[b]) return b;
            b = parent[b];
        }
        return a;
    }
}
/*
1
3
1 2
3 1
3 2
*/
```


## ⭐️느낀점⭐️
> 간단한 트리 조상찾기 문제였다. 문제의 조건에 두 노드를 자식으로 갖는 부모 노드를 찾으라해서 헷갈렸는데, 그냥 내가 처음에 생각한거대로 모든 노드는 자기 자신도 자식노드로 갖는 것이었다.
## 풀이 📣
<hr/>
1️⃣ 입력에 대해 부모-자식 관계를 설정해준다. 

    - int parent[] 배열을 사용해서 부모 자식 관계를 저장한다. 


2️⃣ 찾고자하는 노드에서부터 루트까지 쭉 방문하면서 `cache` 배열을 통해 체크해준다.


3️⃣ 다른 노드에서부터 루트까지 쭉 탐색하면서 `cache` 베열에 이미 체크된 것이 있으면 그 노드가 공통 노드가 된다.


4️⃣ 따라서 이미 방문한 노드를 찾아서 출력해주면 정답이 된다.

<hr/>