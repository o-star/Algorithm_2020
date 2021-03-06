# 부분배열 고르기 해결

## 문제 접근 방법

1. N이 100,000이므로 최소한 NlogN의 알고리즘을 떠올려야 했다.

2. 분할정복을 이용하여 최대 depth를 logN으로 하고, merge하는 과정은 n 타임으로 총 logN 타임이 걸렸다.

3. 종만북에서 본 울타리 잘라내기 문제의 해결책과 거의 같았다는 것을 알고 문제를 풀었다.

4. 여기서 중요한 점은 divide하는 과정에서 중간에 걸치는 최댓값을 구해야하는데, 이 때 중간에서 시작하여, 왼쪽 및 오른쪽으로 확장하며 값을 찾는다..

5. 이 때 한칸씩 확장하는데, 왼쪽으로 확장할지 오른쪽으로 확장할지는 왼쪽값과 오른쪽 값 중 더 큰 쪽으로 한 칸 확장하면 된다.

6. 증명은 귀류법을 활용한다. 식이 Sum of A[l] ~ A[r] * min of A[l] * A[r] 이므로 더 큰 수를 고르게 되면 Sum of A[l] ~ A[r] 과 함께 min of A[l] * A[r]이 작은 수를 골랐을 때보다 클 수 밖에 없으므로 항상 큰 수가 있는 방향으로 확장하는 것이 최선의 선택임이 증명된다.

## 느낀 점

1. 처음에 어떻게 구해야 하는지 많이 고민했는데 힌트를 얻어 분할정복으로 풀 수 있다는 것을 알게 되었다.

2. 분할정복을 많이 풀어본 적이 없어 좋은 경험이 되었으며, 특히 옛날에 봤던 울타리 잘라내기 문제와 유사하다는 점을 보고 신기하였고 울타리 잘라내기를 분할 정복이 아닌 스와핑(O(n))과 세그먼트 트리(logN)로 풀어낼 수 있다는 점을 알게 되었다.

3. long을 연습할 수 있는 좋은 문제였다.

## 한태식이 돌아왔구나...!!
