## 소수 구하기

소수란? 소수는 1 또는 자기 자신만을 약수로 가지는 수를 의미한다. 즉, 1과 자기 자신 외에는 어떤 값으로도 나눌 수 없다. (1은 소수 X)

1. 가장 직관적인 방법

- 2부터 n-1까지 나눠지는지 확인하기
- 가장 느린 방법(시간복잡도 O(n))

  ```
  function is_prime(num){
      for(let i=2; i<num; i++){
          if(num % i === 0) return false;
      }

      return true;
  }
  ```

2. 효율성 개선하기

- 그 어떤 소수도 N의 제곱근보다 큰 수로 나눠지지 않는다
- 시간복잡도 O(sqrt(n))

  ```
  function is_prime(num){
      for(let i=2; i*i <= num; i++){
          if(num % i === 0) return false;
      }

      return true;
  }
  ```

3. 에라토스테네스의 체

- 고대 그리스 수학자 에라토스테네스가 발견한 소수를 찾는 방법
- 시간 복잡도 O(n log log n)

  ```
  function get_primes(num){
      //num까지 소수를 찾는 알고리즘

      const result = [];
      const prime = [false, false, ...Array(num-1).fill(true)]; //0과 1빼고 다 소수의 후보군으로 등록

      //에라토스테네스의 체 알고리즘
      for(let i=2; i*i<=num; i++){
          if(prime[i]){
              for(let j=i*2; j<=num; j+=i){
                  prime[j] = false;
              }
          }
      }

      //소수 출력
      for(let k=0; k<num; k++){
          if(prime[k]){
              result.push(k);
          }
      }

      return result;
  }

  console.log(get_primes(8)); //[2,3,5,7]
  ```
