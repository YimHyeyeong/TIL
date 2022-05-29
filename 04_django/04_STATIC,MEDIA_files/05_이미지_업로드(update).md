## 이미지 업로드(update)

* 이미지 수정하기

  * 이미지는 바이너리 데이터 (하나의 덩어리)이기 때문에 텍스트처럼 일부만 수정하는 것은 불가능
  * 때문에 새로운 사진으로 덮어 씌우는 방식을 사용

  ```
  # html
  
  <form action="{% url 'articles:update' article.pk}" method="POST" enctype="multipart/form-data">
  ```

  ```
  # view.py
  
  def update(request. pk):
  	form = ArticleForm(rerquest.POST, request.FILES, instance=article)
  ```

  * detail 페이지를 출력하지 못하는 문제 해결

    * image가 없는 게시글의 경우 출력할 이미지가 없기 때문

    ```
    {% if article.image %}
    	<img src="{{article.image.url}}" alt="{{article.image}}">
    {% endif %}
    ```

    