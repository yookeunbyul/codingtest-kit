## [JavaScript] 재귀 함수를 이용한 순열, 조합

n개에서 r개씩 뽑아내는 경우의 수를 구하는 문제가 많다.

재귀 함수를 이용한 알고리즘을 외울 필요가 있다.

### 조합

서로 다른 n개의 물건에서 순서를 생각하지 않고 r개를 택할 때,

즉 [1, 2, 3] = [3, 2, 1] 이렇게 순서가 바뀌어도 같은 구성이기 때문에 하나의 조합으로 취급한다는 이야기다.

```
//arr에서 n개를 뽑는다.

function combinations(arr,n){
    //1개만 뽑는거면 그대로 1개 뽑아서 return
    if(n === 1) return arr.map(v => [v]);

    let result = [];

    arr.forEach((fixed, idx, arr) => {
        let rest = arr.slice(idx+1);
        //선택된 요소를 제외한 나머지를 구한다.

        let combis = combinations(rest, n-1);
        //나머지 요소를 가지고 조합을 구한다.

        let combine = combis.map(v => [fixed, ...v]);
        구한 조합과 선택된 요소를 합쳐준다.

        result.push(...combine);
        //결과를 result에 넣어준다.
    })

    return result;
}
```
