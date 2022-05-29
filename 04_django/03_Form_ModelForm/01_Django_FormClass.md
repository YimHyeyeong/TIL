## Django Form Class

* Intro
  * Django는 일부 과중한 적업과 박복 코드를 줄여줌으로써, 이 작업을 훨씬 쉽게 만들어 줌
    * `Django Form`
* Django's forms
  * `Form`은 Django 프로젝ㅌ트의 주요 유효성 검사 도구들 중 하나이며, 공격 및 우연한 데이터 손상에 대한 중요한 방어수단
  * Django는 form 기능의 방대한 부분을 단순화하고 자동화 할 수 있으며 프로그래머가 직접 작성한 코드에서 수행할 수 있는 것보다 더 안전하게 수행할 수 있음
  * Django는 `Form`에 관련된 작업의 아래 세부분을 처리해 줌
    1. 렌더링을 위한 데이터 준비 및 재구성
    2. 데이터에 대한 `HTML forms` 생성
    3. 클라이언트로부터 받은 데이터 수신 및 처리
* The Django 'Form Class'
  * Django Form 관리 시스템의 핵심
  * form 내 field, `field` 배치, 디스플레이 widget, label, 초기값, 유효하지 않은 field에 관련된 에러 메세지를 결정
  * Django는 사용자의 데이터를 받을 때 해야 할 과중한 작업(데이터 유효성 검증, 필요 시 입력된 데이터 검증 결과 재출력, 유효한 데이터에 대해 요구되는 동작 수행 등)과 반복 코드를 줄여 줌

* Form 선언하기

  ```
  # articles/forms.py
  
  from django import forms
  
  class ArticleForm(forms.Form):
  	title = forms.CharField(max_length=10)
  	content = forms.CharField()
  ```

  * `Model`을 선언하는 것과 유사하며 같은 필드타입을 사용(또한, 일부 매개변수도 유사함)
  * forms 라이브러리에서 파생된 `Form` 클래스를 상속받음

* Form 사용하기

  ```
  # articles/views.py
  
  from .forms import ArticleForm
  
  def new(request):
  	form = AricleForm()
  	context = {
  		'form': form,
  	}
  	return render(request, 'articles/new.html', context)
  ```

  ```
  # new.html
  
  {% block content %}
  	<form actions="{% url ' articles:create' %}" method="POST">
  	{%csrf_token%}
  	{{form.as_p}}
  	<input type="submit">
  	</form>
  {%endblock%}
  ```

* From rendering options

  * `<label>` & `<input>` 쌍에 대한 3가지 출력 옵션

  1.  as_p()
     * 각 필드가 단락(`<p>` 태그)으로 감싸져서 렌더링 됨
  2. as_ul()
     * 각 필드가 목록 항목(`<li>` 태그)으로 감싸져서 렌더링 됨
     * `<ul>` 태그는 직접 작성해야 함
  3. as_table()
     * 각 필드가 테이블(`<tr>` 태그)행으로 감싸져서 렌더링 됨
     * `<table>` 태그는 직접 작성해야 함

* Django의 HTML input 요소 표현 방법 2가지

  1. `Form fields`
     * input에 대한 유효성 검사 로직을 처리하며 템플릿에서 직접 사용됨
  2. `Widgets`
     * 웹 페이지의 HTML input 요소 렌더링
     * GET/POST 딕셔너리에서 데이터 추출
     * 하지만 `widgets`은 반드시 form fields에 할당됨

* Widgets

  * Django의 `HTML input element` 표현
  * HTML 렌더링을 처리
  * 주의사항
    * `Form Fields`와 혼동되어서는 안됨
    * `Form Fields`는 input 유효성 검사를 처리
    * Widgets은 웹페이지에서 **Input element**의 단순 raw한 렌더링 처리

  ```
  # articles/forms.py
  
  from django import forms
  
  class ArticleForm(forms.Form):
  	title = forms.CharField(max_length=10)
  	content = forms.CharField(widget=Forms.Textarea)
  ```

* Form field 및 widget 응용

  ```
  from django import forms
  
  class ArticleForm(forms.Form):
  	REGION_A = 'sl'
  	REGION_B = 'dj'
  	REGION_C = 'gm'
  	REGION_D = 'bs'
  	REGION_E = 'sl'
  	REGIONS_CHOICES = [
  		(REGION_A, '서울'),
  		(REGION_B, '대전'),
  		(REGION_C, '광주'),
  		(REGION_D, '구미'),
  		(REGION_E, '부산'),
  	]
  	title = forms.CharField(max_length=10)
  	content = forms.CharField(widget=Forms.Textarea)
  	region = forms.ChoiceField(choices=ReGions_CHOICES, widget=forms.Select())
  ```

  