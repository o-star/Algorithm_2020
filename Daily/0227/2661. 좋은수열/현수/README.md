# 문제
- [2661. 좋은 수열](https://www.acmicpc.net/problem/2661)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.util.Scanner;

public class Q2661 {
    static int n, arr[];
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        arr = new int[n + 1];
        dfs(1, -1);
    }

    private static boolean dfs(int idx, int pre) {
        if(idx >= 5 && badSequence(idx - 1)) return false;
        if(idx > n){
            for (int i = 1; i <= n; ++i)
                System.out.print(arr[i]);
            System.out.println();
            return true;
        }

        for (int i = 1; i <= 3; i++) {
            if(pre == i) continue;
            arr[idx] = i;
            if(dfs(idx + 1, i))
                return true;
        }
        return false;
    }

    private static boolean badSequence(int idx) {
        int size = 2;
        while(size <= (idx / 2)) {
            boolean flag = true;
            for (int i = idx - (size - 1); i <= idx; i++) {
                if (arr[i - size] != arr[i])
                    flag = false;
            }
            if(flag) return true;
            size += 1;
        }
        return false;
    }
}
```

</details>

## ⭐️느낀점⭐️
> 처음에는 어떻게 규칙을 찾을지 고민을 했다가 규칙성을 모르겠어서 완전탐색으로 구현하니까 성공했다.
>

## 풀이 📣
<hr/>

1️⃣ 인접한 부분 수열이 똑같은 것이 2개가 나오면 안되는 조건이 핵심이다.

    - 현재 위치에 숫자를 놓아보고, 이떄까지 만들어놓은 수열이 좋은 수열을 유지하는지 확인한다.

    - 숫자를 둘 때부터 바로 전의 숫자와는 동일하지 않은 숫자를 두기 때문에 길이가 2인 부분수열부터 확인한다.


2️⃣ 부분 수열의 길이를 `현재까지 만들었던 전체 수열의 길이 / 2` 까지 증가시켜보며 인접한 두 부분수열이 일치하는지를 확인한다.

    - 현재 인덱스 - (부분 수열의 길이 - 1) 가 오른쪽 부분 수열의 시작 인덱스

    - (현재 인덱스 - (부분 수열의 길이 - 1)) - 부분 수열의 길이 가 왼쪽 부분 수열의 시작 인덱스

    - 각 부분수열의 시작 인덱스부터 길이만큼 비교해보면서 만약 두 부분수열이 일치하면 나쁜 수열임을 의미한다.

    - 오른쪽 끝에서부터 두 개의 인접한 부분 수열만 생각하면 되는 이유 : 지금까지 만들어놓은 수열은 이미 좋은 수열임이 증명되고 현재 단계까지 넘어왔기 때문. 


3️⃣ 현재 인덱스에 특정 값이 들어가도 좋은 수열을 유지한다면 계속해서 위의 과정을 반복해서 진행한다.


4️⃣ 그렇게 n 개의 수를 모두 채운다면 현재 단계까지 배열에 저장해놓은 값들을 출력한다.

<hr/>

## 실수 😅
- 규칙을 찾으려고 했지만 규칙이 없는 문제여서 찾지 못했다

- 백트랙킹을 사용해서 각 인덱스에 값을 둘 수 있는지 확인해보고 재귀 진입/탈출을 결정할 수 있었다.