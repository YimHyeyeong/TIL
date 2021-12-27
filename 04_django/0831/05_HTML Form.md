## HTML Form



* HTML “form” element

  * 웹에서 사용자 정보를 입력하는 여러 방식 (text, button, checkbox, file, hidden, image, password, radio, reset, submit) 을 제공하고 , 사용자로부터 할당된 데이터를 서버로 전송하는 역할을 담당
  * 핵심 속성
    * `action` : 입력 데이터가 전송될 URL 지정
    * `method` : 입력 데이터 전달 방식 지정

* HTML “input” element

  * 사용자로부터 데이터를 입력 받기 위해 사용
  * type 속성에 따라 동작 방식이 달라짐
  * 핵심 속성
    * `name`
    * 중복 가능 , 양식을 제출했을 때 name 이라는 이름에 설정된 값을 넘겨서 값을 가져올 수 있음
    * 주요 용도는 GET/POST 방식으로 서버에 전달하는 파라미터 (name 은 key , value 는 value)로

* HTML “label” element 

  * 사용자 인터페이스 항목에 대한 설명을 나타냄
  * label 을 input 요소와 연결하기
    1. input 에 `id` 속성 부여
    2. label 에는 input 의 id 와 동일한 값의 `for` 속성이 필요
  * label 과 input 요소 연결의 주요 이점
    * 시각적인 기능 뿐만 아니라 화면 리더기에서 label 을 읽어서 사용자가 입력해야 하는 텍스트가 무엇인지 더 쉽게 이해할 수 있도록 돕는 프로그래밍적 이점도 있음
    * label 을 클릭해서 input 에 초점을 맞추거나 활성화 시킬 수 있음

* HTML “for” attribute

  * for 속성의 값과 일치하는 id 를 가진 문서의 첫 번째 요소를 제어
    * 연결 된 요소가 labelable elements 인 경우 이 요소에 대한 labeled control이 됨
  * “labelable elements"
    * label 요소와 연결할 수 있는 요소
    * button, input(not hidden type), select, textarea

* HTML “id” attribute

  * 전체 문서에서 고유 (must unique) 해야 하는 식별자를 정의
  * 사용 목적
    * linking, scripting, styling 시 요소를 식별

* HTTP

  * `HyperText Transfer Protocol`
  * 웹에서 이루어지는 모든 데이터 교환의 기초
  * 주어진 리소스가 수행 할 원하는 작업을 나타내는 request methods 를 정의
  * `HTTP request method` 종류
    * GET, POST, PUT, DELETE …

* HTTP request method - "GET"

  * 서버로부터 정보를 조회 하는 데 사용

  * 데이터를 가져올 때만 사용해야 함

  * 데이터를 서버로 전송할 때 body 가 아닌 `Query String Parameters` 를 통해 전송

    > ​	quert string parameters -> (= ?key=velue&)

  * 우리는 서버에 요청을 하면 HTML 문서 파일 한 장을 받는데 이때 사용하는 요청의 방식이 GET