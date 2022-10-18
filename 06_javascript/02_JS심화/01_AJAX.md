## AJAX

* AJAX 란
  
  * `Asynchronous JavaScript And XML` (비동기식 JavaScript와 XML)
  
  * 서버와 통신하기 위해 `XMLHttpRequest` 객체를 활용
  
  * JSON, XML, HTML 그리고 일반 텍스트 형식 등을 포함한 다양한 포맷을 주고 받을 수 있음
    
    * [참고] AJAX의 X가 XML을 의미하긴 하지만, 요즘은 더 가벼운 용량과 JavaScript의 일부라는 장점때문에 JSON을 더 많이 사용함

* AJAX특징
  
  * 페이지 전제를 reload(새로고침)를 하지 않고서도 수행되는 **"비동기성"**
    
    * 사용자의 event가 있으면 전체 페이지가 아닌 일부분만을 업데이트할 수 있음
  
  * AJAX의 주요 두가지 특징은 아래의 작업을 할 수 있게 해줌
    
    1. 페이지 새로고침 없이 서버에 요청
    
    2. 서버로부터 데이터를 받고 작업을 수행

* XMLHttpRequest 객체
  
  * 서버와 상호작용하기 위해 사용되며 전체 페이지의 새로고침 없이 데이터를 받아올 수 있음
  
  * 사용자의 작업을 방해하지 않으면서 페이지 일부를 업데이트 할 수 있음
  
  * 주로 AJAX 프로그래밍에 사용
  
  * 이름과 달리 XML뿐만 아니라 모든 종류의 데이터를 받아올 수 있음
  
  * 생성자
    
    * `XMLHttpRequest()`

* XMLHttpRequest 예시
  
  * console에 todo 데이터가 출력되지 않음
  
  * 데이터응답을 기다리지 않고 `console.log()`를 먼저 실행했기 때문
  
  ```
  const request = new XMLHttpRequest()
  const URL = 'https://~~'
  
  request.open('GET', URL)
  request.send()
  
  const todo = reqeust.response
  console.log(`data: ${todo}`)
  ```
  
  
