## Comment Create

* CommentForm 작성

  ```
  # forms.py
  
  from .models import Article, Comment
  
  class CommentForm(forms.ModelForm):
  	class Meta:
  		model =Comment
  		fields = '__all__'
  ```

* detail 페이지에서 CommentForm 출력

  ```
  # views.py
  
  from .forms import ArticleForm, CommentForm
  
  def detail(request, pk):
  	article = get_object_or_404(Article, pk=pk)
  	comment_form = CommentForm()
  	context = {
  		'article':article,
  		'comment_form': comment_form,
  	}
  	return render(request, 'articles/detail.html', context)
  ```

  ```
  # detail.html
  
  <form action="" method="POST">
  	{% csrf_token %}
  	{{ comment_form}}
  	<input type="submit">
  </form>
  ```

  * `foreignKeyField`를 작성자가 직접 입력하는 상황이 발생
  * `CommentForm`에서 외래키 필드 출력 제외

  ```
  # form.py
  
  class CommentForm(forms.ModelForm):
  	class Meta:
  		model = Comment
  		exclude = ('article',)
  ```

* 댓글 작성 로직

  ```
  # urls.py
  
  app_name = 'articles'
  urlpatterns = [
  	path('<int:pk>/<comments/', views.comments_create, name='comments_create'),
  ]
  ```

  ```
  # detail.html
  
  <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
  	{% csrf_token %}
  	{{comment_form}}
  	<input type="sobmit">
  </form>
  ```

  ```
  # views.py
  
  @require_POST
  def comment_create(request, pk):
  	article = get_object_or_404(Article, pk=pk)
  	comment_form = CommentForm(request.POST)
  	if comment_form.is_valid():
  		comment = comment_form.save(commit=False)
  		comment.article= article
  		comment.save()
  	return redirect('articles:detail', article.pk)
  ```

* The 'save()' method

  * save(commit=False)
    * `Create, but don't save the new instance`
    * 아직 **데이터베이스에 저장되지 않은 인스턴스를 반환**
    * 저장하기 전에 **객체에 대한 사용자 지정 처리를 수행할 때** 유용하게 사용