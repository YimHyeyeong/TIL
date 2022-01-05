## Memoization

* 앞의 예에서의 피보나치 수를 구하는 함수를 재귀함수로 구현한 알고리즘은 문제점이 있다.

* "엄청난 중복 호출이 존재한다"는 것이다.

* 메모이 제이션은 컴퓨터 프로그램을 실핼할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행속도를 빠르게 하는 기술이다. 동저계획법의 핵심이 되는 기술이다.

* 앞의 예에서 피보나치 수를 구하는 알고리즘에서 fibo(n)의 값을 계산하자마자 저장하면(`memoize`) 실행시간을 `o(n)`으로 줄일 수 있다.

  * memo를 위한 배열을 할당하고 모두 0으로 초기화 한다.
  * memo[0]을 0으로 memo[1]는 1로 초기화한다.

  ```
  def fibo(n):
  	global memo
  	if n >= 2 and len(memo) <= n:
  		memo.append(fibo(n-1) + fibo(n-2))
  	return memo[n]
  	
  memo = [0,1]
  ```

  ```
  def fibo(n):
  	if n>=2 and memo[n]==0: #아직 계산되지 않는 값이면
  		memo[n] = fibo(n-1) + fibo(n-2)
  	return memo[n]
  	
  memo = [0] * (n+1)
  memo[0] = 0
  memo[1] = 1
  print(fibo(n))
  ```

  