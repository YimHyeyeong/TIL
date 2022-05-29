## Authentication data in templates

* 현재 로그인 되어있는 유저 정보 출력

  ```
  <h3> {{user}} </h3>
  ```

* Authentication data in templates

  * `context processors`
    * 템플릿이 렌더링 될 때 자동으로 호출 가능한 컨텍스트 데이터 목록
    * 작성된 프로세서는 `RequestContext`에서 사용 가능한 변수로 포함됨
  * `Users`
    * 템플릿 `RequestContext`를 렌더링할 때, 현재 로그인한 사용자를 나타내는 **auth.User** 인스턴스(또는 클라이언트가 로그인하지 않은 경우 **AnonymousUser** 인스턴스)는 템플릿 변수 ***{{user}}*** 에 저장됨
  * `Built-in template context processors`
    * **django.contrib.auth.context_processors.auth**
    * django.template.context_processors.debug
    * django.template.context_processors.i18n