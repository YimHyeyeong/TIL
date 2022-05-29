## 이미지 Resizing

* 이미지 크기 변경하기

  * 실제 원본 이미지를 서버에 그대로 업로드 하는 것은 서버의 부담이 큰 작업
  * <img> 태그에서 직정 사이즈를 조정할 수 있지만(width와 height), 업로드 될 때 이미지 자체를 resizing하는 것을 사용해 볼 것
  * django-imagekit 라이브러리 활용

  1. django-imagekit 설치
  2. INSTALLED_APPS에 추가

  ```
  $ pip install django-imagekit
  ```

  ```
  # settings.py
  
  INSTALLED_APP = [
  	'imagekit'
  ]
  ```

  * 원본 이미지를 재가공하여 저장(원본x, 썸네일 o)

  ```
  # models.py
  
  from imagekit.models import ProcessedImageField
  from imagekit.processors import Thumbnail
  
  class Article(models.Model):
  	image = ProcessedImageField(
  		blank=True,
  		processors=[Thumbnail(200,300)],
  		format='JPEG',
  		options={'quality':9}
  	)
  ```

  * ProcessedImageField()의 parameter로 작성된 값들은 변경이 되더라도 다시 makemigrations를 해줄 필요없이 즉시 반영 됨