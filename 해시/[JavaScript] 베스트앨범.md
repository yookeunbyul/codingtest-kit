## 베스트앨범

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.

2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.

3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

## 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.

- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.

- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.

- 장르 종류는 100개 미만입니다.

- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.

- 모든 장르는 재생된 횟수가 다릅니다.

## 입출력 예

| clothes                                         | plays                      | return    |
| ----------------------------------------------- | -------------------------- | --------- |
| ["classic", "pop", "classic", "classic", "pop"] | [500, 600, 150, 800, 2500] | [4,1,3,0] |

## 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생

- 고유 번호 0: 500회 재생

- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생

- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

- 장르 별로 가장 많이 재생된 노래를 최대 두 개까지 모아 베스트 앨범을 출시하므로 2번 노래는 수록되지 않습니다.

## 풀이

```
function solution(genres, plays) {
    //1.노래가 많이 재생된 장르 => plays의 총합을 구한다. => 합이 높은 순으로
    //2.장르 내에서 많이 재생된 노래 => plays가 높은 순으로
    //3.재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다. => plays가 같으면 i가 낮은 순으로

    //최대 두 개까지 모아 베스트 앨범을 출시하므로 2번 노래는 수록되지 않습니다. => 장르별 2개의 노래만

    let genPlays = genres.map((v,i) => [v, plays[i]]);

    const bestAlbum = new Map();

    //고유번호인 i를 계속 끌고가야한다.
    //map에다가 genre를 key로 하고 value를 {total : 0, songs: [{play, index}]}로 담아준다.
    genPlays.forEach(([genre, play], index) => {
        const data = bestAlbum.get(genre) || {total : 0, songs: []};

        bestAlbum.set(genre, {
            total : data.total + play,
            //play를 내림차순으로 정렬하고 최대 2개만 수록하니깐 slice로 2개만 남겨준다.
            songs : [...data.songs, {play, index}].sort((a,b) => b.play - a.play).slice(0,2)
        });
    })


    //배열안에 배열안에 객체가 있어서 단순히 map으로는 index만 가져올 수없다
    //flatMap으로 map을 펴준다
    return [...bestAlbum].sort((a,b) => b[1].total - a[1].total).flatMap(v => v[1].songs).map(song => song.index);
}
```
