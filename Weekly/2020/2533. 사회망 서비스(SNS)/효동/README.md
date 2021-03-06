# 2533. 사회망 서비스(SNS)

# 풀이법

1. DP를 풀어본 적이 거의 없어서 아무런 감이 안잡혔다. 우선 처음에는 트리를 level로 나눴을 때 한 줄씩 번갈아서 얼리어답터인 경우를 생각해보았다. 그럼 두 가지 경우가 나올 것이고 그 중 얼리어답터 수가 적은 경우가 답일 것이라고 생각하였다. 그래서 bfs로 해서 depth를 저장해서 level이 바뀔 때 마다 +1씩 해주면서 depth가 짝수인 경우와 홀수인 경우를 구해서 min을 구하였다.(1차시도-실패)

2. 곰곰히 생각해본 결과 자식노드쪽으로 갈때는 무조건 부모나 자식 중 하나만 얼리어답터가 되어야할 뿐, 무조건 level순으로 얼리어답터 이거나 아니거나가 아니라는 사실을 깨달았다. 그래서 DP라고 생각하였고 각 노드마다 자신이 얼리어답터이거나 아니거나인 상황을 구해서 저장해서 최종 답을 구해야한다고 생각하였다.

![KakaoTalk_20200716_010649042](https://user-images.githubusercontent.com/54053016/87568570-d4108000-c700-11ea-927b-b8ffb5e24f54.jpg)

3. 최상위 부모노드는 아무 노드나 될 수 있어서 그냥 1로 하기로 생각하였다. 문제에 각 정점은 1부터 N까지 표현된다고 하였기 때문에 무조건 1은 있기 때문이었다.

4. D를 채워나가는 형식을 결정하는데에 시간이 많이 걸렸다. 부모에서 자식으로 채워나간다고 생각하니 목적지가 없었다.
그래서 자식에서 부모쪽으로 채운다고 생각을 해보니
1) 자식이 얼리어답터가 아닌경우 부모는 무조건 얼리어답터이다.
2) 자식이 얼리어답터일 경우 부모는 얼리어답터이여도 되고 아니어도 된다.

두 가지 경우가 나왔다.

그러면 밑에서부터 위로 돌면서 순회해야하는데 dfs가 최적이라고 생각하였다.

5. 생각한 대로 구현을 하였고, 답을 얻을 수 있었다.(2차시도-성공) 

# 느낀점

전혀 감이 안잡히지만 혼자 풀어볼려고 노력하였는데 결국 풀 수 있어서 기분이 좋았다. 인터넷에 검색해보니 트리의DP라는 분야가 이미 있어서 그에 대한 자료를 찾아보면서 거의 모든 문제에서 dfs를 사용한다는 점을 알 수 있어서 좀 더 쉽게 착안할 수 있었다. 처음에 생각한 고정관념에서 DP로 넘어가는 과정이 너무 힘들었는데 앞으로는 좀 더 유동적인 생각을 가져야겠고, 그럴려면은 문제를 더 많이 풀어봐야겠다고 생각했다.


