## Comment Read

* 댓글 출력

  * 특정 `article` 에 있는 모든 댓글을 가져온 후 `context` 에 추가

    ```
    # views.py
    
    from .models import Article, Comment
    
    def detail(request, pk):
         article = get_object_or_404(Article, pk=pk)
         comment_form = CommentForm()
         comments = article.comment_set.all()
         context = {
            'article': aritcle,
            'comment_form': comment_form,
            'comments': comments,
         }
         return render(request, 'articles/detail.html', context)
    ```

    ```
    # detail.html
    
    <ul>
    	{% for comment in comments %}
    		<li>{{comment.content}}</li>
    	{% endfor %}
    </ul>
    ```

    