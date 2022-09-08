# 서론

오늘은 동선 최적화 알고리즘에 사용되는 요소들은 무엇이 있는지 파악해보겠습니다.

# 본론

대표적인 동선 최적화 알고리즘은 다익스트라, A\*, Floydwarshall, 벨만-포드 알고리즘 등이 있습니다.

동선 최적화 알고리즘은 최단 경로 문제라고도 부릅니다. 주로 그래프를 이용해 표현됩니다.

각 지점은 그래프에서 `노드`로 표현되고, 지점 간 연결된 도로는 그래프에서 `간선`으로 표현됩니다.

또한 실제 코딩 테스트에서는 최단 경로를 모두 출력하는 문제보다는 단순히 최단 거리를 출력하도록 요구하는 문제가 많이 출제됩니다.

### 다익스트라 최단 경로 알고리즘

그래프에서 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘이다.

이는 기본적으로 그리디 알고리즘으로 분류됩니다.

- 기본 원리

1. 출발 노드를 설정한다.
2. 최단 거리 테이블을 초기화한다.
3. 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택한다.
4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.
5. 위 과정에서 3과 4를 반복한다.

알고리즘 동작 과정에서 최단 거리 테이블은 각 노드에 대한 현재까지의 최단 거리 정보를 가지고 있습니다. 처리 과정에서 더 짧은 경로를 찾으면 `이제부터는 이 경로가 제일 짧은 경로야`라고 갱신합니다.

### 시간복잡도

구현하기 쉽지만 느리게 동작하는 경우와 구현하기에는 까다롭지만 빠르게 동작하는 경우가 있습니다. 첫번째의 경우, 그리디 알고리즘을 활용하는 경우이며 시간복잡도는 O(V^2)입니다.
이 때, 단계를 거치며 한 번 처리된 노드의 최단 거리는 고정되어 더 이상 바뀌지 않습니다.
한 단계당 하나의 노드에 대한 최단 거리를 확실하게 찾는 형태입니다. 
두번째의 경우에는 O(V)입니다.

### 실제 구현 

```python
import sys
input = sys.stdin.readline
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정하였음

# 노드의 개수, 간선의 개수를 입력받기 ex) "3 5 6 7" => [3, 5, 6, 7]
n, m = map(int, input().split())
# 시작 노드 번호를 입력받기
start = int(input())
# 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트를 만든다
graph = [[] for i in range(n + 1)]
# 방문한 적이 있는지 체크하는 목적의 리스트를 만든다
visited = [False] * (n + 1)
# 최단 거리 테이블을 모두 무한으로 초기화
distance = [INF] * (n + 1)

# 모든 간선 정보를 입력받기
for _ in range(m):
  a, b, c = map(int, input().split())
  # a번 노드에서 b번 노드로 가는 비용이 c라는 의미
  graph[a].append((b, c))

# 방문하지 않은 노드 중에서, 가장 최단 거리가 짧은 노드의 번호를 반환
def get_smallest_node():
  min_value = INF
  index = 0 # 가장 최단 거리가 짧은 노드(인덱스)
  for i in range(1, n + 1):
    # 최단 거리보다 작고 아직 방문하지 않았다면, 최단거리를 갱신한다
    if distance[i] < min_value and not visited[i]:
      min_value = distance[i]
      index = i
  return index

def dijkstra(start):
  # 시작 노드에 대해서 초기화 
  distance[start] = 0
  visited[start] = True
  for j in graph[start]:
    distance[j[0]] = j[1]
  # 시작 노드를 제외한 전체 n - 1개의 노드에 대해 반복
  for i in range(n - 1):
    # 현재 최단 거리가 가장 짧은 노드를 꺼내서, 방문 처리
    now = get_smallest_node()
    visited[now] = True
    # 현재 노드와 연결된 다른 노드를 확인
    for j in graph[now]:
      cost = distance[now] + j[1]
      # 현재 노드를 거쳐서 다른 노드로 이동하는 거리가 더 짧은 경우
      if cost < distance[j[0]]:
        distance[j[0]] = cost

# 다익스트라 알고리즘을 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리를 출력
for i in range(1, n + 1):
  # 도달할 수 없는 경우, 무한(INFINITY)라고 출력
  if distance[i] == INF:
    print("INFINITY")
  else:
    print(distance[i])

```

### 플로이드 워셜 알고리즘
