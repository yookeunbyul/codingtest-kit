## 소수 찾기

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.

- numbers는 0~9까지 숫자만으로 이루어져 있습니다.

- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

## 입출력 예

| numbers | return |
| ------- | ------ |
| "17"    | 3      |
| "011"   | 2      |

## 입출력 예 설명

- 입출력 예 #1

  [1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

- 입출력 예 #2

  [0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.

  - 11과 011은 같은 숫자로 취급합니다.

## 풀이

```
//순열 구하는 알고리즘
function permutations(arr, n){
    if(n === 1) return arr.map(v => [v]);

    let result = [];

    arr.forEach((fixed, index, arr) => {
        let rest = arr.filter((_,idx) => idx !== index);

        let perms = permutations(rest, n-1);

        let combine = perms.map(v => [fixed, ... v]);

        result.push(...combine);
    })

    return result;
}

//소수 구하는 알고리즘
function is_prime(num){
    if(num === 1) return false;
    else if(num === 0) return false;

    for(let i=2; i*i<=num; i++){
        if(num % i === 0) return false;
    }

    return true;
}


function solution(numbers) {
    //"17" => 이면 1과 7로 만들 수 있는 순열을 구해서
    //소수인 순열만 남긴후 몇개인지 return 하면되는거잖아~..

    let stack = numbers.split('');
    let perms = [];

    for(let i=1; i<=stack.length; i++){
        perms.push(permutations(stack, i));
    }

    perms = perms.flatMap(v => v).map(v => v.join('')).map(Number);

    //set으로 중복삭제
    let mySet = new Set(perms);

    return [...mySet].filter(v => is_prime(v)).length;
}
```
