## CRUD

+ 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 `Create`(생성), `Read`(읽기), `Update`(갱신), `Delete`(삭제)를 묶어서 일컫는 말

+ 조회

  + DB에 인스턴스 객체를 얻기 위한 쿼리문 날리기
  + 이때 레코드가 하나만 있으면 **인스턴스 객체**로, 두 개 이상이면 **쿼리셋**으로 리턴

  ```
  Article.objects.all()
  
  # 전체 article 객체 조회
  ```

+ CREATE

  + `CREATE` 첫번째 방법

  ```
  # 특정 테이블에 새로운 행을 추가하여 데이터 추가
  
  > article = Article() # Article(class)로부터 article(instance)
  > article
  <Article:Article object (None)>
  
  > article.title = 'first' # 인스턴스 변수(title)에 값을 할당
  > article.content = 'django' # 인스턴스 변수(content)에 값을 할당
  ```
  
  ```
  # save를 하지 않으면 아직 DB에 값이 저장되지 않음
  > article
  <Article:Article object (None)>
  
  >Article.objects.all()
  <QuerySet []>
  ```
  
  ```
  # save를 하고 확인하면 저장된 것을 확인할 수 있다
  > article.save()
  > article
  <Article:Article object (1)>
  
  > Article.objects.all()
  <QuerySet [Article:Article object(1)]>
  ```
  
  ```
  # 인스턴스인 article을 활용하여 변수에 접근해보자 (저장된 걸 확인)
  > article.title
  'first'
  > article.content
  'django'
  > article.created_at
  ```
  
  + `CREATE` 두번째 방법
  
  ```
  > article = Article(title='second', content='django')
  
  # 아직 저장이 안되어 잇음
  > article
  <Article:Article object (None)>
  ```
  
  ```
  # save를 해주면 저장이 됨
  > article.save()
  
  > article
  <Article:Article object (2)>
  
  > Article.object.all()
  <QuerySet [<Article: Article object (1)>, <Article: Article object (2)>]>
  ```
  
  ```
  # 값 확인
  > article.pk
  2
  > article.title
  'second'
  > article.content
  'django'
  ```
  
  + `CREATE` 세번째 방법
  
  ```
  # 위의 2개의 방식과는 다르게 바로 쿼리 표현식 리턴
  > Artciel.objects.create(title='third', content='django')
  <Article: Article object (3)>
  ```
  
+ CREATE 관련 메서드

  + save() method
    + `Saving objects`
    + 객체를 데이터베이스에 저장
    + 데이터 생성 시 save()를 호출하기 전에는 객체의 `ID` 값이 무엇인지 알 수 없음

+ str method

  ```
  class Articlce(models.Model):
  	title = ...,
  	...
  	
  	def __str__(self):
  		return self.title
  ```

  + 표준 파이썬 클래스의 메소드인 `str()` 을 정의하여 각각의 object가 사람이 읽을 수 있는 문자열을 반환하도록 할 수 있음
  + 작성 후 반드시 `shell_plus` 재시작

+ READ

  + `QuerySet API method`를 사용한 다양한 조회를 하는 것이 중요
  + 크케 2가지로 분류
    1. Methods that return new querysets
    2. Methods that do not return querysets
  + all()
    + QuerySet return
    + 리스트는 아니지만 리스트와 거의 비슷하게 동작

  ```
  > Article.objects.all()
  <QuerySet [<Article~~~>, <Article...>]
  ```

  + get()

    > 주어진 lookup 매개변수와 일치하는 객체를 반환

    + 객체가 없으면 `DoesNotExist` 에러가 나오고 객체가 여러 개일 경우에 `MultipleObjectReturned` 오류를 띄움
    + 위와 같은 특징을 가지고 있기 때문에 unique 혹은 Not Null 특징을 가지고 있으면 사용할 수 있음

  ```
  > article = Article.objects.get(pk=100)
  DoesNotExist: Article matching query does not exist.
  
  > Article.objects.get(content='django')
  MultipleObjectsReturned: get() returnedmore than one Article -- it returned 2!
  ```

  + filter()

    > 없는걸 필터해도 에러 안뜸 pk가 아닌것을 필터하는것이 더 나음

    + 지정된 조회 매개 변수와 일치하는 객체를 포함하는 새 `QuerySet`을 반환

  ```
  > Article.objects.filter(content='django')
  <QuerySet [<Article:first>, <Article:third>]>
  
  > Article.objects.filter(title='first')
  <QuerySet [<Article: first>]>
  ```

  > get은 하나의 객체를 반환 filter 은 퀴러셋을 반환
  >
  > 리셋은 유사 리스트라서 리스트에 접근하듯이 해야 됨

+ UPDATE

  + article 인스턴스 객체의 인스턴스 변수의 값을 변경

  ```
  # UPDATE articles SET title='byebye' WHERE id=1;
  > article = Article.objects.get(pk=1)
  > article.title
  'first'
  ```

  ```
  # 값을 변경하고 저장
  > article.title = 'byebye'
  > article.save()
  ```

  ```
  # 정상적으로 변경된 것을 확인
  > article.title
  'byebye'
  ```

+ DELETE

  + article 인스턴스 생성 후 `.delete()` 호출

  ```
  > article = Article.objects.get(pk=1)
  
  # 삭제
  > article.delete()
  (1, {'articles.Article': 1})
  
  # 1번은 이제 찾을 수 없음
  > Article.objects.get(pk=1)
  DoesNotExist: Article matching query does not exist.
  ```

+ Field lookups

  + SQL WHERE 절을 지정하는 방법
  + 조회 시 특정 조선을 적용시키기 위해 사용
  + `QuerySet` 메서드 `filter()`, `exclude()` 및 `get()`에 대한 키워드 인수로 지정
  + ex)
    + Article.objects.filter(pk__gt=2)
    + Article.objects.filter(content__contains='ja')

> QuerySet Api method 공식문서
>
> https://docs.djangoproject.com/en/3.2/ref/models/querysets/#queryset-api-reference