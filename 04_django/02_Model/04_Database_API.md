## Database API

+ DB API 

  > 파이썬으로 씀

  * "DB를 조작하기 위한 도구"
  * django가 기본적으로 ORM을 제공함에 따른 것으로 DB 를 편하게 조작할 수 있도록 도움
  * Model을 만들면 django는 객체들을 만들고 읽고 수정하고 지울 수 있는 `database abstract API`를 자동으로 만듦
  * `database abstract API` 혹은 `database access API` 라고도 함

+ DB API 구문 - Making Queries

  ```
  Article.objects.all()
  ClassName.Manager.QuerySet APi
  
  # 모든 게시글 조회하는 명령어
  ```

+ DB API

  * Manager

    * django모델에 데이터베이스 query 작업이 제공되는 인터페이스
    * 기본적으로 모든 django 모델 클래스에 `objects`라는 manager를 추가

  * QuerySet

    * 데이터베이스로부터 전달받은 객체 목록
    * `queryset` 안의 객체는 0 개 , 1 개 혹은 여러 개일 수 있음
    * 데이터베이스로부터 조회 , 필터 , 정렬 등을 수행 할 수 있음

  * Django shell

    * 일반 파이썬 쉘을 통해서는 장고 프로젝트 환경에 접근할 수 없음
    * 그래서 장고 프로젝트 설정이 로드 된 파이썬 쉘(+shell_plus)을 활용해
      DB API 구문 테스트
    * 기본 Django shell 보다 더 많은 기능을 제공하는 `shell_plus` 설치 후 진행

  * 실습

    ```
    $ pip install ipython
    ```

    ```
    $ pip install django-extensions
    ```

    ```
    # settings.py
    
    INSTALLED_APPS = [
    	...,
    	'django_extenstions',
    	...,
    ]
    ```

    ```
    $ python manage.py shell_plus
    ```

    

