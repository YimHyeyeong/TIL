## Like

* Like 구현
  
  * ManyToManyField 작성
  
  ```
  # models.py
  
  class Article(models.Model):
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
      like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles`)
  ```
  
  - url 작성
  
  ```
  # urls.py
  
  urlpatterns = [
      ...
      path('<int:article_pk>/likes/', views.likes, name='likes)
  ]
  ```
  
  - like view 함수 작성
  
  ```
  # articles/views.py
  
  
  def likes(request, article_pk):
       if request.user.is_authenticated:
           article = get_object_or_404(Article, pk=article_pk)
           if article.like_users.filter(pk=request.user.pk).exists():
               article.like_users.remove(request.user)
           else:
               article.like_users.add(request.user)
           return redirect('articles:index')
       return redirect('accounts:login')
  ```

* QuerySet API - `exists()`
  
  * QuerySet에 결과가 포함되어 있으면 `True`를 반환하고 그렇지 않으면 `False`를 반환
  
  * 특히 규모가 큰 QuerySet의 컨텍스트에서 특정 개체 존재 여부와 관련된 검색에 유용
  
  * 고유한 필드(예: primary key)가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법
