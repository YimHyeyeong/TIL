## Single Model

* DRF with Single Model
  
  * 단일 모델의 data를 직렬화(serializaion)하여 JSON으로 변환하는 방법에 대한 학습
  
  * 단일 모델을 두고 CRUD 로직을 수행 가능하도록 설계
  
  * API 개발을 위한 핵심 기능을 제공하는 도구 활용
    
    * DRF built-in form
    
    * Postman

* [참고] Postman
  
  * API를 구축하고 사용하기 위해 여러 도구를 제공하는 API 플랫폼
  
  * 설계, 테스트,문서화 등의 도구를 제공함으로써 API를 더 빠르게 개발 및 생성할 수 있도록 도움

* ModelSerializer
  
  * 모델 필드에 해당하는 필드가 있는 `Serializer` 클래스를 자동으로 만들 수 있는 shortcut
  
  * 아래 핵심 기능을 제공
    
    1. 모델 정보에 맞춰 자동으로 필드 생성
    
    2. `serializer`에 대한 유효성 검사기를 자동으로 생성
    
    3. `.create()` & `.update()` 의 간단한 기본 구현이 포함됨
  
  * Model의 필드를 어떻게 **'직렬화'** 할 지 설정하는 것이 핵심
  
  * 이 과정은 Django에서 Model의 필드를 설정하는 것과 동일함
    
    ```
    # serializers.py
    class ArticleListSerizlizer(serializers.ModelSerializer):
        class Meta:
            model = Article
            fields = ('id', 'title',)
    ```

* 'many' argument
  
  * many=True
    
    * **"Serializing multiple objects"**
    
    * 단일 인스턴스 대신 QuerySet 등을 직렬화하기 위해서는 serializer를 인스턴스화 할 때 many=True를 키워드 인자로 전달해야 함
1. GET - Article List
   
   * url 및 view 함수 작성
   
   ```
   urlpatterns = [
       path('articles/', views.article_list)
   ]
   ```
   
   ```
   @api_view(['GET'])
   def article_list(request):
       articles = get_list_or_404(Article)
       serializer = ArticleListSerializer(articles, many=True)
       return Response(serializer.data)
   ```
   
   * **api_view** decorator
     
     * 기본적으로 `GET` 메서드만 허용되면 다른 메서드 요청에 대해서는 **405 Method Not Allowed**로 응답
     
     * View 함수가 응답해야 하는 HTTP 메서드의 목록을 리스트의 인자로 받음
     
     * DRF에서는 선택이 아닌 **필수적으로 작성**해야 해당 view 함수가 정상적으로 동작함

2. GET - Article Detail
   
   * Article List와 Article Detail을 구분하기 위해 추가 Serializer 정의
   
   * 모든 필드를 직렬화하기 위해 fields 옵션을 `'__all__'`로 설정
   
   ```
   class ArticleSerializer(serializers.ModelSerializer):
       class Meta:
           model = Article
           fields = '__all__'
   ```
   
   * url 및 view 함수 작성
   
   ```
   urlpatterns = [
       path('articles/<int:article_pk/', views.article_detail)
   ]
   ```
   
   ```
   @api_view(['GET'])
   def article_detail(request, article_pk):
       article_get_object_or_404(Article, pk=article_pk)
       serializer = ArticleSerializer(article)
       return Response(serializer.data)
   ```

3. POST - Create Article
   
   * 201 Created 상태 코드 및 메시지 응답
   
   * RESTful 구조에 맞게 작성
     
     1. URI는 자원을 표현
     
     2. 자원을 조작하는 행위는 HTTP Method
   
   * article_list 함수로 게시글을 조회하거나 생성하는 행위를 모두 처리 가능
   
   * view 함수 수정
   
   ```
   @api_view(['GET', 'POST'])
   def article_list(request):
       if request.method == 'GET':
           articles = get_list_or_404(Article)
           serializer = ArticleListSerializer(articles, many=True)
           return Response(serializer.data)
       elif request.method == 'POST':
           serializer = ArticleSerializer(data=reqeust.data)
           if serializer.is_valid():
               serializer.save()
               return Response(serialzier.data, status=status.HTTP_201_CREATED)
           return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
   ```
* Status Codes in DRF
  
  * DRF에는 status code를 보다 명확하고 읽기 쉽게 만드는 데 사용할 수 있는 정의된 상수 집합을 제공
  
  * **status** 모듈에 HTTP status code  집합이 모두 포함되어 있음
  
  * 단순히 `status=201`같은 표현으로도 사용할 수 있지만 DRF는 권장하지 않음

* **'raise_exception'** argument
  
  * "Raising an exception on invalid data"
  
  * `is_valid()`는 유효성 검사 오류가 있는 경우 `serializers.ValidationError` 예외를 발생시키는 선택적 raise_exception 인자를 사용할 수 있음
  
  * DRF에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며 기본적으로 **HTTP status code 400**을 응답으로 반환함
  
  ```
  @api_view(['GET', 'POST'])
  def article_list(request):
      if request.method == 'GET':
          articles = get_list_or_404(Article)
          serializer = ArticleListSerializer(articles, many=True)
          return Response(serializer.data)
      elif request.method == 'POST':
          serializer = ArticleSerializer(data=reqeust.data)
          if serializer.is_valid(raise_exception=True):
              serializer.save()
              return Response(serialzier.data, status=status.HTTP_201_CREATED)
          # 400statuscode를 응답하는 return 구문 삭제
  ```
4. DELETE - Delete Article
   
   * 204 No Content 상태 코드 및 메시지 응답
   
   * article_detail 함수로 상세 게시글을 조회하가너 삭제하는 행위 모두 처리 가능
   
   ```
   @api_view(['GET', 'DELETE'])
   def article_detail(reqeust, article_pk):
       article = get_object_or_404(Article, pk=article_pk)
       if request.method == 'GET':
           serializer = ArticleSerializer(article)
           return Response(serializer.data)
       elif request.method == 'DELETE':
           article.delete()
           data = {
               'delete': f`데이터 {article_pk}번이 삭제되었습니        
           }
           return Response(data, status.status.HTTP_204_NO_CONTENT)
   ```

5. PUT - Updata Article
   
   * article_detail 함수로  상세 게시글을 조회하거나 삭제, 수정하는 행위 모두 처리 가능
   
   ```
   @api_view(['GET', 'POST', 'PUT'])
   def article_detail(request, article_pk):
       ...
       elif reqeust.method == 'PUT':
           serializer = ArticleSerializer(article, data=reqeust.data)
           if serializer.is_valid(raise_exception=True):
               serializer.save()
               return Response(serializer.data)
   ```
   
   
