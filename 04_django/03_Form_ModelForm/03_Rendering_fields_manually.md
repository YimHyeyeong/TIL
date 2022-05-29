## Rendering fields manually

* 수동으로 Form 작성하기
  1. Rendering fields manually
  2. Looping over the form's fields ({% for %})

1. Rendering fields manually

   ```
   # articles/create.html
   
   <form action="" method="POST">
   	{{form.title.errors}}
   	{{form.title.label_tag}}
   	{{form.title}}
   	{{form.content.errors}}
   	{{form.content.label_tag}}
   	{{form.content}}
   </form>
   ```

2. Looping over the form's fields

   ```
   # articles/create.html
   
   <form action="" method="POST">
   	{% for field in form %}
   		{{field.errors}}
   		{{field.label_tag}}
   		{{field}}
   </form>
   ```

* Bootstrap과 함께 사용하기
  1. Bootstrap class with widgets
  2. Django Bootstrap 5 Library

1. Bootstrap Form class

   * 핵심 클래스

     * `form-control`

   * `Bootstrap Form`의 핵심 class를 widget에 작성

     ```
     # forms.py
     
     class ArticleForm(forms.ModelForm):
     	title = forms.CharField(
     		label='제목',
     		widget=forms.TextInput(
     			attrs={
     				'class' :'my-title form-control',
     				'rows':5,
     				'cols':50,
     		}),
     		error_messages={
     		'required':'내용 넣어라...'
     		}
     	)
     ```

   * 에러 메시지 `with bootstrap alert` 컴포넌트

     ```
     # create.html
     
     {% if field.errors %}
     	{% for error in field.errors %}
     		<div class="alert alert-warning" role="alert"> {{error|escape}} </div>
     	{%endfor%}
     {%endif%}
     ```

2. Django Bootstrap 5 Library

   * django-bootstrap v5
     * form class에 bootstrap을 적용시켜주는 라이브러리

   ```
   $ pip install django-bootstrap-v5
   ```

   ```
   # settings.py
   
   INSTALLED_APPS = [
   	...
   	'bootstrap5',
   ]
   ```

   ```
   # base.html
   
   {% load bootstrap5 %}
   ..
   {% bootstrap_css %}
   <title></title>
   <body>
   ..
   {%bootstrap_javascript%}
   </body>
   ```

   