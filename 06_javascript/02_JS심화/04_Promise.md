## Promise

* Promise object
  
  * 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
    
    * 미래의 완료 또는 실패와 그 결과 값을 나타냄
    
    * 미래의 어떤 상황에 대한 약속
  
  * 성공(이행)에 대한 약속
    
    * `.then()`
  
  * 실패(거절)에 대한 약속
    
    * `.catch()`
  
  ```javascript
  const myPromise = axios.get(URL)
  
  myPromise
      .then(response => {
          return response.data
      })
  axios.get(URL)
      .then(response => {
          return response.data
      })
      .catch(error => {
          console.log(error)
      })
      .finally(function () {
          console.log('마지막에 무조건 시행')
      })
  ```

* Promise methods
  
  * `.then(callback)`
    
    * 이전 작업(promise)이 성공했을 때(이행했을 때) 수행할 작업을 나타내는 callback 함수
    
    * 그리고 각 callback 함수는 이전작업의 성공 결과를 인자로 전달받음
    
    * 따라서 성공했을 때의 코드를 callback 함수 안에 작성
  
  * `.catch(callback)`
    
    * .then이 하나라도 실패하면(거부 되면) 동작 (동기식의 'try - except` 구문과 유사)
    
    * 이전 작업의 실패로 인해 생성된 error 객체는 catch 블록 안에서 사용할 수 있음
  
  * 각각의 `.then()`블록은 서로 다른 promise를 반환
    
    * 즉, `.then()`을 여러 개 사용(chaning)하여 연쇄적인 작업을 수행할 수 있음
    
    * 결국 여러 비동기 작업을 차례대로 수행할 수 있다는 뜻
  
  * `.then()`과 `.catch()` 메서드는 모두 promise를 반환하기 때문에 `chaiining`가능
  
  * 주의
    
    * 반환 값이 반드시 있어야 함
    
    * 없다면 callback 함수가 이전의 promise 결과를 받을 수 없음
  
  * `.finally(callback)`
    
    * Promise 격체를 반환
    
    * 결과와 상관없이 무조건 지정된 callback 함수가 실행
    
    * 어떠한 인자도 전달받지 않음
      
      * Promise가 성공되었는지 거절되었는지 판단할 수 없기 때문
    
    * 무조건 실행되어야 하는 절에서 활용
      
      * .`then()`과 `.catch()` 블록에서의 코드 중복을 방지

* callback Hell -> Promise
  
  ```javascript
  work1(funcsion(result1) {
      work2(result1, function(result2) {
          work3(result2, funcsion(result3) {
              console.log(result3)
          })
      })
  })
  ```
  
  ```javascript
  work1().then(function(result1) {
      return work2(result1)
  })
  .then(function(result2) {
      return work3(result2)
  })
  .then(function(result3) {
      console.log(result3)
  })
  .catch(failureCallback)
  ```

* Promise 가 보장하는 것
  
  * Async callback 작성 스타일과 달리 Promise가 보장하는 특징
  1. callback 함수는 JavaScript의 Event Loop가 현재 실행 중인 Call Stack을 완료하기 이전에는 절대 호출되지 않음
     
     - Promise callback 함수는 Event Queue에 배치되는 엄격한 순서로 호출됨
  
  2. 비동기 작업이 성고하거나 실패한 뒤에 `.then()` 메서드를 이용하여 추가한 경우에도 1번과 똑같이 동작
  
  3. `.then()`을 여러 번사용하여 여러개의 callback 함수를 추가할 수 있음(Chaining)
     
     - 각각의 callback은 주어진 순서대로 하나하나 실행하게 됨
     
     - Chaining은 Promise의 가장 뛰어난 장점
