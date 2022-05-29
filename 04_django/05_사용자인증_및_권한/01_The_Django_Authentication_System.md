## The Django Authentication System

* The Django Authentication System
  * Django 인증 시스템은 django.contrib.auth에 `Django contrib module`로 제공
  * 필수 구성은 settings.py에 이미 포함되어 있으며 INSTALLED_APPS 설정에 나열된 아래 두 항목으로 구성됨
    1. `django.contrib.auth`
       * 인증 프레임워크의 핵심과 기본 모델을 포함
    2. `django.contrib.contenttypes`
       * 사용자가 생성한 모델과 권한을 연결할 수 있음
  * Django 인증 시스템은 **인증(Authentication)**과 **권한(Authorization)** 부여를 함께 제공(처리)하며 이러한 기능이 어느 정도 결합되어 일반적으로 인증 시스템이라고 함
* Authentication & Authorization
  * `Authentication` (인증)
    * 신원 확인
    * 사용자가 자신이 누구인지 확인하는 것
  * `Authorization` (권한, 허가)
    * 권한 부여
    * 인증된 사용자가 수행할 수 있는 작업을 결정

