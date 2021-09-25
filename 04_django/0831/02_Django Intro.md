## Django Intro

* Django 시작하기
  1. 가상환경 생성: `python -m venv venv `
  2. 가상환경 활성화: `source venv/Scripts/activate`
  3. pip list 확인
  4. `pip install django`
  5. 프로젝트 폴더 생성 : `django-admin startproject config .`



* 프로젝트 구조

  * `__init__.py`

    * Python 에게 이 디렉토리를 하나의 Python 패키지로 다루도록 지시

  * `asgi.py`

    * Asynchronous Server Gateway Interface
    * django 애플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도움

  * `settings.py`

    * 애플리케이션의 모든 설정을 포함

  * `urls.py`

    * 사이트의 url과 적절한 views의 연결을 지정

  * `wsgi.py`

    * Web Server Gateway Interface

    * django 애플리케이션이 웹서버와 연결 및 소통하는 것을 도움

      > asgi.py와 wsgi,py는 배포할 때 사용함

  * `manage.py`

    * django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인 유틸리티

      ```
      $ python manage.py <command> [options]
      ```





* Application 생성

  * 일반적으로 Application명은 복수형으로 하는 것을 권장

    > Application : 로그인/로그아웃/회원가입/탈퇴/ 글쓰기/ 삭제 와 같은 기능을 담당함 근데 우리가 만들어야 함

    ```
    $ python manage.py startapp <어플리케이션 명>
    ```

* Application 구조

  * `admin.py`

    * 관리자용 페이지를 설정하는 곳

  * `apps.py`

    * 앱의 정보가 작성되는 곳

  * `models.py`

    * 앱에서 사용되는 Model을 정의하는 곳

      > DB를 정의

  * `tests.py`

    * 프로젝트의 테스트 코드를 작성하는 곳

      > TDD

  * `views.py`

    * view 함수들이 정의되는 곳





* Project & Application

  * Project

    * Project는 Application의 집합
    * 프로젝트에는 여러 앱(=기능)이 포함될 수 있음
    * 앱은 여러 프로젝트에 있을 수 있음

  * Application

    * 앱은 실제 요청을 처리하고 페이지를 보여주는 하는등의 역할을 담당

    * 하나의 프로젝트는 여러 앱을 가짐

    * 일반적으로 앱은 하나의 역할 및 기능 단위로 작성함

      > MTV에서 M,V의 기능을 담당함
      >
      > config는 어플리케이션이 아님, 설정 폴더이며 Project에 대한 설정을 담당함
      >
      > application은 startapp으로 생성된 친구들, project는 startproject로 생성

  > 앱을 생성하고 등록할때 순서를 지켜줘야 함 가장 최근에 생성한 앱은 가장 위쪽에 배치/ 중간 단계는 외부에서 pip install로 설치한 것 중 장고에 등록해서 사용하는 것을 적음/ 장고 기본앱은 가장 마지막에



* 앱 등록
  * 프로젝트에서 앱을 사용하기 위해서는 반드시 `INSTALLED_APPS`리스트에 추가해야 함
  * `INSTALLED_APPS`
    * Django installation 에 활성화 된 모든 앱을 지정하는 문자열 목록
  * 앱 생성 시 주의 사항
    * **반드시 생성 후 등록**
    * `INSTALLED_APPS` 에 먼저 작성하고 생성하려면 앱이 생성되지 않음

