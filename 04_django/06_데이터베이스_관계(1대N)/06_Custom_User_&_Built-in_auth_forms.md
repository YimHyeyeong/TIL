## Custom User & Built-in auth forms

* Custom Built-in Auth Forms

  * 기존 User 모델을 사용하기 때문에 커스텀 User 모델로 다시 작성하거나 확장해야 햐는 forms

    * `UserCreationForm`
    * `UserChangeForm`

  * 이처럼 커스텀 User 모델이 AbstractUser의 하위 클래스인 경우 다음과 같은 방식으로 form을 확장

    ```
    from django.contrib.auth.forms import UserCreationForm
    form myapp.models import CustomUser
    
    class CustomUserCreationForm(UserCreationForm):
    	class Meta(UserCreationFOrm.Meta):
    		model  = CustiomUser
    		fields = UserCreationForm.Meta.fields + ('custom_field')
    ```

* UserCreationForm 확장

  ```
  # accounts/forms.py
  
  from djago.contrib.auth.forms import UserChangeForm, UserCreationForm
  
  class CustomUserCreationForm(UserCreationForm):
  	class Meta(UserCreationForm.Meta):
  		model = get_user_model()
  		fields = UserCreationForm.Meta.fields + ('email',)
  ```

* signup view 함수 코드 수정

  ```
  # accounts/views.py
  
  from .forms import CustomUserCreationForm
  
  def signup(request):
  	if request.method == 'POST':
  	 form = CustomUserCreationForm(request.POST)
  	 if form.is_valid():
  	 	user = form.save()
  	 	auth_login(request, user)
  	 	return redirect('articles:index')
  	 else:
  	 	form = CustomUserCreationForm()
  	 context = {
  	 	'form':form,
  	 }
  	 return render(request,'accounts/signup.html', context)
  ```

* get_user_model()

  * 현재 프로젝트에서 활성화된 사용자 모델(active user model)을 반환
    * User 모델을 커스터마이징한 상황에서는 **Custom User** 모델을 반환
  * 이 때문에 django는 User 클래스를 직접 참조하는 대신 `django.contrib.auth.get_user_model()`을 사용하여 참조해야 한다고 강조