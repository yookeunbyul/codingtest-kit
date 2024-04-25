## 게임 맵 최단거리

ROR 게임은 두 팀으로 나누어서 진행하며, 상대 팀 진영을 먼저 파괴하면 이기는 게임입니다. 따라서, 각 팀은 상대 팀 진영에 최대한 빨리 도착하는 것이 유리합니다.

지금부터 당신은 한 팀의 팀원이 되어 게임을 진행하려고 합니다. 다음은 5 x 5 크기의 맵에, 당신의 캐릭터가 (행: 1, 열: 1) 위치에 있고, 상대 팀 진영은 (행: 5, 열: 5) 위치에 있는 경우의 예시입니다.

<img width="87" alt="최단거리1_sxuruo" src="https://github.com/yookeunbyul/codingtest-kit/assets/91243651/8f8733c8-19ac-4672-9dd1-4ff438121997">

위 그림에서 검은색 부분은 벽으로 막혀있어 갈 수 없는 길이며, 흰색 부분은 갈 수 있는 길입니다. 캐릭터가 움직일 때는 동, 서, 남, 북 방향으로 한 칸씩 이동하며, 게임 맵을 벗어난 길은 갈 수 없습니다.
아래 예시는 캐릭터가 상대 팀 진영으로 가는 두 가지 방법을 나타내고 있습니다.

첫 번째 방법은 11개의 칸을 지나서 상대 팀 진영에 도착했습니다.

<img width="87" alt="최단거리2_hnjd3b" src="https://github.com/yookeunbyul/codingtest-kit/assets/91243651/fa64a2f8-fe54-4c8e-96e2-ff4998f93c8a">

두 번째 방법은 15개의 칸을 지나서 상대팀 진영에 도착했습니다.

<img width="87" alt="최단거리3_ntxygd" src="https://github.com/yookeunbyul/codingtest-kit/assets/91243651/28f98f1f-fe59-44cd-84e9-995ac90ad543">

위 예시에서는 첫 번째 방법보다 더 빠르게 상대팀 진영에 도착하는 방법은 없으므로, 이 방법이 상대 팀 진영으로 가는 가장 빠른 방법입니다.

만약, 상대 팀이 자신의 팀 진영 주위에 벽을 세워두었다면 상대 팀 진영에 도착하지 못할 수도 있습니다. 예를 들어, 다음과 같은 경우에 당신의 캐릭터는 상대 팀 진영에 도착할 수 없습니다.

<img width="86" alt="최단거리4_of9xfg" src="https://github.com/yookeunbyul/codingtest-kit/assets/91243651/36c4fe6c-b34f-4cac-b9b8-0339e7ad2198">

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 최솟값을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 해주세요.

## 제한사항

- maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수입니다.

  - n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않습니다.

- maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타냅니다.

- 처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있습니다.

## 입출력 예

| maps                                                          | answer |
| ------------------------------------------------------------- | ------ |
| [[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,1],[0,0,0,0,1]] | 2      |
| [[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,0],[0,0,0,0,1]] | -1     |

## 입출력 예 설명

- 입출력 예 #1

  주어진 데이터는 다음과 같습니다.

    <img width="87" alt="최단거리6_lgjvrb" src="https://github.com/yookeunbyul/codingtest-kit/assets/91243651/62c78d0b-12a9-43bd-b283-c731ca1379bc">

  캐릭터가 적 팀의 진영까지 이동하는 가장 빠른 길은 다음 그림과 같습니다.

    <img width="87" alt="최단거리2_hnjd3b (1)" src="https://github.com/yookeunbyul/codingtest-kit/assets/91243651/2b671726-29e4-475b-bdb6-0edef23d4581">

  따라서 총 11칸을 캐릭터가 지나갔으므로 11을 return 하면 됩니다.

- 입출력 예 #2

  문제의 예시와 같으며, 상대 팀 진영에 도달할 방법이 없습니다. 따라서 -1을 return 합니다.

## 풀이

```
function solution(maps) {
    //최단거리, 최소 => bfs
    //0은 벽이 있는 자리, 1은 벽이 없는 자리
    //처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치
    //상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치 => 5 * 5면 어쨋든 4,4의 자리

    const n = maps.length; //행
    const m = maps[0].length; //열
    const dir = [[-1, 0], [1, 0], [0, -1], [0, 1]]; //상하좌우

    function bfs(){
        let queue = [[0,0,1]]; //행,열,이동거리
        maps[0][0] = 0; //방문한 곳은 0으로 처리해서 중복관리

        //큐가 빌때까지 반복
        while(queue.length !== 0){
            //인접을 확인하기 위해 앞에서 빼준다.
            let [row, col, distance] = queue.shift();

            //종료조건
            if(row === n - 1 && col === m - 1){
                return distance;
            }

            for(let [r, c] of dir){
                let newRow = row + r;
                let newCol = col + c;

                //여기서 maps안에 있어야하고 벽이없는 자리여야한다.
                if(newRow < n && newRow >= 0 && newCol < m && newCol >= 0 && maps[newRow][newCol] === 1){
                    //이동거리를 +1해주면서 카운트한다.
                    queue.push([newRow, newCol, distance + 1]);
                    maps[newRow][newCol] = 0; //방문처리
                }
            }
        }
        //다 못돌고 튀어져나오면 못가는거니깐 -1
        return -1;
    };
    return bfs();
}
```
