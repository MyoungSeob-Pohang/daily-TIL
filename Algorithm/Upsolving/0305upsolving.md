# 3월 5일 업솔빙

DFS/BFS 를 이해하기 위한 한 주였다. 아직 더 많은 문제를 풀어봐야 할 것같다. 아직 100% 습득은 되지 안았다, 내가 이전에 작성한 코드를 조금 봐야 풀 수 있다. 조금만 더 노력하고 이해하려고 하면 충분히 할 수 있을 것 같다.

1. 단지번호 붙이기

```
n = int(input())
m = n
graph = []
sum = 0
count = 0
result = []

def bfs(x, y):
  global count

  if x <= -1 or x >= n or y <= -1 or y >= m:
    return False

  if graph[x][y] == 1:
    graph[x][y] = 0
    count += 1

    bfs(x - 1, y)
    bfs(x + 1, y)
    bfs(x, y - 1)
    bfs(x, y + 1)

    return True
  return False


for i in range(n):
  graph.append(list(map(int, input())))

for x in range(n):
  for y in range(m):
    if bfs(x, y) == True:
      sum += 1
      result.append(count)
      count = 0

print(sum)

result.sort()
for i in result:
  print(i)
```

2. 바이러스

```
def dfs(graph, start, visited):
  global count

  visited[start] = True

  for i in graph[start]:
    if not visited[i]:
      dfs(graph, i, visited)
      count += 1


n = int(input())
m = int(input())
graph = [[] * n for _ in range(n + 1)]
count = 0

for i in range(m):
  x, y = map(int, input().split())
  graph[x].append(y)
  graph[y].append(x)

visited = [False] * (n + 1)

dfs(graph, 1, visited)

print(count)
```

3. 연결 요소의 개수

```
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline

def dfs(s):
  visited[s] = True

  for i in graph[s]:
    if visited[i] == False:
      dfs(i)

n, m = map(int, input().split())

graph = [[] for _ in range(n + 1)]
count = 0

for i in range(m):
  x, y = map(int, input().split())
  graph[x].append(y)
  graph[y].append(x)

visited = [False for _ in range(n + 1)]

for i in range(1, n + 1):
  if visited[i] == False:
    dfs(i)
    count += 1

print(count)
```

4. 유기농 배추

```
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline

def dfs(x, y):
  if x <= -1 or x >= n or y <= -1 or y >= m:
    return False

  if graph[x][y] == 1:
    graph[x][y] = 0

    dfs(x - 1, y)
    dfs(x + 1, y)
    dfs(x, y - 1)
    dfs(x, y + 1)
    return True
  return False


case = int(input())
result = []
for z in range(case):
  m, n, k = map(int, input().split())

  graph = [ [0] * m for _ in range(n)]
  count = 0

  for i in range(k):
    x, y = map(int, input().split())
    graph[y][x] = 1

  for i in range(n):
    for j in range(m):
      if dfs(i, j) == True:
        count += 1
  result.append(count)

for i in result:
  print(i)
```
