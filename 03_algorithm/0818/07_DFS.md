## DFS(깊이우선탐색)

* 비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요함
* 두 가지 방법
  * 깊이 우선 탐색(DFS)
  * 너비 우선 탐색(BFS)
* 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 **더 이상 갈 곳이 없게 되면** 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법
* 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 스택 사용

> DFS가 스택을 사용해서 할 수 있는거지 무조건 스택 사용할 필요없음

+ DFS 알고리즘

  1. 시작 정점 v를 결정하여 방문한다

  2. 정점 v에 인접한 정점 중에서

     > (1) - > (2) :1에서 2는 인접 but 2에서 1은 인접 x
     >
     > (1) - (2) : 1과 2는 인접 
     >
     > 방향의 유무

     1) 방문하지 않는 정점 w가 있으면 정점 v를 스택에 `push`하고 정점 w를 방문한다 그리고 w를 v로 하여 다시 2)를 반복한다
     2) 방문하지 않은 정점이 없으면 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 다시 2)를 반복한다

  3. 스택이 공백이 될 때까지 2)를 반복한다

* DFS 예

  * 초기상태: 배열 `visited`를 `False`로 초기화하고 공백스택을 생성
  * 인접 정점 w를 찾아서 방문하고 이전에 있던 위치는 스택에 `push`한다.
  * 배열에는 방문했던 곳을 `True`로 바꾸고 v는 이동한 곳을 옮긴다
  * 계속 진행을 하다가 더 이상 인접한 곳도 방문할 곳도 없다면 이동한 순서대로 다시 뒤로 돌아오고 스택에서 차례로 `pop`한다
  * `pop`을 하면서 방문하지 않았고 인접한 곳이 등장한다면 위의 방식대로 반복하고
  * 계속 `pop`을 하며 방문하지 않을 곳을 찾는다
  * 마지막으로 스택이 비었다면 종료

```
노드의 개수 : V
엣지의 개수 : E
V, E = map(int, input().split())
ad = [[0] * (V+1) for _ in range(V+1)]
for _ in range(E):
	n1, n2 = map(int, input().split())
	ad[n1][n2] = 1
	ad[n2][n1] = 1
```

```
def dfs(s, V):
	visited = [0] * (V+1)
	stack = []
	visited[s] = 1
	i = s # 현재 방문한 정점 i
	print(node[i])
	while i != 0: #True:
		for w in range(1, V+1):
			if adj[i][w] == 1 and visited[w]==0:
				stack.append(i) # 방문 경로 저장
				i = w # 새 방문지 
				visited[w] = 1
				print(node[i])
				break
		else:
			if stack:
				i = stack.pop()
			else:
				i = 0
				# break
				
node = ['', 'A','B','C','D','E','F','G']
		
```
