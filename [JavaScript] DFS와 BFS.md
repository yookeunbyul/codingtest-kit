## DFS와 BFS

- 대표적인 그래프 탐색 알고리즘이 DFS , BFS

우선,

정점과 간선의 정보를 주면 그 데이터로 그래프를 만들어준다.

```
//정점의 개수, 간선의 개수, 정점의 번호
let [n,m,v] = input[0].split(' ').map(Number);

//간선이 연결하는 두 정점의 번호
let edges = input.slice(1).map(v => v.split(' ').map(Number));

//1부터 시작하니깐 0은 버리고 시작
let graph = [];
for(let i=1; i<=n; i++) graph[i] = [];

//쌍으로 연결된 정점을 인접리스트로 만들어준
for(let [from, to] of edges){
  graph[from].push(to);
  graph[to].push(from);
}
```

<br />

### DFS(깊이 우선 탐색)

- 스택 자료구조를 사용한다.

1. 시작 노드를 스택에 넣고 [방문 처리]한다.

2. 스택에 마지막으로 들어 온 노드에 방문하지 않은 인접 노드가 있는지 확인한다.

   1. 있다면 방문하지 않은 인접 노드를 스택에 삽입하고 방문처리한다.
   2. 없다면 현재 노드를 스택에서 추출한다.

3. 2번 과정을 더이상 반복할 수 없을 때까지 반복한다.

<br />

```
//dfs 탐색
//방문여부 체크
let dfs_visited = new Array(n+1).fill(false);
let dfs_graph = [];

function dfs(v){
  //정점의 번호부터 들어온다
  dfs_visited[v] = true; //방문했습니다.
  dfs_graph.push(v); //출력을 위해 리스트에 노드 삽입

  //연결되어있는 노드를 탐색해준다.
  for(let node of graph[v]){
    //방문하지않았다면 탐색
    if(!dfs_visited[node]) dfs(node);
  }
}

dfs(v);
console.log(dfs_graph.join(' '));
```

<br />

### BFS(너비 우선 탐색)

- 큐 자료구조를 사용한다.

- 최단 거리를 탐색하기 위한 목적으로 사용

1. 시작 노드를 큐에 넣고 방문 처리한다.

2. 큐에서 원소를 꺼내서 방문하지 않은 인접 노드가 있는지 확인한다.

   - 있다면 방문하지 않은 인접 노드를 큐에 삽입하고 방문 처리한다.

3. 2번 과정을 더이상 반복할 수 없을 때까지 반복한다.

<br />

```
//bfs 탐색
let bfs_visited = new Array(n+1).fill(false);
let bfs_graph = [];

function bfs(v){
  let queue = [];
  //큐에 넣고
  queue.push(v);
  //방문처리 해준다.
  bfs_visited[v] = true;

  //큐가 빌때까지 계속 반복
  while(queue.length !== 0){
    //원소를 꺼내서 방문하지 않은 인접노드가 있는지 확인한다.
    //큐이기 때문에 앞에서 빼줘야됨
    let dequeue = queue.shift();
    bfs_graph.push(dequeue); //출력을 위해 리스트에 노드 삽입

    for(let node of graph[dequeue]){
      //아직 방문하지 않은 인접노드를 큐에 삽입하고 방문처리 => 탐색
      if(!bfs_visited[node]){
        queue.push(node);
        bfs_visited[node] = true;
      }
    }
  }
}

bfs(v);
console.log(bfs_graph.join(' '));
```
