## CSRF

+ 사이트 간 요청 위조(Croass-Site-Request-Forgery)

  + 웹 애플리케이션 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법
  + django는 CSRF에 대항하여 `middleware` 와 `template tag` 를 제공

+ CSRF 공격 방어

  + `Security Token` 사용 방식(CSRF Token)
    + 사용자의 데이터에 임의의 난수 값을 부여해 매 요청마다 해당 난수 값을 포함시켜 전송시키도록 함
    + 이후 서버에 요청을 받을 때마다 전달된 `token`값이 유효한지 검증
  + 일반적으로 데이터 변경이 가능한 `POST`, `PATCH`, `DELETE Method` 등에 적용 (GET 제외)
  + django는 `csrf token` 템플릿 태그를 제공

+ csrf token in Django

  ```
  {% csrf_token %}
  ```

  + input type이 `hidden`으로 작성되며 value는 django에서 생성한 `hash`값이 들어 있음
  + 해당 태그가 없다면 Django 서버는 `403 forbidden`을 응담

+ CsrfViewMiddleware

  + 해당 `csrf attack` 보안과 관련된 설정은 settings.py 에서 `MIDDLEWARE` 에 되어있음
  + 실제로 요청 과정에서 urls.py 이전에 `Middleware` 의 설정 사항들을 순차적으로 거치며 응답은 반대로 하단에서 상단으로 미들웨어를 적용시킴

+ Middleware

  + 공통 서비스 및 기능을 애플리케이션에 제공하는 소프트웨어
  + 데이터 관리 , 애플리케이션 서비스 , 메시징 , 인증 및 API 관리를 주로 미들웨어를 통해 처리
  + 개발자들이 애플리케이션을 보다 효율적으로 구축할 수 있도록 지원하여 애플리케이션 , 데이터 및 사용자 사이를 연결하는 요소처럼 작동