## 투 포인터 알고리즘

투 포인터 알고리즘 (Two Pointers Algorithm)은 일차원 배열에서 **두 개의 포인터의 위치를 기록하면서 처리하는 알고리즘**을 의미한다.

```
function solution() {
    //원소 하나씩 돌면서 연속된 구간의 합이 M이 되면 answer++
    //M보다 작으면 계속해서 더해주고
    //크면 break

    const arr = [1,3,2,2,5,7,2,6];
    let answer = 0;
    const M = 9;

    for(let i=0; i<arr.length; i++){
        //첫 합은 첫번째 원소부터 시작..
        let sum = arr[i];
        for(let j=i+1; j<arr.length; j++){
            if(sum === M){
                answer++;
            }else if(sum > M){
                break;
            }else if(sum < M){
                sum += arr[j];
            }
        }
    }

    return answer;
}
```

위와 같이 일차원 배열에서 연속된 구간의 합이 M이 되도록 하는 경우의 수를 구하는 문제에 이중 for문을 사용하면 시간복잡도가 O(N2)이므로

배열의 크기가 커질 수록 엄청나게 느려지게 될 것이다.

이를 보완하기 위해 등장한 알고리즘이 투 포인터 알고리즘이다.

우선 start, end 두개의 포인터를 가진다.

```
만약 현재 구간의 합이 M(9)와 같다면 ans 값을 증가시켜주고 합이 M(9) 보다 작거나 같다면 end를 1 증가시켜주면서 ans 값을 증가시켜준다, 합이 M(9) 보다 크다면 start를 1 증가시킨다.
```

```
//포인터가 index의 값을 가진다.

function solution() {
    const arr = [1,3,2,2,5,7,2,6];
    let answer = 0, start = 0, end = 0;
    let sum = arr[0];
    const M = 9;

    while(arr[start] && arr[end]){
        if(sum === M){
            answer++;
            end++;
            sum += arr[end];
        }else if(sum > M){
            //앞에 빼주고 이동
            sum -= arr[start];
            start++;
        }else if(sum < M){
            end++;
            sum += arr[end];
        }
    }

    return answer;
}
```

## 슬라이딩 윈도우 알고리즘

슬라이딩 윈도우 알고리즘은 투포인터 알고리즘과 유사하지만 두 포인터가 각자 움직이는 것이 아닌 고정적인 길이를 가지고 함께 움직이는 알고리즘이다.
