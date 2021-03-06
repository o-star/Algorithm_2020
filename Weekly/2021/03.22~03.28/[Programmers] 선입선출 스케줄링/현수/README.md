---
title: '선입 선출 스케줄링'
metaTitle: '만렙 개발자 키우기'
metaDescription: '알고리즘 문제를 풀고 정리한 곳입니다.'
tags: ['이분 탐색']
date: '2021-04-02'
---

# 문제
- [선입 선출 스케줄링](https://programmers.co.kr/learn/courses/30/lessons/12920?language=java)

## 코드

<details><summary> 코드 보기 </summary>

``` java
class Solution {
    static int cores[], n;
    public int solution(int N, int[] Cores) {
        cores = Cores;
        n = N;

        // 시작부터 모든 코어에 배치시킬 수 있는 경우
        if(n <= cores.length) return N;

        int answer = 0, time = 0;
        int minTime = 987654321, maxTime = 0;
        for (int core : Cores) {
            minTime = Math.min(minTime, core);
            maxTime = Math.max(maxTime, core);
        }

        // 이분탐색 범위 최적화.
        int lo = (minTime * n) / cores.length - minTime;
        int hi = (maxTime * n) / cores.length - maxTime;
        while(lo <= hi){

            // 모든 작업을 처리하는데 걸리는 lower_bound
            int mid = (lo + hi) / 2;
            if(check(mid)) {
                hi = mid - 1;
                time = mid;
            }
            else lo = mid + 1;
        }

        // 모든 작업을 처리하는데 걸리는 시간에서 1시간 전의 상황에 몇 개까지 처리했는지 계산
        int cnt = cores.length, preTime = time - 1;
        for(int core : cores)
            cnt += (preTime) / core;

        // 마지막 시간이 되었을 때 몇 번째 코어에서 작업이 완료되는지 확인.
        for(int i = 0; i < cores.length; ++i){
            if(time % cores[i] == 0)
                cnt += 1;
            if(cnt == n){
                answer = i+1;
                break;
            }
        }
        return answer;
    }
    public boolean check(int value){
        int cnt = 0;
        for(int core: cores){
            cnt += (value/core);
        }
        return cnt >= n - cores.length;
    }
}
```
</details>

## ⭐️느낀점⭐️
> 비슷한 유형의 문제를 풀어봐서 쉽게 이해할 수 있었다.
> 
> 하지만 이분 탐색의 최대 시간을 어떻게 잡냐에 따라서 답이 달라진다는 사실을 처음 알았다.
> 
> 최대가 될 수 있는 값보다 큰 값으로 설정하는 것은 어차피 이분 탐색을 진행하면서 줄여나가기 때문에 괜찮을 거라 생각했었다..

## 풀이 📣

1️⃣ 모든 작업을 처리하는데 걸리는 시간의 `lower_bound`를 이분 탐색으로 찾아낸다.

    - 다음과 같이 이분 탐색의 범위를 최적화할 수 있다. => 모든 코어가 가장 빠른 작업 처리 시간을 가지면 그때가 최소값, 가장 느린 작업 처리 시간을 가지면 그때가 최대값!

    - 최소값 : 코어의 작업 처리시간 중 가장 빠른 코어의 작업시간을 구한다. 그리고 다른 모든 코어들도 동일한 작업시간을 갖는다고 가정한다.

    - lo = (minTime * n) / cores.length - minTime // 최소 작업 처리 시간으로 모든 작업을 처리하는데 걸리는 시간을 구하고 한 번에 모든 코어에서 각 작업을 처리하므로 cores.length 로 나눠준다.

    - 마지막에 minTime 을 빼주는 이유 : 시작과 동시에 모든 코어에 작업을 맡기기 때문에 첫 minTime은 세어주지 않는다.

    - 최대값 : 코어의 작업 처리시간 중 가장 느린 코어의 작업시간을 구한다. 그리고 다른 모든 코어들도 동일한 작업시간을 갖는다고 가정한다.

    - hi = (maxTime * n) / cores.length - maxTime // 최대 작업 처리 시간으로 모든 작업을 처리하는데 걸리는 시간을 최소시간을 구했던 것과 같이 구해준다.


2️⃣ 모든 작업을 처리하기 1시간 전까지 처리했던 작업의 수를 계산한다.

    - preTime = time - 1 // 모든 작업 처리하기 1시간 전

    - cnt += preTime / cores[i] // 해당 시간동안 각 코어마다 몇 개의 작업을 처리했는지 


3️⃣ 마지막 시간이 되었을 때, 마지막 작업을 처리하는 코어를 찾아낸다.

    - if(time % cores[i] == 0) // 마지막 시간에 i번째 코어가 작업을 처리할 수 있는지

    - if(cnt == n) // 마지막 작업을 처리하였다면 i + 1 리턴 (코어가 0부터 인덱스되어 있어서 1 더함)


## 실수 😅

- 최대 작업 시간을 설정하는 부분에서 50000 * 10000 으로 했는데 몇 개의 케이스가 통과하지 못했다.

- 올바른 탐색 범위를 설정하지 못했다!