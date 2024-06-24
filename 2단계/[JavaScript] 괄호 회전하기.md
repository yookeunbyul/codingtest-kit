## 괄호 회전하기

다음 규칙을 지키는 문자열을 올바른 괄호 문자열이라고 정의합니다.

- (), [], {} 는 모두 올바른 괄호 문자열입니다.

- 만약 A가 올바른 괄호 문자열이라면, (A), [A], {A} 도 올바른 괄호 문자열입니다. 예를 들어, [] 가 올바른 괄호 문자열이므로, ([]) 도 올바른 괄호 문자열입니다.

- 만약 A, B가 올바른 괄호 문자열이라면, AB 도 올바른 괄호 문자열입니다. 예를 들어, {} 와 ([]) 가 올바른 괄호 문자열이므로, {}([]) 도 올바른 괄호 문자열입니다.

대괄호, 중괄호, 그리고 소괄호로 이루어진 문자열 s가 매개변수로 주어집니다. 이 s를 왼쪽으로 x (0 ≤ x < (s의 길이)) 칸만큼 회전시켰을 때 s가 올바른 괄호 문자열이 되게 하는 x의 개수를 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- s의 길이는 1 이상 1,000 이하입니다.

## 입출력 예

| s        | result |
| -------- | ------ |
| "[](){}" | 3      |
| "}]()[{" | 2      |
| "[)(]"   | 0      |
| "}}}"    | 0      |

## 입출력 예 설명

- 입출력 예#1

  | x   | s를 왼쪽으로 x칸만큼 회전 | 올바른 괄호 문자열? |
  | --- | ------------------------- | ------------------- |
  | 0   | "[](){}"                  | O                   |
  | 1   | "](){}["                  | X                   |
  | 2   | "(){}[]"                  | O                   |
  | 3   | "(){}[]"                  | X                   |
  | 4   | "{}[]()"                  | O                   |
  | 5   | "}[](){"                  | X                   |

## 풀이

```
function isArray(s){
    const array = [];
    const spread = [...s];

    for(let value of spread){
        if(value === "("){
            array.push(value);
        }else if(value === ")" && array[array.length-1] === "("){
            array.pop();
        }else if(value === ")" && array.length === 0){
            return false;
        }else if(value === "["){
            array.push(value);
        }else if(value === "]" && array[array.length-1] === "["){
            array.pop();
        }else if(value === "]" && array.length === 0){
            return false;
        }else if(value === "{"){
            array.push(value);
        }else if(value === "}" && array[array.length-1] === "{"){
            array.pop();
        }else if(value === "}" && array.length === 0){
            return false;
        }
    }

    return array.length === 0 ? true : false;
}

function solution(s) {
    //s를 왼쪽으로 x (0 ≤ x < (s의 길이)) 칸만큼 회전
    //올바른 괄호 문자열이 되게 하는 x의 개수를 return

    const sArray = [...s];
    const array = [s];

    for(let i=1; i<s.length; i++){
        //왼쪽으로 회전하는게 앞에껄 뒤로 넣는거잖아..
        const front = sArray.shift();
        sArray.push(front);

        array.push(sArray.join(''));
    }

    let answer = 0;

    for(let value of array){
        if(isArray(value)) answer++;
    }

    return answer;
}
```

## 풀이 2

```
function isArray(s){
    const array = [];
    const spread = [...s];

    for(let value of spread){
        if(value === "(" || value === "[" || value === "{"){
            array.push(value);
        }else if(value === ")"){
            if(array.length === 0){
                return false;
            }else if(array[array.length-1] === "("){
                array.pop();
            }
        }else if(value === "]"){
            if(array.length === 0){
                return false;
            }else if(array[array.length-1] === "["){
                array.pop();
            }
        }else if(value === "}"){
            if(array.length === 0){
                return false;
            }else if(array[array.length-1] === "{"){
                array.pop();
            }
        }
    }

    return array.length === 0 ? true : false;
}

function solution(s) {
    //s를 왼쪽으로 x (0 ≤ x < (s의 길이)) 칸만큼 회전
    //올바른 괄호 문자열이 되게 하는 x의 개수를 return

    const sArray = [...s];
    const array = [s];

    for(let i=1; i<s.length; i++){
        //왼쪽으로 회전하는게 앞에껄 뒤로 넣는거잖아..
        const front = sArray.shift();
        sArray.push(front);

        array.push(sArray.join(''));
    }

    let answer = 0;

    for(let value of array){
        if(isArray(value)) answer++;
    }

    return answer;
}
```
