## 모음 사전

사전에 알파벳 모음 'A', 'E', 'I', 'O', 'U'만을 사용하여 만들 수 있는, 길이 5 이하의 모든 단어가 수록되어 있습니다. 사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA"이며, 마지막 단어는 "UUUUU"입니다.

단어 하나 word가 매개변수로 주어질 때, 이 단어가 사전에서 몇 번째 단어인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- word의 길이는 1 이상 5 이하입니다.

- word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.

## 입출력 예

| word    | result |
| ------- | ------ |
| "AAAAE" | 6      |
| "AAAE"  | 10     |
| "I"     | 1563   |
| "EIO"   | 1189   |

## 입출력 예 설명

- 입출력 예 #1

  사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA", "AAA", "AAAA", "AAAAA", "AAAAE", ... 와 같습니다. "AAAAE"는 사전에서 6번째 단어입니다.

- 입출력 예 #2

  "AAAE"는 "A", "AA", "AAA", "AAAA", "AAAAA", "AAAAE", "AAAAI", "AAAAO", "AAAAU"의 다음인 10번째 단어입니다.

- 입출력 예 #3

  "I"는 1563번째 단어입니다.

- 입출력 예 #4

  "EIO"는 1189번째 단어입니다.

## 풀이

```
function solution(word) {
    //dfs해서

    //a,e,i,o,u 순으로 dfs 탐색하는데
    //계속 재귀함수가 되니깐
    //""
    //"A"
    //"AA"
    //"AAA" ... 이렇게 계속 붙으면서 사전을 만들어간다

    //길이가 5 이하이기 때문에 5를 넘어가면 return 해줘야한다

    let alphabet = ["A","E","I","O","U"];
    let dic = [];

    //중복을 따지는게 아니니깐 방문여부는 필요 X

    function dfs(cur, len){
        //종료 조건
        if(len > 5) return;

        //사전을 만들어간다 => index가 곧 몇번째 단어
        dic.push(cur);

        for(let i=0; i<alphabet.length; i++){
            dfs(cur + alphabet[i], len + 1);
        }
    }

    dfs("", 0);

    return dic.indexOf(word);
}
```
