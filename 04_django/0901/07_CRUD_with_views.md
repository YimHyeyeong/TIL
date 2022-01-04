## CRUD with views

+ 게시글 정렬 순서 변경

  1. DB로부터받은 쿼리셋을 이후에 파이썬이 변경(Python)

     ```
     def index(request):
     	articles = Article.objects.all()[::-1]
     ```

  2. 처음부터 내림차순 쿼리셋으로 받음(DB가 조작)

     ```
     articles = Article.objects.order_by('-pk')
     ```

+ HTTP method

  + GET

    + 특정 리소스를 가져오도록 요청할 때 사용
    + 반드시 데이터를 가져올 때만 사용해야 함
    + DB에 변화를 주지 않음
    + CRUD에서 `R` 역할을 담당

    > a 태그 link도 get으로 처리
    >
    > redirect()도 get

  + POST

    + 서버로 데이터를 전송할 때 사용
    + 리소스를 생성/변경하기 위해 데이터를 `HTTP body` 에 담아 전송
    + 서버에 변경사항을 만듦
    + CRUD에서 `C`/`U`/`D` 역할을 담당

+ Django shortcut function - "redirect()"

  + 새 URL로 되돌림

  + 인자에 따라 `HttpResponseRedirect`를 반환

  + 브라우저는 현재 경로에 따라 전체 URL 자체를 재구성(reconstruct)

  + 사용 가능한 인자

    1. model
    2. view name : viewname can be **URL pattern name** or callable **view object.**
    3. absolute or relative URL

  + ex

    ```
    # views.py
    
    from django.shortcuts import render, redirect
    
    def create(reqeust):
    	...
    	return redirect('articles:index')
    ```

+ DETAIL

  + 개별 게시글 상세 페이지

  + 글의 번호(pk)를 활용해서 각각의 페이지를 따로 구현해야 함

    ```
    # url.py
    path('<int:pk',views.detail, name='detail')
    ```

    ```
    # views.py
    
    def detail(request, pk):
    	article = Article.objects.get(pk=pk)
    	...
    ```

  + 오른쪽 pk는 `variable routing` 을 통해 받은 pk

  + 왼쪽 pk 는 DB 에 저장된 레코드의 pk (id)