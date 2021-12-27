## URL

* Django URLs

  * Dispatcher(발송자 , 운항 관리자)로서의 URL
  * 웹 애플리케이션은 URL 을 통한 클라이언트의 요청에서부터 시작 됨

* Variable Routing

  * URL 주소를 변수로 사용하는 것
  * URL 의 일부를 변수로 지정하여 view 함수의 인자로 넘길 수 있음
  * 즉 변수 값에 따라 하나의 `path()` 에 여러 페이지를 연결 시킬 수 있음

  ```
  path(/accounts/user/<int:user_pk >/’,)
   • accounts/user/1 →→(1 번 user 관련 페이지)
   • accounts/user/2 →→(2 번 user 관련 페이지)
  ```

* URL Path converters

  * str
    * `/` 를 제외하고 비어 있지 않은 모든 문자열과 매치
    * 작성하지 않을 경우 기본 값
  * int
    * 0 또는 양의 정수와 매치
  * slug
    * ASCII 문자 또는 숫자 , 하이픈 및 밑줄 문자로 구성된 모든 슬러그 문자열과 매치

* App URL mapping

  * app 의 view 함수가 많아지면서 사용하는 path() 또한 많아지고 , app 또한 더 많이 작성되기 때문에 프로젝트의 urls.py 에서 모두 관리하는 것은 프로젝트 유지보수에 좋지 않음
  * 각 app 에 urls.py 를 작성 하게 됨

* Including other URLconfs

  * include()
    * 다른 URLconf (app1/urls. 들을 참조할 수 있도록 도움
    * 함수 include() 를 만나게 되면 , URL 의 그 시점까지 일치하는 부분을 잘라내고 , 남은 문자열 부분을 후속 처리를 위해 include 된 URLconf 로 전달
    
    ```
    url.py
    
    from django.contrib import admin
    from django.urls import path, include
    
    urlpattern = [
    	path('articles/', include('articles.urls'))
    ]
    ```
    
    + urlpattern은 언제든지 다른 `URLconf` 모듈을 포함할 수 있음
    
  * django 는 명시적 상대경로 `(from .module import ..)` 를 권장

* Naming URL patterns

  * 이제는 링크에 url 을 직접 작성하는 것이 아니라 path() 함수의 `name` 인자를 정의해서 사용

  * Django Template Tag 중 하나인 url 태그를 사용해서 path() 함수에 작성한 name 을 사용할 수 있음

    ```
    path('index/', views.index, name='index')
    ```

    ```
    <a href="{% url 'index' %}">home</a>
    ```

  * url 설정에 정의된 특정한 경로들의 의존성을 제거할 수 있음

* url template tag

  ```
  {% url %}
  ```

  * 주어진 URL 패턴 이름 및 선택적 매개 변수와 일치하는 절대 경로 주소를 반환
  * 템플릿에 URL을 하드 코딩하지 않고도 [DRY 원칙](https://docs.djangoproject.com/ko/3.2/misc/design-philosophies/#don-t-repeat-yourself-dry)을 위반하지 않으면서 링크를 출력하는 방법