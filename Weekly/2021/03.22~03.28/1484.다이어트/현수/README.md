---
title: '[BOJ] 1484. 다이어트'
metaTitle: '1484. 다이어트'
metaDescription: '알고리즘 문제를 풀고 정리한 곳입니다.'
tags: ['투 포인터', '수학']
date: '2021-03-23'
---

# 문제
- [2213. 트리의 독립집합](https://www.acmicpc.net/problem/2213)

## 코드

<details><summary> 코드 보기 </summary>

``` java
import java.util.Scanner;

public class Q1484 {
    static int n;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        long lo = 1, hi = 1, ans = 0;
        while(!(lo == hi - 1 && diff(lo, hi) > n)){
            if(diff(lo, hi) < n) hi += 1;
            else lo += 1;

            if (diff(lo, hi) == n) {
                System.out.println(hi);
                ans += 1;
            }
        }
        if (ans == 0)
            System.out.println(-1);
    }

    private static long diff(long lo, long hi) {
        long h = hi, l = lo;
        return (h * h) - (l * l);
    }
}

```

</details>

## ⭐️느낀점⭐️
<hr/>

> 처음에 투 포인터를 떠올리지 못했지만 간단한 투 포인터 문제였다.
> 
> 언제 종료가 되는지 고민을 하다가 투 포인터를 설정하고 더 이상 진행이 불가능 할 때의 조건을 찾아냈다.

## 풀이 📣
<hr/>

1️⃣ 현재 몸무게를 `hi`, 기억하는 몸무게를 `lo` 로 설정한다. 


2️⃣ 설정한 투 포인터를 가지고 조건을 만족하는지 탐색한다.

    - hi 를 한 칸씩 움직이다가 hi^2 - lo^2 가 G 보다 커지면 lo를 증가시켜준다.

    - hi^2 - ㅣo^2 가 G 이면 그 때의 hi 값을 출력한다.

    - hi^2 - ㅣo^2 < G 이면 hi 를 한 칸 움직여 준다.


3️⃣ 종료 조건을 만족하면 더 이상 탐색을 종료한다.

    - G = hi^2 - lo^2, (G <= 100,000) 의 제약 사항을 따라야 한다.

    - 그렇다면 lo 가 hi - 1 일 때, 더이상 hi^2 - lo^2 값을 줄일 수 없다.

    - 그 때 hi^2 - lo^2 가 100,000 보다 크다면 더 이상 탐색이 불가능하다

    - 왜냐면 lo 를 더 증가시킬 수 없어서 hi 를 증가시킬 수 밖에 없는데, 그렇게되면 hi^2 - lo^2 값이 더 커지기 때문이다.


4️⃣ 만약 조건을 만족하는 경우가 없으면 `-1`을 출력하고 종료한다.


## 실수 😅
<hr/>

- 최대 독립 집합의 크기와 포함되는 원소들을 한 번에 구하려고 했는데 한 쪽을 생각하고 짜면 다른 쪽이 막히는 등의 문제점이 발생했다.


- 결국 두 가지를 분리해서 따로 구해서 출력해야만 했다.