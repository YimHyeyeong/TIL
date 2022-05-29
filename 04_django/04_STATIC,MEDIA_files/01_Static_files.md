## Static files

* Static file

  * 정적 파일
  * 응답할 때 별도의 처리 없이 파일 내용을 그대로 보여주면 되는 파일
    * 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 보여주는 파일
  * 예를 들어 웹 사이트는 일반적으로 이미지, 자바 스크립트 또는 CSS와 같은 미리 준비된 추가 파일(움직이지 않는)을 제공해야 함
  * Django에서는 이러한 파일을 `static file` 이라 함

* Static file 구성

  1. `django.contrib.staticfiles`가 INSTALLED_APPS에 포함되어 있는지 확인

  2. setting.py에서 `STATIC_URL`을 정의

  3. 템플릿에서 **static 템플릿 태그**를 사용하여 지정된 상대경로에 대한 URL을 빌드

     ```
     {% load static %}
     <img src="{% tatic 'my_app/ex.jpg' %}" alt="My IMG">
     ```

  4. 앱의 static 폴더에 정적 파일을 저장

* Django template tag

  * load

    * 사용자 정의 템플릿 태그 세트를 로드
    * 로드하는 라이브러리, 패키지에 등록된 모든 태그와 필터를 로드

  * static

    * STATIC_ROOT엥 저장된 정적 파일에 연결

      ```
      {% load static %}
      
      <img src="{% tatic 'my_app/ex.jpg' %}" alt="My IMG">
      ```

* The staticfiles app

  * STATIC_ROOT

    * `collectstatic`이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로
    * django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아 넣는 경로
    * 개발 과정에서 setting.py의 **DEBUG** 값이 `True`로 설정되어 잇으면 해당 값은 작용되지 않음
      * 직접 작성하지 않으면 django 프로젝트에서는 setting.py에 작성되어 있지 않음
    * 실 서비스 환결(배포 환경)에서 django의 모든 정적 파일을 다른 웹 서버가 직접 제공하기 위함

  * STATIC_URL

    * STATIC_ROOT에 있는 정적 파일을 참조 할 때 사용할 URL
      * 개발 단계에서는 실제 정적 파일들이 저장되어 있는 `app/static/경로(기본 경로) 및 STATICFILES_DIRS`에 정의된 추가 경로들을 탐색함
    * 실제 파일이나 디렉토리가 아니며 URL로만 존재
    * 비어 있지 않은 값으로 설정 한다면 반드시 **slash(/)**로 끝나야 함

    ```
    STATIC_URL = '/static/'
    ```

  * STATICFILES_DIRS

    * app/static/디렉토리 경로를 사용하는 것(기본 경로) 외에 추가적인 정적 파일 경로 목록을 정의하는 리스트
    * 추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목옥으로 작성되어야 함

* [참고] collectstatic

  * STATIC_ROOT에 정적 파일을 수집

  ```
  # STATIC_ROOT 작성
  
  STATIC_ROOT = BASE_DIR / 'staticfiles'
  ```

  ```
  # collectstatic 명령어
  
  $ python manage.py collectstatic
  ```

* 정적파일 사용하기 - 기본 경로

  * 정적 파일 경로

    ```
    app/static/app/
    
    ex)
    -articles
    ㄴstatic
    	ㄴarticles
    		ㄴsample.jpg
    ```

  * template에서 경로 참조

    ```
    # detail.html
    
    {% load static %}
    
    	<img src="{% static 'articles/sample.jpg' %}">
    ```

* 정적파일 사용하기 - 추가 경로

  * 정적 파일 위치 및 추가 경로 작성

    ```
    ex)
    -aticles
    -static
    	ㄴimages
    		ㄴsample.jpg
    ```

    ```
    STATICFILES_DIRS = [
    	BASE_DIR / 'static',
    ]
    ```

  * template에서 경로 참조

    ```
    # base.html
    
    {% load static %}
    
    <img src="{% static 'images/sample.jpg' %}">
    ```

    