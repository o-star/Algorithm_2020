# 1938. 통나무 옮기기

# 풀이법

    1) 최단경로 구하기 BFS로 접근하였다.
    2) 통나무의 세점을 다 check하기 힘드니깐, 통나무의 중심점을 이용하여 visit check를 하였다. dir따라 왼쪽점 오른쪽점 위점 아래점 이렇게 따로 설정하였다.
    3) visit배열은 dir에 따라 다르게 설정하였다. [x][y][dir]
    4) 이동가능여부 확인 및 회전가능 여부에 따라 que에 추가하였다. -> 회전가능 여부 확인은 문제에 나와있는대로 6점을 다 체크하였다.
    5) 통나무의 start middlePoint 가 목표점인 end middlePoint와 같게 되면 통나무가 도착한거니깐 그대로 결과 return
    

# 느낀점

프로그래머스의 블록 옮기기 문제를 풀어보지 않았다면 나는 통나무를 3개를 visit로 체크하였을 것이다. 오랜만에 좋은문제를 풀었다.


