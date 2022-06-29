## Substituting a custom User model

* User 모델 대체하기

  * 일부 프로젝트에서는 Django의 **내장 User 모델이 제공하는 인증 요구사항이 적절하지 않을 수 있음**
    * ex) username 대신 email을 식별 토큰으로 사용하는 것이 더 적합한 사이트
  * Django는 User를 참고하는데 사용하는 `AUTH_USER_MODEL` 값을 제공하여 `default user model`을 **재정의(override)**할 수 있도록 함
  * Django는 새프로젝트를 시작하는 경우 기본 사용자 모델이 충분하더라도, ***커스텀 유저 모델을 설정하는 것을 강력하게 권장***(highly recommended)
    * **단, 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 함**

* AUTH_USER_MODEL

  * User를 나타내는데 사용하는 모델
  * 프로젝트가 **진행되는 동안 변경할 수 없음**
  * 프로젝트 시작 시 설정하기 위한 것이며 참조하는 모델은 첫번째 마이그레이션에서 사용할 수 있어야 함
  * 기본 값: `auth.User` (auth 앱의 User 모델)
  * [참고] 프로젝트 중간(mid-project)에 `AUTH_USER_MODEL` 변경하기
    * 모델 관계에 영향을 미치기 때문에 훨씬 더 어려운 작업이 필요
    * 즉, 중간 변경을 권장하지 않으므로 초기에 설정하는 것을 권장

* Custom User 모델 정의하기

  * 관리자 권한과 함께 완전한 기능을 갖춘 User 모델을 구현하는 기본 클래스인 `AbstractUser`를 상속받아 새로운 User 모델 작성

  ```
  # models.py
  
  from django.contrib.auth.models import AbstractUser
  
  class User(AbstractUser):
  	pass
  ```

  * 기존에 django가 사용하는 User 모델이었던` auth` 앱의 User 모델을 `accounts` 앱의 User 모델을 사용하도록 변경

  ```
  # settings.py
  
  AUTH_USER_MODEL = 'accounts.User'
  ```

  * admin site에 Custom User 모델 등록

  ```
  # admin.py
  
  from django.contrib import admin
  from django.contrib.auth.admin import UserAdmin
  from .models import User
  
  admin.site.register(User, UserAdmin)
  ```