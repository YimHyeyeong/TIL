## Web Framework

* Framework

  >  도구 더미

  * 프로그래밍에서 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스와 라이브러리 모임
  * 재사용 할 수 있는 수많은 코드를 프레임워크로 통합함으로써 개발자가 새로운 애플리케이션을 위한 표준 코드를 다시 작성하지 않아도 같이 사용할 수 있도록 도움
  * `Application Framework` 라고도 함

* Web framework
  * **웹 페이지를 개발하는 과정에서 겪는 어려움을 줄이는 것이 주 목적**으로 데이터베이스 연동, 템플릿 형태의 표준, 세션 관리, 코드 재사용 등의 기능을 포함
  * 동적인 웹 페이지나, 웹 애플리케이션, 웹 서비스 개발 보조용으로 만들어지는 `Application Framework`의 일종

* Django를 사용해야 하는 이유

  * 검증된 Python 언어 기반 `Web framwork`
  * 대규모 서비스에도 안정적이며 오랫동안 세계적인 기업들에 의해 사용됨
  * ex) Spotify / instagram

* Framework Architecture

  * `MVC Design Pattern` (model- view-controller)

    > `model`(DB), `view`(보여지는거), `control`(관리)를 분리해서 따로 관리할 수 있음. 편집을 할 때 서로에게 영향을 주지 않음

  * 소프트웨어 공학에서 사용되는 디자인 패턴 중 하나

  * 사용자 인터페이스로부터 프로그램 로직을 분리하여 애플리케이션의 시각적 요소나 이면에서 실행되는 부분을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있음

  * Django는 `MTV Pattern` 이라고 함 

    > MVC랑 같은 개념임

* MTV Pattern (== MVC)

  * `Model`

    * 응용프로그램의 데이터 구조를 정의하고 **데이터베이스**의 기록을 관리(추가, 수정, 삭제)

  * `Templete`

    * 파일의 구조나 레이아웃을 정의

    * 실제 내용으 보여주는 데 사용

      > MVC의 view랑 같음. html이랑 같음

  * `View `

    * HTTP 요청을 수신하고 HTTP 응답을 반환

    * `Model`을 통해 요청을 충족시키는데 필요한 데이터에 접근

    * `template`에게 응답의 서식 설정을 맡김

      > MVC의 controller