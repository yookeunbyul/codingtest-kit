## 프로세스

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| --------- | ---------------- | ------------------ | --------- |
| 0         | []               | []                 | [7,4,5,6] |
| 1~2       | []               | [7]                | [4,5,6]   |
| 3         | [7]              | [4]                | [5,6]     |
| 4         | [7]              | [4,5]              | [6]       |
| 5         | [7,4]            | [5]                | [6]       |
| 6~7       | [7,4,5]          | [6]                | []        |
| 8         | [7,4,5,6]        | []                 | []        |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

## 제한사항

- bridge_length는 1 이상 10,000 이하입니다.

- weight는 1 이상 10,000 이하입니다.

- truck_weights의 길이는 1 이상 10,000 이하입니다.

- 모든 트럭의 무게는 1 이상 weight 이하입니다.

## 입출력 예

| bridge_length | weight | truck_weights                   | return |
| ------------- | ------ | ------------------------------- | ------ |
| 2             | 10     | [7,4,5,6]                       | 8      |
| 100           | 100    | [10]                            | 101    |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] | 110    |

## 풀이

```
class Node{
    constructor(value){
        this.value = value;
        this.next = null;
    }
}

class Queue{
    constructor(){
        this.head = null;
        this.tail = null;
        this.size = 0;
        this.sum = 0;
    }

    enqueue(newValue){
        const newNode = new Node(newValue);
        if(this.head === null){
            this.head = this.tail = newNode;
        }else{
            this.tail.next = newNode;
            this.tail = newNode;
        }
        this.size += 1;
        this.sum += this.tail.value;
    }

    dequeue(){
        const value = this.head.value;
        this.head = this.head.next;
        this.size -= 1;
        this.sum -= value;

        return value;
    }

    peek(){
        return this.head.value;
    }
}

function solution(bridge_length, weight, truck_weights) {
    //건너려면 최소 몇 초가 걸리는지 알아내야 합니다.
    //다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며,
    //다리는 weight 이하까지의 무게를 견딜 수 있습니다.

    const queue = new Queue();

    let count = 0;

    //[0, 0, 0] 상태이면 길이가 3인 다리에 아직 트럭이 하나도 올라오지 않은 상태이며
    //[0, 0, 7] 상태이면 무게 7인 트럭이 이제 막 진입한 상태이며, 1초가 지날 때 마다
    //[0, 0, 7] -> [0, 7, 0] -> [7, 0, 0] => 3초 소요

    //아 그래서 길이에 따라 한칸씩 1초씩 올라가는구나

    for(let i=0; i<bridge_length; i++){
        queue.enqueue(0);
    }

    for(let truck of truck_weights){
        //한번밀고
        queue.dequeue();

        //또올라와도될까?확인
        //현재 다리위무게와 올라갈 트럭이 weight을 넘으면안되니깐
        //위에있는트럭을 보내서 맞춰줘야됨
        while(queue.sum + truck > weight){
            queue.dequeue();
            queue.enqueue(0);
            count++;
        }

        //트럭올라와
        queue.enqueue(truck);
        count++;
    }

    //트럭 for문 다 돌아도 다리를 비워줘야하니깐
    while(queue.sum > 0){
        queue.dequeue();
        queue.enqueue(0);
        count++;
    }

    return count;
}
```
