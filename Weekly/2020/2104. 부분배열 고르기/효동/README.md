# 2104. 부분배열 고르기

# 풀이방법

> 분할정복

1. 분할정복을 이용하여 3가지 범위(start에서mid, mid+1에서start, mid 겹치는 부분)를 나누어 각 범위에서의 Max값을 구한 뒤 비교하여 최종 답을 도출한다.
2. mid 겹치는 부분에서 구할 때는 양옆으로 범위를 넓혀가야 하는데 left와 right 중 무엇을 먼저 선택해서 나아갈지가 중요하다.
![image](https://user-images.githubusercontent.com/54053016/90668188-a3d17980-e28a-11ea-9829-9229705c3cdb.png)
3. mid에서 나아갈 때 max값을 비교하는 것이 초점이므로 멈추지 말고 계속 나아가야하는 것 역시 중요하다!



# 중요했던 점 및 어려웠던 점

1. 수의 범위가 너무 커서 계속 불가능하다는 생각때문에 시간을 많이 잡아 먹었다.
2. mid를 기준으로 양쪽으로 나눈다는 것 외에 중간구간도 따로 따져야 하는 것을 인지했으나 한번도 구현해본 적이 없어서 낯설었다.
3. 중간에서 양쪽으로 범위를 넓혀갈 때 결국은 start점과 end점까지 끝까지 가야하는 것이 아주아주 중요했다.
4. left와 right 중 무엇을 먼저 선택하느냐가 아주 중요했다.


# 느낀점

개인적으로 다른 유형보다 어렵고 다른 사람의 풀이를 조금 참고해도 이해가 잘 가지 않는 문제였다. 여러모로 많은 것을 배운 문제였다. 나름 계산한다고 메모리값을 계산했으나 도저히 수의 범위가 너무 커서 진도가 잘 안나갔는데 참.. 제대로 판단하는 방법을 익힐 필요가 있다.

