## ModelForm

* Intro

  * Article 모델이 있고 사용자가 게시글을 제출할 수 있는 양식을 만들고 싶은 경우 이미 모델에서 필드를 정의했기 때문에 `form`에서 필드를 재정의 하는 중복된 행위 발생
  * 그래서 Django는 Model을 통해 `Form Class`를 만들 수 있는 Helper를 제공
    * ***ModelForm***

* ModelForm Class

  * Model을 통해 `Form Class`를 만들 수 있는 Helper
  * 일반 `Form Class`와 완전히 같은 방식(객체 생성)으로 view에서 사용 가능

* ModelForm 선언하기

  ```
  # articles/forms.py
  
  from django import forms
  from .models import Article
  
  class ArticleForm(forms.ModelForm):
  	class Meta:
  		model = Article
  		fields = '__all__'
  ```

  * forms 라이브러리에서 파생된 `ModelForm` 클래스를 상속받음
  * 정의한 클래스 안에 Meta 클래스를 선언하고 어떤 모델을 기반으로 form을 작성할 것인지에 대한 정보를 Meta클래스에 지정

* Meta class

  * `Model`의 정보를 작성하는 곳
  * `ModelForm`을 사용할 경우 사용할 모델이 있어야 하는데 **Meta class**가 이를 구성함
    * 해당 model에 덩희한 field 정보를 Form에 적용하기 위함
  * [참고] Inner class(Nested class)
    * 클래스 내에 선언된 다른 클래스
    * 관련 클래스를 함께 그룹화하여 가독성 및 프로그램 유지 관리를 지원(논리적으로 묶어서 표현)
    * 외부에서 내부 클래스에 접근할 수 없으므로 코드의 복잡성을 줄일 수 있음
  * [참고] Meta 데이터
    * "데이터에 대한 데이터"
    * ex) 사진촬영 - 사진 데이터 - 사진의 메타 데이터 (촬영 시각, 렌즈, 조리개 값 등)

* create view 수정

  ```
  # articles/views.py
  
  def create(request):
  	form = ArticleForm(request.POST)
  	if form.is_valid():
  		article = form.save()
  		return redirect('articles:detail', article.pk)
  	return redirect('articles:new')
  ```

* is_valid() method

  * 유효성 검사를 실행하고 데이터가 유효한지 여부를 `boolean`으로 반환
  * 데이터 유효성 검사를 보장하기 위한 많은 테스트에 대해 django는 **is_valid()**를 제공
  * [참고] 유효성 검사
    * 요청한 데이터가 특정 조건에 충족하는지 확인하는 작업
    * 데이터베이스 각 필드 조건에 올바르지 않은 데이터가 서버로 전송되거나 저장되지 않도록 하는 것

* The save() method

  * form에 바인딩 된 데이터에서 데이터베이스 객체를 만들고 저장
  * `ModelForm`의 하위(sub)클래스는 기존 모델 인스턴스를 키워드 인자 ***instance***로 받아 들일 수 있음
    * 이것이 제공되면 save()는 해당 인스턴스를 업데이트(UPDATE)
    * 제공되지 않은 경우 save()는 지정된 모델의 새 인스턴스를 만듦(CREATE)
  * form의 유효성이 확인되지 않은 경우(hasn't been validated) save()를 호출하면 form.errors를 확인하여 에러 확인 가능

* The save() method

  ```
  # Create a form instance from POST data.
  form = ArticleForm(request.POST)
  
  # CREATE
  # Save a new Article object from the form's data.
  new_article = f.save()
  
  # UPDATE
  # Create a form to edit an existing Article, but use POST data to populate the form
  article = Article.objects.get(pk=1)
  form = ArticleFOrm(request.POST, instance=article)
  form.save()
  ```

* create view 함수 구조 변경

  ```
  # article/views.py
  
  def create(request):
  	if request.method == 'POST':
  		form = ArticleForm(request.POST)
  		if form.is_valid():
  			article = form.save()
  			return redirect('articles:detail', article.pk)
  	else:
  		form = ArticleForm()
  	contextx = {
  		'form' : form,
  	}
  	return render(request, 'articles/create.html', context)
  ```

  ```
  # create.html
  
  <form action="{% url 'articles:create' %}" method="POST">
  	{% csrf_token %}
  	{{ form.as_p}}
  	<input type="submit">
  </form>
  ```

* WIdgets 적용하기

  * django의 HTML input element 표현
  * HTML 렌더링을 처리
  * 2가지 작성 방식을 가짐

  ```
  # articles/forms.py
  
  class ArticleForm(forms.ModelForm):
  	class Meta:
  		model = Article
  		fields = '__all__'
  		widgets = {
  			'title': forms.TextInput(attrs={
  				'class' : 'title',
  				'placeholder' : 'Enter the title',
  				'maxlength' : 10,
  			})
  		}
  ```

  * 첫 번째 방식

  ```
  # articles/forms.py
  
  class ArticleForm(forms.ModelForm):
  	title = forms.CharField(
  		label='제목',
  		widget=forms.TextInput(
  			attrs={
  				'class': 'my-title',
  				'placeholder' : 'Enter the title',
  			}
  		),
  	)
  	class Meta:
  		model = Article
  		fields = '__all__'
  ```

  * 두번째 방식(권장)

* DELETE

  ```
  # views.py
  
  def delete(request, pk):
  	article = Article.objects.get(pk=pk)
  	if request.method == 'POST':
  		article.delete()
  		return redirect('articles:index')
  	return redirect('articles:detail', article.pk)
  ```

  ```
  # detail.html
  
  <form action="{% url 'articles:delete' article.pk %}" method="POST">
  	{% csrf_token %}
  	<input type="submit" value="DELETE">
  </form>
  ```

* UPDATE

  ```
  # articles/views.py
  
  def update(request, pk):
  	article = Article.objects.get(pk=pk)
  	if request.method = 'POST':
  		form = ArticleForm(request.POST, instance=article)
  		if form.is_valid():
  			form.save()
  			return redirect('articles:detail', article.pk)
  	else:
  		form = ArticleForm(instance=article)
  	context = {
  		'form':form,
  		'article' :article
  	}
  	return render(request, 'articles/update.html', context)
  ```

  ```
  # updata.html
  
  <form action="{%url 'articles: update' article.pk %}" method="POST">
  	{% csrf_token %}
  	{{ form.as_p }}
  	<input type="submit">
  </form>
  ```

* Form & ModelForm

  * Form
    * 어떤 model에 저장해야 하는 지 알 수 없으므로 유효성 검사 이후 `cleaned_data` 딕셔너리를 생성
    * `cleaned_data` 딕셔너리에서 데이터를 가져온 후 **.save()** 호출해야 함
    * model에 연관되지 않은 데이터를 받을 때 사용
  * ModelForm
    * django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의
    * 어떤 레코드를 만들어야 할 지 알고 있으므로 바로 **.save()**호출 가능