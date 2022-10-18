## 1:N Relation

* DRF with 1:N Ralation
  
  ```
  class Comment(models.Model):
      article = models.ForeignKey(Article, on_delete=models.CASCADE)
      content = models.TextField()
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  ```
1. GET - Comment LIst
   
   ```
   class CommentSerilizer(serializers.ModelSerializer):
       class Meta:
           model = Comment
           fields = '__all__'
   ```
   
   ```
   urlpatterns = [
       path('comments/', views.comment_list)
   ]
   ```
   
   ```
   @api_view(['GET'])
   def comment_list(request):
       comments = get_list_or_404(Comment)
       serializer = CommentSerializer(comments, many=True)
       return Response(serializer.data)
   ```

2. GET - Comment Detail
   
   ```
   @api_view(['GET'])
   def comment_detail(request, comment_pk):
       comment = get_object_or_404(Comment, pk=comment_pk)
       serializer = CommentSerializer(comment)
       return Response(serializer.data)
   ```

3. POST - Create Comment
   
   ```
   @api_view(['POST'])
   def comment_create(request,article_pk):
       article = get_object_or_404(Article, pk=article_pk)
       serializer = CommentSerializer(data=reqeust.data)
       if serializer.is_valid(raise_exception=True)
           serializer.save()
           return Response(serializer.data, status=status.HTTP.201_CREATED)
   ```
   
   - Article 생성과 달리 Comment 생성은 생성 시에 참조하는 모델의 객체 정보가 필요
     
     - 1:N 관계에서 N은 어떤 1을 참조하는지에 대한 정보가 필요하기 때문 (외래 키)
* Passing Additional attributes to **.save()**  
  
  * `.save() `메서드는 특정 Serializer 인스턴스를 저장하는 과정에서 추가적인 데이터를 받을 수 있음
    
    * 인스턴스를 저장하는 시점에 추가 데이터 삽입이 필요한 경우
    
    ```
    @api_view(['POST'])
    def comment_create(request, article_pk):
        article = get_object_or_404(Article, pk=article_pk)
        serializer = CommentSerializer(data=reqeust.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save(article=article)
            return Response(serializer.data, status=status.HTTP_201_CREATED)
    ```

* Read Only Field(읽기 전용 필드)
  
  * 어떤 게시글에 작성하는 댓글인지에 대한 정보를 `form-data`로 넘겨주지 않았기 때문에 직렬화하는 과정에서 article 필드가 유효성 검사(is_valid)를 통과하지 못함
    
    * CommentSerializer에서 `article field`에 해당하는 데이터 또한 요청으로부터 받아서 직렬화하는 것으로 설정되었기 때문
  
  * 이때는 읽기 전용 필드(read_only_fields) 설정을 통해 직렬화하지 않고 반환 값에만 해당 필드가 포함되도록 설정할 수 있음
  
  ```
  class CommentSerializer(serializers.ModelSerializer):
      class Meta:
          model = Comment
          fields = '__all__'
          read_only_fields = ('article',)
  ```
4. DELETE & PUT - delete, update Comment
   
   * Article 생성 로직에서와 마찬가지로 `comment_detail` 함수가 모두 처리할 수잇도록 작성
   
   ```
   @api_view(['GET', 'DELETE', 'PUT'])
   def comment_detail(request, comment_pk):
       comment = get_object_or_404(Comment, pk=comment_pk)
       serializer = CommentSerializer(comment)
       return Response(serializer.data)
       
       elif request.method == 'DELETE':
           comment.delete()
           data = {
               'delete':'댓글이 삭제되었습니다.'
           }
           return Response(data, status,status.HTTP_204_NO_CONTENT)
       
       elif request.method == 'PUT':
           serializer = CommentSerializer(comment, data=request.data)
           if serializer.is_valid(raise_exception=True)
           return Response(serializer.data)
   ```
* 1:N Serializer
  
  1. 특정 게시글에 작성된 댓글 목록 출력하기
     
     * 기존 필드 override
  
  2. 특정 게시글에 작성된 댓글의 개수 구하기
     
     * 새로운 필드 추가
