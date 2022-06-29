## User - Comment(1:N)

* User와 Comment 간 모델 관계 정의 후 migration

  ```
  # articles/models.py
  
  class Comment(models.Model):
  	article =models.ForeignKey(Article, on_delete=models.CASCADE)
  	user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
  ```

* 댓글 출력 필드 수정

  * 게시글 작성 페이지에서 불필요한 필드가 출력되는 것을 확인

  * 댓글 작성 시 user ForeignKeyField를 출력하지 않도록 설정

    ```
    # articles/forms.py
    
    class CommentForm(forms.ModelForm):
    	class Meta:
    		model = Comment
    		exclude('article','user',)
    ```

  * 댓글 작성 시 NOT NULL constraing failed: articles_comment.user_id 에러 발생

  * 댓글 작성 시 작성자 정보(comment.user)가 누락되었기 때문

* CREATE

  * 댓글 작성 시 작성자 정보(request.user) 추가 후 댓글 작성 재시도

    ```
    # articles/views.py
    
    def comments_create(reqeust, pk):
    	if reqeust.user.is_authenticated:
    		article = get_object_or_404(Article, pk=pk)
    		comment_form = CommentForm(request.POST)
    		if comment_form.is_vaild():
    			comment = comment_form.save(commit=False)
    			comment.article = article
    			comment.usr = request.user
    			comment.save()
    			...
    ```

* READ

  * 비로그인 유저에게는 댓글 form 출력 숨기기

    ```
    # articles/detail.html
    
    {% if reqeust.user.is_authenticated %}
    	<form action="{% user 'articles:comments_create' article.pk %}" method="POST">
    {% else %}
    ...
    {% endif %}
    ```

  * 댓글 작성자 출력하기

    ```
    # articles/detail.html
    
    {% for comment in comments %}
    	<li>
    	{{ comment.user }} - {{comment.content}}
    	</li>
    {% empty %}
    ...
    {% endfor %}
    ```

* DELETE

  * 자신이 작성한 댓글만 삭제 버튼을 볼 수 있도록 수정

    ```
    # articles/detail.html
    
    {% for comment in comments %}
    	<li>
    	{% if user == comment.user %}
    	...
    	{% endif %}
    	</li>
    {% empty %}
    ...
    {% endfor %}
    ```

  * 자신이 작성한 댓글만 삭제 할 수 있도록 수정

    ```
    # articles/views/py
    
    def comments_delete(request, article_pk, comment_pk):
    	if request.user.is_authenticated:
    		comment = get_onject_or_404(Comment, pk=comment_pk)
    		if request.user = comment.user:
    			comment.delete()
    	return redirect('articles:detail', article_pk)
    ```

    