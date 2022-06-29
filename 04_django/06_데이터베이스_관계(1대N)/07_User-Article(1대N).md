## User - Article (1:N)

* User 모델 참조하기

  1. **settings.AUTH_USER_MODEL**
     * User 모델에 대한 외래 키 또는 다대다 관계를 정의할 때 사용해야 함
     * models.py에서 User 모델을 참조할 때 사용
  2. **get_user_model()**
     * 현재 활성화(active)된 User 모델을 반환
       * 커스터마이징한 User모델이 있을 경우는 Custom User모델, 그렇지 않으면 User를 반환
       * User를 직접 참조하지 않는 이유
     * models.py가 아닌 다른 모든 곳에서 유저 모델을 참조할 때 사용

* User와 Article 간 모델 관계 정의 후 migration

  ```
  # articles/models.py
  
  from django.conf import settings
  
  class Article(models.Model):
  	user = models.ForeignKey(settings.AUTH_USER_MODEL, ondelete=models.CASCADE)
  ```

* 게시글 출력 필드 수정

  * 게시글 작성 페이지에서 불필요한 필드가 출력되는 것을 확인

  * `ArticleForm`의 출력 필드 수정 후게시글 작성 재시도

    ```
    # articles/forms.py
    
    class ArticleForm(forms.ModelForm):
    	class Meta:
    		model = Article
    		fields = ('title','content',)
    ```

  * 게시글 작성 시 `NOT NULL constraint failed: article_article.user_id` 에러 발생

  * 게시글 작성 시 작성자 정보(article.user)가 누락되었기 때문

* CREATE

  * 게시글 작성 시 작성자 정보(article.user) 추가 후 게시글 작성 재시도

    ```
    # articles/views.py
    
    def create(request):
    	if request.method == 'POST':
    		form = ArticleForm(request.POST)
    		if form.is_valid():
    			article = form.save(commit=False)
    			article.user = request.user
    			article.save()
    			return redirect('articles:detail', article.pk)
    ```

* DELETE

  * 자신이 작성한 게시글만 삭제 가능하도록 설정

    ```
    # articles/views.py
    
    def delete(request, pk):
    	article = get_object_or_404(Article, pk=pk)
    	if request.user.is_authenticated:
    		if request.user == article.user:
    			article.delete()
    			return redirect('articles:index')
    	return redirect('articles:detail',article.pk)
    ```

* UPDATE

  * 자신이 작성한 게시물만 수정 가능하도록 설정

    ```
    # articles/views.py
    
    def update(request,pk):
    	article = get_object_or_404(Article, pk=pk)
    	if request.user == article.user:
    		if request.method == 'POST':
    			form = ArticleForm(request.POST, instance=article)
    			if form.is_valid():
    				form.save()
    				return redirect('articles:detail', article.pk)
    	else:
    		return redirect('articles:index')
    	context = {
    		'form':form
    	}
    	return render(request, 'articles/update.html', context)
    ```

* READ

  * 게시글 작성 user가 누구인지 `index.html`에서 출력하기

    ```
    # articles/index.html
    
    {% for article in articles %}
    	<p>{{article.user}}</p>
    	<a href="{% user 'articles:detail' article.pk %}">DETAIL</a>
    {% endfor %}
    ```

    * 해당 게시글의 작성자가 아니라면 수정/삭제 버튼을 출력하지 않도록 처리

    ```
    # articles/detail.html
    
    {% if user == article.user %}
    ...
    {% endif %}
    ```

    