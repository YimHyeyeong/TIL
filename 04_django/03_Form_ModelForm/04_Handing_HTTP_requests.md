## Handing HTTP requests

* Django에서 HTTP 요청을 처리하는 방법
  1. Django shortcut functions
  2. View decorators

1. Django shortcut functions

   * django.shortcut 패키지는 개발에 도움 될 수 있는 여러 함수와 클래스를 제공
   * shortcuts functions 종류
     * `render()`
     * `redirect()`
     * `get_object_or_404()`
     * `get_list_or_404()`

   * get_object_or_404()
     * 모델 manager objects에서 get()을 호출하지만 해당 객체가 없을 경우 `DoesNotExist` 예외 대신 **Http 404**를 raise
     * get() 메서드에 경우 조건에 맞는 데이터가 없을 경우에 에러를 발생 시킴
       * 코드 실행 단계에서 발생한 에러에 대해서 브라우저는 `http status code 500`으로 인식함
     * 상황에 따라 적절한 예외처리를 하고 클라이언트에게 올바른 에러를 전달하는 것 또한 개발의 중요한 요소 중 하나

2. Django View decorators

   * Django는 다양한 HTTP 기능을 지원하기 위해 뷰에 적용할 수 있는 여러 데코레이터를 제공
   * [참고] Decorator(데코레이터)
     * 어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능을 연장 해주는 함수
     * django는 다양한 기능을 지원하기 위해 `view`함수에 적용할 수 있는 여러 데코레이터를 제공
   * Allowed HTTP methods
     * 요청 메서드에 따라 view 함수에 대한 엑세스를 제한
     * 요청이 조건을 충족시키지 못하면 `HttpResponseNotAllowed`을 return (405 Method Not Allowed)
     * require_http_methods(), require_POST(), require_safe(), require_GET()
   * require_http_methods()
     * view 함수가 특정한 method 요청에 대해서만 허용하는 데코레이터
   * require_POST()
     * view함수가 POST method 요청만 승인하도록 하는 데코레이터

   ```
   # views.py
   
   form django.views.decorators.http import require_http_methods, require_POST
   
   @require_http_methods(['GET','POST'])
   def create(request)
   	pass
   	
   @require_POST
   def create(request)
   	pass
   ```

   