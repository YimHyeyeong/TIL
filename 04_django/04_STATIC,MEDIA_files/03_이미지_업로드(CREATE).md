## 이미지 업로드(CREATE)

* ImageField 작성

  * `upload_to='images/'`
    * 실제 이미지가 저장되는 경로를 지정
  * `blank=True`
    * 이미지 필드에 빈 값(빈 문자열)이 허용되도록 설정 (이미지를 선택적으로 업로드 할 수 있도록)

  ```
  # models.py
  
  class Article(models.Model):
  	image = models.ImageField(blank=True, upload_to='images/')
  ```

* Model field option - "blank"

  * 기본 값: `False`
  * True인 경우 필드를 비워 둘 수 있음
    * DB에는 **''(빈 문자열)**이 저장됨
  * 유효성 검사에서 사용 됨 (is_valid)
    * 필드에 blank=True가 있ㅇ으면 form 유효성 검사에서 빈 값을 입력할 수 있음

* Model field option - "null"

  * 기본 값: `False`
  * True면 django는 빈 값을 DB에 **NULL**로 저장
  * 주의 사항
    * CharField, TextField와 같은 ***문자열 기반 필드에는 사용하는 것을 피해야 함***
    * 문자열 기반 필드에 True로 설정 시 *'데이터 없음(no data)'*에 **"빈 문자열(1)"**과 **"NULL(2)"**의 2가지 가능한 값이 있음을 의미하게 됨
    * 대부분의 경우 "데이터 없음"에 대해 두 개의 가능한 값을 갖는 것은 중복되며, Django는 NULL이 아닌 빈 문자열을 사용하는 것이 규칙

* blank & null 비교

  * blank
    * Validation-related
  * null
    * Database-related
  * 문자열 기반 및 비문자열 기반 필드 모두에 대해 null option은 DB에만 영향을 미치므로, form에서 **빈 값을 허용하려면** `blank=True`를 설정해야 함

  ```
  # models.py
  
  class Person(models.Model):
  	# null = True 금지
  	bio = models.TextField(max_length=50, blank=True)
  	
  	# null.blank 모두 설정 가능 -> 문자열 기반 필드가 아니기 때문
  	birth_date = models.DateField(null=True, blank=True)
  ```

  ```
  # create.html
  
  <form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
  ```

* form 요소 - enctype(인코딩) 속성

  1. multipart/form-data
     * 파일/이미지 어 ㅂ로드 시에 반드시 사용해야 함 (전송되는 데이터의 형식을 지정)
     * `<input thpe="file">`을 사용할 경우에 사용
  2. application/x-www-form-urlencoded
     * (기본값) 모든 문자 인코딩
  3. text/plain
     * 인코딩을 하지 않은 문자 상태로 전송
     * 공백은 '+' 기호로 변환하지만, 특수 문자는 인코딩 하지 않음

* input 요소 - accept 속성

  * 입력 허용할 파일 유형을 나타내는 문자열
  * 쉼표로 구분된 "고유 파일 유형 지정자" (unique file type specifiers)
  * **파일 검증을 하는 것은 아님** 
    * (이미지만 accept 해 놓더라도 비디오나 오디오 파일을 제출할 수 있음)
  * 고유 파일 유형 지정자
    * `<input type="file">`에서 선택할 수 있는 파일의 종류를 설명하는 문자열
  * 파일 업로드 시 허용할 파일 형식에 대해 자동으로 필터링

* Views.py 수정

  * 업로드 한 파일은 request.FILES 객체로 전달됨

  ```
  # views.py
  
  def create(request):
  	form = ArticleForm(request.POST, request.FILES)
  ```

  