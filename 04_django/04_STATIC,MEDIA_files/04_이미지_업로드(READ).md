## 이미지 업로드 (READ)

* 이미지 경로 불러오기

  * article.image.url == 업로드 파일의 경로
  * article.image == 업로드 파일의 파일 이름

  ```
  # html
  
  <img src="{{article.image.url}}" alt="{{article.image}}">
  ```

* STATIC_URL과 MEDIA_URL

  * static, media 결국 모두 서버에 요청해서 조회하는 것
  * 서버에 요청하기 위한 url을 urls.py가 아닌 settings에 먼저 작성 후 `urlpatterns`에 추가하는 형식