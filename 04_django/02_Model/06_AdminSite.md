## Admin Site

+ Automatic admin interface
  + 사용자가 아닌 서버의 관리자가 활용하기 위한 페이지
  + `Article class`를 admin.py에 등록하고 관리
  + `django.contrib.auth` 모듈에서 제공
  + `record` 생성 여부 확인에 매우 유용하며 직접 `record` 를 삽입할 수도 있음
  
+ admin 생성

  ```
  $ python manage.py createsuperuser
  ```

  + 관리자 계정 생성 후 서버를 실행한 다음 `/admin` 으로 가서 관리자 페이지 로그인
    + 계정만 만든 경우 `Django` 관리자 화면에서 아무 것도 보이지 않음
  + 내가 만든 record를 보기 위해서는 admin.py 에 작성하여 django 서버에 등록
  + auth에 관련된 기본 테이블이 생성되지 않으면 관리자 계정을 생성할 수 없음

+ admin 등록

  ```
  # articles/admin.py
  
  from django.contrib import admin
  from .models import Article
  
  # admin site에 register하겠다
  admin.site.register(Article)
  ```

  + admin.py는 관리자 사이트에 `Article` 객체가 관리 인터페이스를 가지고 잇다는 것을 알려주는 것
  + models.py 에 정의한 `__str__`의 형태로 객체가 표현됨

+ ModelAdmin options

  ```
  # articles/admin.py
  
  from django.contrib import admin
  from .models import Article
  
  class ArticleAdmin(admin.ModelAdmin):
  	list_display = ('pk','title','content','created_at','updated_at')
  	
  admin.site.register(Article, ArticleAdmin)
  ```

  + list_display
    + admin 페이지에서 우리가 `models.py` 정의한 각각의 속성(컬럼)들의 값(레코드)를 출력
    + list_filter, list_display_links 등 다양한 `ModleAdmin options` 참고
    + https://docs.djangoproject.com/en/3.2/ref/contrib/admin/#modeladmin-options