1. 특정 게시글에 작성된 댓글 목록 출력하기
   
   * Serializer는 기존 필드를 override하거나 추가 필드를 구성할 수 있음
   
   * 우리가 작성한 로직에서는 크게 2가지 형태로 구성할 수 잇음
     
     1. PrimaryKeyRelatedFIeld
     
     2. Nested relationships
   
   * case 1) PrimaryKeyRelatedField
     
     * pk를 사용하여 관계된 대상을 나타내는 데 사용할 수 있음
     
     * 필드가 `to many relationships(N)`를 나타내는데 사용되는 경우 **many=True** 속성 필요
     
     * comment_set 필드 값을 `form-data`로 받지 않으므로 **read_only=True** 설정 필요
     
     ```
     class ArticleSerializer(serializers.ModelSerializer):
         comment_set = serializer.PrimaryKeyRelatedField(many=True,read_only=True)
         class Meta():
             model = Article
             fields= '__all__'
     ```
     
     * 역참조 시 생성되는 `comment_set`을 **override** 할 수있음
     
     ```
     class Comment(models.Model):
         article = models.ForeignKey(Article,on_delete=models.CASCASE, related_name='comments')
     ```
   
   * case 2) Nested relationships
     
     * 모델 관계상으로 참조된 대상은 참도하는 대상의 표현(응답)에 포함되거나 중첩(nested)될 수 있음
     
     * 이러한 중첩된 관계는 `serializers`를 필드로 사용하여 표현할 수 있음
     
     * 두 클래스의 상하위치 변경
     
     ```
     class CommentSerializer(serializers.ModelSerializer):
         class Meta:
             model = Comment
             fields = '__all__'
             read_only_fields = ('article',)
     
     
     class ArticleSerializer(serialiers.ModelSerializer):
         comment_set = CommentSerializer(many=True,read_only=True)
         class Meta:
             model = Article
             fields = '__all__'
     ```

2. 특정 게시글에 작성된 댓글의 개수 구하기
   
   * `comment_set` 매니저는 모델 관계로 인해 자동으로 구성되기 때문에 커스텀 필드를 구성하지 않아도 `comment_set`이라는 필드명을 fields`옵션에 작성만 해도 사용 할 수 있엇음
   
   * 하지만 지금처럼 별도의 값을 위한 필드를 사용하려는 경우 자동으로 구성되는 매니저가 아니기 때문에 직접 필드를 작성해야 함
   
   ```
   class ArticleSerializer(serializers.ModelSerializer):
       comment_set = CommentSerializer(many=True,read_only=True)
       commet_count = serializers.IntegerField(source='comment_set.count', read_only=True)
       class Meta:
           model = Article
           fields = '__all__'
   ```
   
   * '**source' arguments**   
     
     * 필드를 채우는 데 사용할 속성의 이름
     
     * 점 표기법 (dot notation)을 사용하여 속성을 탐색 할 수 있음
     
     * `comment_set`이라는 필드에 .(dot)을 통해 전체 댓글의 개수 확인 가능
     
     * `.count()`는 **built-in Queryset API** 중 하나
     
     ```
     class ArticleSerializer(serializers.ModelSerializer):
         comment_set = CommentSerializer(many=True, read_only=True)
         comment_count = serialilzers.IntegerField(source='comment_set.count', read_only=True)
         class Meta:
             model = Article
             fields = '__all__'
     ```
* [주의 사항] 'read_only_fields' shoutcut issue
  
  * 특정 필드를 override 혹은 추가한 경우 read_only_fields shotycut 으로 사용할 수 없음
  
  ```
  class ArticleSerializer(serializers.ModelSerializer):
      comment_set = CommentSerializer(many=True, read_only=True)
      comment_count = serialilzers.IntegerField(source='comment_set.count', read_only=True)
      class Meta:
          model = Article
          fields = '__all__'
          read_only_fields = ('comment_set', 'comment_count',) #사용불가
  ```
  
  
