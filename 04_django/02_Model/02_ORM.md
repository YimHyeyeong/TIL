## ORM

+ ORM

  * **object-Relaional-Mapping**
  * 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 (Django - SQL)데이터를 변환하는 프로그래밍 기술
  * OOP 프로그래밍에서 `RDBMS`을 연동할 때 데이터베이스와 객체 지향 프로그래밍 언어 간에 호환되지 않는 데이터를 변환하는 프로그래밍 기법
  * Django는 내장 `Django ORM`을 사용

+ ORM의 장점과 단점

  * 장점
    * SQL을 잘 알지 못해도 `DB `조작이 가능
    * SQL의 절차적인 접근이 아닌 **객체 지향적 접근**으로 인한 높은 생산성
  * 단점
    * ORM 만으로 완전한 서비스를 구현하기 어려운 경우가 있음
  * 현대 웹 프레임워크의 요점은 웹 개발의 속도를 높이는 것(**생산성**)

+ 왜 ORM을 사용할까?

  * **"우리는 DB를 객체(object)로 조작하기 위해 ORM을 사용한다"**

+ models.py 작성

  ```
  # articles/models.py
  
  class Article(models.Model):
  	title =models.CharField(max_length=10)
  	content = models.TextField()
  ```

  + DB 컬럼과 어떠한 타입으로 정의할 것인지에 대해 `django.db` 라는 모듈의 models를 상속
    + 각 모델은 django.db.models.Model 클래스의 서브 클래스로 표현
  + title과 content은 모델의 필드를 나타냄
    + 각 필드는 클래스 속성으로 지정되어 있으며 각 속성은 각 데이터베이스의 열에 매핑
  + 사용 모델 필드
    + CharField(max_length-None, **options)
      + 길이의 제한이 있는 문자열을 넣을 때 사용
      + CharField 의 `max_length` 는 필수 인자
      + 필드의 최대 길이(문자), 데이터베이스 레벨과 Django 의 유효성 검사(값을 검증하는 것)에서 활용
    + TextField(**options)
      + 글자의 수가 많을 때 사용
      + max_length 옵션 작성시 자동 양식 필드인 `textarea` 위젯에 반영은 되지만
        모델과 데이터베이스 수준에는 적용되지 않음 (CharField 를 사용)