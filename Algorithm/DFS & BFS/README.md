# DFS & BFS

## 그래프 탐색

- 목적 : 임의의 정점에서 시작해서, 연결되어 있는 모든 정점을 1번씩 방문하는 것

- 예제 - 백준 1260

[1260번: DFS와 BFS](https://www.acmicpc.net/problem/1260)

## DFS

```
깊이 우선 탐색, Depth First Search
```

![image](https://user-images.githubusercontent.com/85485290/204121369-7b7660fc-5521-473e-a953-443e61f43bfb.png)


- `스택`을 이용해서 최대한 깊이 방문하고, 갈 수 없으면 이전 정점으로 돌아간다.
- **앞으로 찾아서 가야할 노드** & **이미 방문한 노드**를 기준으로 탐색✨

### DFS 구현

1. **재귀 함수로 구현**
- `dfs(x)` : x를 방문한다.
- 시간복잡도 : `O(N)`

```python
"""
N : 정점
M : 간선
V : 첫번째 노드

그래프
4 5 1
1 2
1 3
1 4
2 4
3 4

인접 행렬
  1 2 3 4 
1 0 1 1 1
2 1 0 0 0
3 1 0 0 1
4 1 1 1 0
"""

graph
visited = [False] * (N+1)

def dfs(x, n):
	visited[x] = True
	# print

	for (i in range(1, n): 
		if graph[x][i] == 1 && !visited[i]:
			dfs(i, n)

```

---

## BFS

```
넓이 우선 탐색, Breadth First Search
```

![image](https://user-images.githubusercontent.com/85485290/204121376-889f0d86-9da9-4db0-8d27-1f31995fecf1.png)

- `큐`를 이용해서 구현 가능
    - 노드를 방문하면서 인접한 노드 중 방문하지 않았던 노드의 정보만 큐에 넣고, 먼저 큐에 들어있던 노드부터 방문하면 된다.
    

### BFS 구현

- 파이썬에서 list를 이용해 list.append(), list.pop() 을 사용하면 시간복잡도가 O(N)이 돼서 비효율적.
- 파이썬의 `deque`를 사용!
- 시간복잡도 :  `O(N)`

```python
from collections import deque

"""
N : 정점
M : 간선
V : 첫번째 노드

그래프
4 5 1
1 2
1 3
1 4
2 4
3 4

인접 행렬
  1 2 3 4 
1 0 1 1 1
2 1 0 0 0
3 1 0 0 1
4 1 1 1 0
"""

graph
visited = [False] * (N+1)

# root node, node 개수
def bfs(root, n):
    queue = deque([root])
		visited[root] = True
		# print

    while queue:
        v = queue.popleft()

				for i in range (1, N):
					if graph[v][i] == 1 && !visited[i]:
						visited[i] = True
						queue.add(i)
						# print
```

---

## BFS에 대해서

- BFS의 목적

```python
임의의 정점에서 시작해서, 모든 정점을 한 번씩 방문하는 것
```

- DFS도 목적은 같지만, 차이점은

```python
BFS는 최단 거리를 구하는 알고리즘! 이란 것

DFS는 해를 구하면 탐색이 종료되므로, 구한 해가 최단 경로라는 보장이 없음,,
```

- BFS를 이용해 해결할 수 있는 문제는 다음과 같은 조건을 만족해야 함
1. **최소 비용** 문제여야 함 
2. **간선의 가중치**가 1이어야 함
3. 정점과 간선의 개수가 적어야 함 (적다는 것 = 문제의 조건에 맞춰서 해결할 수 있다는 것)
- 간선의 가중치가 문제에서 구하라고 하는 최소 비용과 의미가 일치해야 함
    - 거리의 최소값을 구하는 문제 -> **가중치 = 거리**
    - 시간의 최소값을 구하는 문제 -> **가중치 = 시간**
