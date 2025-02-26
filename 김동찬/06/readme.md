# 알고리즘

Recursive, Data Structure, DP

# 난이도

배점: 300

AtCoder Problems: 528

# 링크

https://atcoder.jp/contests/abc340/tasks/abc340_c

# 접근 과정

보고 든 생각은 탑 다운 DP였다. 재귀함수를 갈라지는 값이 나오면 상태공간의 트리의 끝까지 갈 필요 없이 계산된 값을 구하면 되기 때문이다. 에디토리얼 역시 DP였지만, 실제로 풀 때는 DP를 활용하지 않고 딕셔너리를 활용했다.

문제를 관찰하다보니 루트에서 서브트리로 갈라지더라도 총 비용은 감소하지 않았다. 예를 들어 100을 50, 50으로 나누더라도 여전이 50 + 50 = 100이다. 마찬가지로 25 + 25 + 25 + 25 = 100이다. 그러므로 반으로 나뉠 때마다 딕셔너리에 기록을 해주면서 해당 값이 얼마나 있는지를 확인하고 셈해주며 초기화한다.

```python
from copy import deepcopy

N = int(input())
start = {}
nxt = {}

start[N] = 1
ans = 0
while start:  # 딕셔너리에 내용물이 있는 동안 반복
    for k, v in start.items():
        ans += k * v  # 해당 값이 몇 개나 있는지?
        if k % 2 == 0:
            mid = k // 2
            if mid != 1:  # 1은 더이상 연산에 필요하지 않음
                if not mid in nxt:
                    nxt[mid] = v * 2
                else:
                    nxt[mid] += v * 2
        else:  # 홀수면, 예를 들어 25는 13과 12로 나뉨.
            left = k // 2 + 1
            right = k // 2

            if left != 1:
                if not left in nxt:
                    nxt[left] = v
                else:
                    nxt[left] += v

            if right != 1:
                if not right in nxt:
                    nxt[right] = v
                else:
                    nxt[right] += v

    start = deepcopy(nxt)
    nxt.clear()

print(ans)
```
