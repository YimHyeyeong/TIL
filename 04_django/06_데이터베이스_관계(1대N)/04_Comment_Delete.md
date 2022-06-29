## Comment Delete

```
# urls.py

app_name = 'articles'
urlpatterns = [
	path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
]
```

```
# html

<ul>
	{% for comment in comments %}
	<li>
		{{ comment.content }}
		<form action="{ % url 'articles:comments_delete' article.pk comment.pk %}" method="POST">
		{% csrf_token %}
		<input type="submit" value="DeLETE">
		</form>
	</li>
	{% endfor %}
</ul>
```

```
# views.py

@require_POST
def comments_delete(request, article_pk, comment_pk):
	comment = get_object_or_404(Comment, pk=comment_pk)
	comment.delete()
	return redirect('articles:detail', article_pk)
```

* 인증된 사용자의 경우만 댓글 작성 및 삭제

  ```
  # views.py
  
  @require.POST
  def comment_create(request, pk):
  	if request.user.is_authenticated:
  		...
  	return redirect('accounts:login')
  	
  @require_POST
  def comments_delete(request, article_pk, comment_pk):
  	if request.user.is_authenticated:
  		...
  	return redirect('articles:detail', article_pk)
  ```





## Comment 추가사항

* 댓글 개수 출력하기

  1. `{{comments|length}}`
  2. `{{article.comment_set.all|length}}`
  3. `{{comments.count}}`

  ```
  # detail.html
  
  {% if comments %}
  	<p>{{comments|length}}</p>
  {% endif %}
  ```

* 댓글이 없는 경우 대체 컨텐츠 출력 (DTL의 for-empty 태그 활용)

  ```
  # html
  
  {% for comment in comments %}
  	<li>
  		{{ comment.content }}
  		<form action="{ % url 'articles:comments_delete' article.pk comment.pk %}" method="POST">
  		{% csrf_token %}
  		<input type="submit" value="DeLETE">
  		</form>
  	</li>
  {% empty %}
  	<p>댓글이 없어요..</p>
  {% endfor %}
  ```

  