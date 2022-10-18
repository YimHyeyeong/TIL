## RESTful API

* API
  
  * `Application Programming Interface`
  
  * 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
    
    * 애플리케이션과 프로그래밍으로 소통하는 방법
    
    * CLI는 명령줄, GUI는 그래픽(아이콘), API는 프로그래밍을 통해 특정한 기능 수행
  
  * Web API
    
    * 웹 애플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세
    
    * 현재 웹 개발은 모든 것을 직접 개발하기보다 여러 Open API를 활용하는 추세
  
  * 응답 데이터 타입
    
    * HTML, XML, JSON 등

* REST
  
  * **RE**presentational **S**tate **T**ransfer
  
  * API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론
    
    * 2000년 로이 필딩의 박사학위 논문에서 처음으로 소개 된 후 네트워킹 문화에 널리 퍼짐
  
  * 네트워크 구조(Network Architecture)원리의 모음
    
    * 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법
  
  * REST 원리를 따르는 시스템을 RESTful 이란 용어로 지칭함
  
  * 자원을 정의하는 방법에 대한 고민
    
    * ex) 정의된 자원을 어디에 위치 시킬 것인가
  
  * REST의 자원과 주소의 지정 방법
    
    1. 자원
       
       * `URI`
    
    2. 행위
       
       * `HTTP Method`
    
    3. 표현
       
       * 자원과 행위를 통해 궁극적으로 표현되는 (추상화된) 결과물
       
       * JSON으로 표현된 데이터를 제공

* JSON
  
  * **JSON (JavaScript Object Notation)**
    
    * `JSON is a lightweight data-interchange format`
    
    * JavaScript의 표기법에 따른 단순 문자열
  
  * 특징
    
    * 사람이 읽거나 쓰기 윕고 기계가 파싱(해석, 분석)하고 만들어내기 쉬움
    
    * 파이썬의 `dictionary,` 자바스크립트의 `object`처럼 C계열의 언어가 갖고 있는 자료구조로 쉽게 변화할 수 있는 key-value형태의 구조를 갖고 있음
  
  * REST의 핵심 규칙
    
    1. '정보'는 URL로 표현
    
    2. 자원에 대한 '행위'는 HTTP Method로 표현 (GET, POST, PUT, DELETE)
  
  * 설계 방법론은 지키지 않았을 때 잃는 것보다 지켰을 때 얻는 것이 훨씬 많은
    
    * 단, 설계 방법론을 지키지 않더라도 동작 여부에 큰 영향을 미치지는 않음

* RESTful API
  
  * REST 원리를 따라 설계한 API
  
  * `RESTful services`, 혹은 `simply REST services`라고도 부름
  
  * 프로그래밍을 통해 클라이언트의 요청에 JSON을 응답하는 서버를 구성
    
    * 지금까지 사용자의 입장에서 썼던 API를 제공자의 입장이 되어 개발해보기
