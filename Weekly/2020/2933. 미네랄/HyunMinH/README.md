# 2933 풀이

## 풀이법

1. 문제를 보고 처음에 절단점 찾기 문제인가 하였으나, 굳이 절단점까지 찾을 필요는 없었다.

2. 막대기가 던져서 맞는 지점부터 dfs를 하는데, visitd에 cluster number를 저장한다.

3. 해당 지점을 비우고, 방문했던 지점들을 모두 확인해 내려갈 수 있으면 내려간다.

## 실패

1. 문제를 풀며 여러가지 경우도 많이 생각해보고, 인터넷에서 테스트 케이스도 찾을 수 있는 건 다 해서 통과하였는데, 백준에서 11%에서 막혔다.

2. 너무 아쉽고 다행히 나랑 거의 같은 로직인 코드를 발견하였고, 통과하였다 해서 1:1 비교하면서 새로 다시 짜봐야겠다.