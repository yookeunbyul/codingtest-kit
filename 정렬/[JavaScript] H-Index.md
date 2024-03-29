## H-Index

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

## 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.

- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

## 입출력 예

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |

## 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

## 풀이

https://www.ibric.org/bric/trend/bio-series.do?mode=series_view&newsArticleNo=8802417&articleNo=8882714&beforeMode=latest_list#!/list

h-index에 이해하려면 이 글을 먼저 읽어보는 것이 좋다.

```
function solution(citations) {
    //어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다.

    //전체 논문중 많이 인용된 순으로 정렬
    citations.sort((a,b) => b-a);

    //피인용수가 논문수와 같아지거나 피인용수가 논문수보다 작아지기 시작하는 숫자가 바로 나의 h가 됩니다.
    //그럼 피인용수가 논문수보다 크면 계속 논문수를 1 증가시켜준다.

    //논문수
    let thesis = 0;

    //[6.5.3.1.0]
    while(thesis < citations[thesis]){
        thesis++;
    }

    return thesis;
}
```
