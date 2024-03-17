## 전력망을 둘로 나누기

n개의 송전탑이 전선을 통해 하나의 트리 형태로 연결되어 있습니다. 당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다. 이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.

송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다. 전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- n은 2 이상 100 이하인 자연수입니다.

- wires는 길이가 n-1인 정수형 2차원 배열입니다.

  - wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.

  - 1 ≤ v1 < v2 ≤ n 입니다.

  - 전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.

## 입출력 예

| n   | wires                                             | result |
| --- | ------------------------------------------------- | ------ |
| 9   | [[1,3],[2,3],[3,4],[4,5],[4,6],[4,7],[7,8],[7,9]] | 3      |
| 4   | [[1,2],[2,3],[3,4]]                               | 0      |
| 7   | [[1,2],[2,7],[3,7],[3,4],[4,5],[6,7]]             | 1      |

## 입출력 예 설명

- 입출력 예 #1

  - 다음 그림은 주어진 입력을 해결하는 방법 중 하나를 나타낸 것입니다.

  ![ex1](https://github.com/yookeunbyul/codingtest-kit/assets/91243651/51d19930-f6fe-41eb-8f24-331a03f602e9)

  - 4번과 7번을 연결하는 전선을 끊으면 두 전력망은 각 6개와 3개의 송전탑을 가지며, 이보다 더 비슷한 개수로 전력망을 나눌 수 없습니다.

  - 또 다른 방법으로는 3번과 4번을 연결하는 전선을 끊어도 최선의 정답을 도출할 수 있습니다.

- 입출력 예 #2

  다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.

  ![ex2](https://github.com/yookeunbyul/codingtest-kit/assets/91243651/04dedd27-d3c5-4246-a230-20b32e935944)

  - 2번과 3번을 연결하는 전선을 끊으면 두 전력망이 모두 2개의 송전탑을 가지게 되며, 이 방법이 최선입니다.

- 입출력 예 #3

  다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.

  ![ex3](https://github.com/yookeunbyul/codingtest-kit/assets/91243651/21df3d47-aea3-4bca-93d5-0067c8fbd9a8)

  - 3번과 7번을 연결하는 전선을 끊으면 두 전력망이 각각 4개와 3개의 송전탑을 가지게 되며, 이 방법이 최선입니다.

## 풀이

```
function solution(n, wires) {
    //송전탑의 개수가 100이하이기 때문에 101로 우선 설정
    let answer = 101;

    let tree = [];
    for(let i=1; i<=n; i++) tree[i] = [];

    for(let [from, to] of wires){
        tree[from].push(to);
        tree[to].push(from);
    }

    //bfs로 탐색하는데 => 노드의 개수를 세기위해
    //전선들 중 하나를 끊어서 ===> 노드를 제외시켜서
    //두개의 그룹을 만든 후 송전탑 개수의 차이를 구해준다.
    //가능한 비슷하도록 나누면 그 차이가 최소일테니 최소값을 구해주면된다.

    //1을 제외한 전력망과 3을 제외한 전력망의 송전탑 수를 빼주면
    //1과 3이 끊겼을 때 경우를 구할 수 있다.

    function bfs(root, exceptNode){
        //정점과 제외할 노드를 받는다.
        let visited = new Array(n+1).fill(false);
        let count = 0;

        let queue = [];
        queue.push(root);
        visited[root] = true;

        while(queue.length){
            let dequeue = queue.shift();
            count++;

            for(let node of tree[dequeue]){
                //제외할 노드가 아니고 방문도 안했을 경우만 탐색
                if(node !== exceptNode && !visited[node]){
                    queue.push(node);
                    visited[node] = true;
                }
            }
        }

        return count;
    }

    for(let [from,to] of wires){
        //최소값을 구하기때문에 answer을 우선 가장 큰 값으로
        answer = Math.min(answer, Math.abs(bfs(from,to) - bfs(to,from)));
    }

    return answer;
}
```
