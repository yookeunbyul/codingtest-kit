## 더 맵게

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.

- K는 0 이상 1,000,000,000 이하입니다.

- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.

- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

## 입출력 예

| scoville             | K   | return |
| -------------------- | --- | ------ |
| [1, 2, 3, 9, 10, 12] | 7   | 2      |

## 입출력 예 설명

스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
새로운 음식의 스코빌 지수 = 1 + (2 \* 2) = 5
가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
새로운 음식의 스코빌 지수 = 3 + (5 \* 2) = 13
가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

## 풀이

```
class MinHeap {
    //부모가 제일 작은 값
    constructor() {
        this.heap = [];
    }

    size() {
        return this.heap.length;
    }

    push(value) {
        //값을 넣고 정렬
        this.heap.push(value);
        let currentIndex = this.heap.length - 1;

        //부모가 제일 작아야하는데 부모의 값이 제일 크면 계속 정렬
        while(currentIndex > 0 && this.heap[currentIndex] < this.heap[Math.floor((currentIndex - 1) / 2)]){
            //부모와 자식이랑 교환
            const temp = this.heap[currentIndex];
            this.heap[currentIndex] = this.heap[Math.floor((currentIndex - 1)/ 2)];
            this.heap[Math.floor((currentIndex-1)/2)] = temp;

            //현재 인덱스를 부모 인덱스로 대입 => 올라왔으니깐
            currentIndex = Math.floor((currentIndex-1)/2);
        }
    }

    pop() {
        //값을 빼고 정렬(루트가 빠지겟네?)
        if(this.heap.length === 0) return null;
        if(this.heap.length === 1) return this.heap.pop();

        const minValue = this.heap[0];
        this.heap[0] = this.heap.pop();
        let currentIndex = 0;

        while(currentIndex * 2 + 1 < this.heap.length){
            let minChildIndex = currentIndex * 2 + 2 < this.heap.length && this.heap[currentIndex * 2 + 2] < this.heap[currentIndex * 2 + 1] ? currentIndex * 2 + 2 : currentIndex * 2 + 1;

            if(this.heap[currentIndex] < this.heap[minChildIndex]){
                break;
            }

            const temp = this.heap[currentIndex];
            this.heap[currentIndex] = this.heap[minChildIndex];
            this.heap[minChildIndex] = temp;
            currentIndex = minChildIndex;
        }

        return minValue;
    }

    peek() {
        return this.heap[0];
    }
}

function solution(scoville, K) {
    //섞은 음식의 스코빌 지수 =
    //가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

    //해서 스코빌 지수를 k이상으로 만든다.
    //섞어야하는 최소 횟수를 return

    //힙을 활용해서 만들어야한다.
    const minHeap = new MinHeap();

    //힙에다가 sco값을 넣어준다.
    for(const sco of scoville){
        minHeap.push(sco);
    }

    //섞는 최소횟수 return
    let mixedCount = 0;

    //섞어야하는 두개가 남아있고 원하는 스코빌보다 작으면 계속 돌려
    while(minHeap.size() >= 2 && minHeap.peek() < K){
        const first = minHeap.pop();
        const second = minHeap.pop(); //어차피 정렬알아서 해주니깐
        const mixed = first + (second * 2);
        mixedCount++;
        minHeap.push(mixed);
    }

    //peek()이 가장 작은 값이니깐
    return minHeap.peek() >= K ? mixedCount : -1;
}
```

이게 들어오는 값 길이가 너무 길어서 단순히 배열이나 리스트로 하기엔 무리.
heap으로 우선순위 큐 만들어서 풀이하는데 .... 이걸 어떻게 풀어요?
