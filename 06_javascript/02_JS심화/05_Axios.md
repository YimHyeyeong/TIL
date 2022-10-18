## Axios

* Axios
  
  * **"Promise based HTTP client for the brower and Node.js"**
  
  * 브라우저를위한 Promise 기반의 클라이언트
  
  * 원래는 "XHR" 이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리하는데 이보다 편리한 AJAX 요청이 가능하도록 도움을 줌
    
    * 확장 가능한 인터페이스와 함께 패키지로 사용이 간편한 라이브러리를 제공
  
  ```
  axios.get('https://~~') // Promise
      .then(..)
      .catch(..)
  ```

* XMLHttpRequest -> Axios 변경
  
  ```javascript
  const request = new ZMLHttpRequest()
  const URL = 'https://~~'
  
  request.open('GET', URL)
  request.send()
  
  const todo = request.response
  console.log(todo)
  ```
  
  ```javascript
  const URL = 'https://~~'
  
  axios.get(URL)
      .then(response => {
          console.log(response.data)
      })
  ```

* 
  
  
