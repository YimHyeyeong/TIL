## DOM(Document Object Model)

* 브라우저에서 할 수 있는 일
  
  * DOM 조작
    
    * 문서(HTML)조작
  
  * BOM 조작
    
    * navigator, screen, location, frames, history, XHR
  
  * JavaScript Core (ECMAScript)
    
    * Data Structure(Object, Array), Conditional Expresstion, Iteration

* DOM 이란?
  
  * HTML, XML 과 같은 문서를 다루기 위한 문서 프로그래밍 인터페이스
  
  * 문서를 구조화하고 구조화된 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델
  
  * 문서가 구조화되어 있으며 각 요소는 객체(object)로 취급
  
  * 단순한 속성 접근, 메서드 활용뿐만 아니라 프로그래밍 언어적 특성을 활용한 조작 가능
  
  * 주요 객체
    
    * `window`: DOM을 표현하는 창. 가장  최상위 객체 (작성 시 생략 가능)
    
    * `document`: 페이지 컨텐츠의 Entry Point역할을 하며 <body>등과 같은 수많은 다른 요소들을 포함
    
    * navigator, location, history, screen

* DOM

![](assets/2022-07-03-12-38-45-image.png)

* DOM - 해석
  
  * 파싱(Parsing)
    
    * 구문 분석, 해석
    
    * 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정

* BOM 이란?
  
  * Brower Object Model
  
  * 자바스크립트가 브라우저와 소통하기 위한 모델
  
  * 브라우저의 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
    
    * 버튼,URL입력창, 타이틀 바 등 브라우저 윈도우 및 웹 페이지 일부분을 제어 가능
  
  * window 객체는 모든 브라우저로부터 지원받으며 브라우저의 창(window)를 지칭

   
