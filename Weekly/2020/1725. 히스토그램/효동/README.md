# 1725. 히스토그램

## 풀이법

1. 재귀, 분할정복을 이용한다.
2. '부분배열 고르기'와 동일한 방식으로 middle점을 기준으로 왼쪽 오른쪽을 재귀로 수행하여 max넓이를 구하고 중간에서 양옆으로 나아가면서 넓이를 구한다.
3. 이후 세 영역에서 구한 넓이 중 max값을 출력한다.


## 실수 및 어려웠던 점

1. mid에서 양쪽으로 나아갈 때 당장 바로 옆 두개만 봐도 되는지 처음에 좀 헷갈렸다. 전체를 볼려고 생각하니 꼬여서 조금 오래 걸렸다.


## 느낀점

확실히 한번 풀어본 유형은 그 다음에 생각을 하는데 도움이 많이 되는구나!

