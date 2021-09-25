## Web Framework

* Static web page(정적 웹 페이지)

  * 서버에 미리 저장된 파일이 사용자에게 그대로 전달되는 웹 페이지
  * 서버가 정적 웹 페이지에 대한 요청을 받은 경우 서버는 추가적인 처리 과정 없이 클라이언트에게 응답을 보냄
  * 모든 상황에서 모든 사용자에게 동일한 정보를 표시
  * 일반적으로 `HTML`, `CSS` ,`JavaScript` 로 작성됨
  * `flat page`라고도 함
* Dynamic web page(동적 웹 페이지)

  * 웹 페이지에 대한 요청을 받은 경우 서버는 추가추가적인 처리 과정 이후 클라이언트에게 응답을 보냄
  * 동적 페이지는 방문자와 상호작용하기 때문에 페이지 내용은 그때그때 다름
  * 서버 사이드 프로그래밍 언어 (`python`, `java`, `c++` 등) 가 사용되며 파일을 처리하고 데이터베이스와의 상호작용이 이루어짐





## Template

* Django Template

  * "데이터 표현을 제어하는 도구이자 표현에 관련된 로직"
  * 사용하는 built-in system
    * `Django Template Language` (DTL)

* Django Template Language(DTL)

  * django template 에서 사용하는 `built - in template system`
  * 조건 , 반복 , 변수 치환 , 필터 등의 기능을 제공
  * 단순히 Python이 HTML 에 포함 된 것이 아니며 프로그래밍적 로직이 아니라 **프레젠테이션을 표현하기 위한 것**
  * Python 처럼 일부 프로그래밍 구조 (if, for 등)를 사용할 수 있지만 , 이것은 해당 Python 코드로 실행되는 것이 아님

* DTL Syntax - Variable

  ```
  {{variable(변수명)}}
  ```

  * render()를 사용하여 `view.py` 에서 정의한 변수를 `template`파일로 넘겨 사용하는 것
  * 변수명은 영어, 숫자와 밑줄(_)의 조합으로 구성될 수 있으나 밑줄로는 시작 할 수 없음
    * 공백이나 구두점 문자 또한 사용할 수 없음
  * dot(.)를 사용하여 변수 속성에 접근할 수 있음  ex) a.0 = a[0]
  * render() 세번째 인자로 `{'key': value}` 와 같이 딕셔너리 형태로 넘겨주며 여기서 정의한 key에 해당하는 문자열이 template에서 사용가능한 변수명이 됨

* DTL Syntax - Filters

  ```
  {{variable|filter}}
  ```

  > `|`앞뒤로 띄우쓰기 금지

  * 표시할 변수를 수정할 때 사용
  * 60 개의 built in template filters 를 제공
  * `chained `가 가능하며 일부 필터는 인자를 받기도 함

* DTL Syntax - Tags

  > 태그 하나에 그 의미를 가지고 있음

  ```
  { % tag %}
  ```

  * 출력 텍스트를 만들거나 반복 또는 논리를 수행하여 제어 흐름을 만드는 등 변수보다 복잡한 일들을 수행
  * 일부태그는 시작과 종료태그가 필요 ex) if 태그
  * 약 24 개의 built in template tags 를 제공

* DTL Syntax - comments

  ```
  {# #}
  ```

  * django template 에서 라인의 주석을 표현하기 위해 사용
  * 아래처럼 유효하지 않은 템플릿 코드가 포함될 수 있음
  * 한 줄 주석에만 사용할 수 있음, 줄 바꿈이 허용되지 않음
  * 여러 줄 주석은 {% comment%} 와 {% endcomment %}사이에 위치

* 코드 작성 순서

  * 데이터의 흐름에 맞추어 작성
    1. `urls.py`
    2. `views.py`
    3. `templates`

* Template inheritance (템플릿 상속)

  * 템플릿 상속은 기본적으로 코드의 재사용성에 초점을 맞춤
  * 템플릿 상속을 사용하면 사이트의 모든 공통 요소를 포함하고 , 하위 템플릿이 재정의 (override) 할 수 있는 블록을 정의하는 기본 `skeleton` 템플릿을 만들 수 있음

* Template inheritance - "tags"

  ```
  {% extends '' %}
  ```

  * 자식 하위 템플릿이 부모 템플릿을 확장한다는 것을 알림
  * 반드시 템플릿 최상단에 작성 되어야 함

  ```
  {% block content %} {% endblock %}
  ```

  * 하위 템플릿에서 재지정 ( overriden )할 수 있는 블록을 정의
  * 즉 , 하위 템플릿이 채울 수 있는 공간

* Django template system (feat.django 설계 철학)

  * “표현과 로직 ( view) 을 분리”

    * 템플릿 시스템은 표현을 제어하는 도구이자 표현에 관련된 로직일 뿐dl라고 생각한다
    * 즉 , 템플릿 시스템은 이러한 기본 목표를 넘어서는 기능을 지원하지 말아야 한다

  * •“중복을 배제”

    * 대다수의 동적 웹사이트는 공통 header, footer, navbar 같은 사이트 공통 디자인을 갖는다
    * Django 템플릿 시스템은 이러한 요소를 한 곳에 저장하기 쉽게 하여 중복 코드를 없애야 한다

    