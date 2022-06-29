## Foreign Key

* Foreign Key 개념

  * 외래 키 (외부 키)
  * 관계형 데이터베이스에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
  * 참조하는 테이블에서 1개의 키(속성 또는 속성의 집합)에 해당하고 이는 참조되는 측 테이블의 **기본 키**(Primary Key)를 가리킴
  * 참조하는 테이블의 행 1개의 값은, 참조되는 측 테이블의 행 값에 대응됨
    * 이 때문에 참조하는 테이블의 행에는 참조되는 테이블에 나타나지 않는 값을 포함할 수 없음
  * 참조하는 테이블의 행 여러 개가 참조되는 테이블의 동일한 행을 참조할 수 있음

* Foreign Key 예시

  * Article - Comment
    * 참조하는 모델(Comment)에서 외래 키는 참조되는 축 모델 (Article)의 기본 키 (Primary Key)를 가리킴

* Foreign Key 특징

  * 키를 사용하여 부모 테이블의 유일한 값을 참조 (참조 무결성)
  * 외래 키의 값이 반드시 부모 테이블의 기본 키 일 필요는 없지만 유일한 값이어야 함
  * [참고] 참조 무결성
    * 데이터베이스 관계 모델에서 관련된 2개의 테이블 간의 일관성을 말함
    * 외래 키가 선언된 테이블의 외래 키 속성(열)의 값은 그 테이블의 부모가 되는 테이블의 기본 키 값으로 존재해야 함

* **Foreign Key** field

  * `A many-to-one relationship`

  * 2개의 위치 인자가 반드시 필요

    1. 참조하는 `model class`
    2. `on_delete` 옵션

  * migrate 작섭 시 필드 이름에 _id를 추가하여 데이터베이스 열 이름을 만듦

  * [참고] 재귀 관계 (자신과 1:N)

    ```
    models.ForeignKey('self', on_delete=models.CASCADE)
    ```

  * comment 모델 정의하기

    ```
    class  Comment(models.Model):
    	article = models.ForeignKey(Article, on_delete=models.CASCADE)
    	...
    ```

* **ForeignKey** arguments = 'on_delete'

  * 외래 키가 참조하는 객체가 사라졌을 때 외래 키가 가진 객체를 어떻게 처리할 지를 정의
  * Database integrity(데이터 무결성)을 위해서 매우 중요한 설정
  * On_delete 옵션에 사용 가능한 값들
    * CASCADE: 부모 객체(참조 된 객체)가 삭제 됐을 때 이를 참조하는 객체도 삭제

* [참고] 데이터 무결성

  * 데이터의 정확성과 일관성을 유지하고 보증하는 것을 가리키며 데이터베이스나 RDBMS 시스템의 중요한 기능임
  * 무결성 제한의 유형
    1. 개체 무결성(Entity integrity)
       * PK의 개념과 관련
       * 모든 테이블이 PK를 가져야 하며 PK로 선택된 열은 고유한 값이어야 하고 빈 값은 허용치 않음을 규정
    2. 참조 무결성(Referential integrity)
       * FK(외래 키) 개념과 관련
       * FK 값이 데이터베이스의 특정 테이블의 PK값을 참조하는 것
    3. 범위(도메인) 무결성 (Domain integrity)
       * 정의된 형식(범위)에서 관계형 데이터베이스의 모든 컬럼이 선언되도록 규정

* 데이터베이스의 ForeignKeyField 표현

  * 만약 ForeginKey 인스턴스를 `abcd`로 생성했다면 `abcd_id`로 만들어짐
  * 하지만 명시적인 모델 관계 파악을 위해 참조하는 클래스 이름의 **소문자(단수형)**로 작성하는 것이 바람직함(1:N)

* 1:N 관계 related manager

  * 역참조(**'comment_set'**)
    * **Article(1) => Comment(N)**
    * `article.comment `형태로는 사용할 수 없고 article**.comment_set** manager가 생성됨
    * 게시글에 몇 개의 댓글이 작성 되었는지 Django ORM이 보장할 수 없기 때문
      * article은 comment가 있을 수도 있고 없을 수도 있음
      * **실제로 Article 클래스에는 Comment 와의 어떠한 관계도 작성되어 있지 않음**
  * 참조 ('article')
    * Comment(N) => Article(1)
    * 댓글의 경우 어떠한 댓글이든 반드시 자신이 참조하고 있는 게시글이 있으므로, **comment.article**과 같이 접근할 수 있음
    * 실제 ForeignKeyField 또한 `Comment` 클래스에서 작성됨

* 1:N related maneger 연습하기

  * `dir()` 함수를 통해 article 인스턴스가 사용할 수 있는 모든 속성, 메서드를 직접 확인하기

  * article의 입장에서 모든 댓글 조회하기 (역참조, 1 -> N)

    ```
    article.comment_set_all()
    ```

  * 조회한 모든 댓글 출력하기

    ```
    comments = article.comment_set.all()
    for comment in comments:
    	print(comment.content)
    ```

  * comment의 입장에서 참조하는 게시글 조회하기 (참조, N->1)

    ```
    comment = COmment.objects.get(pk=1)
    comment.article
    comment.article.content
    comment.article_id
    ```

* **ForeignKey** arguments - 'related_name'

  * 역참조 시 사용할 이름('model_set' manager)을 변경할 수 있는 옵션

    ```
    class Comment(models.Model):
    	article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments')
    ```

    - 위와 같이 변경하면 `article.comment_set`은 더이상 사용할 수 없고, **article.comments**로 대체됨
    - [주의] 역참조 시 사용할 이름 수정 후, migration 과정 필요