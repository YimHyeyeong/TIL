## Profile Page

- Profile Page 작성
  
  - url 작성
    
    ```
    # urls.py
    urlpatterns = [
        path('<username>/', views.profile, name='profile'),
    ]
    ```
  
  - profile view 함수
    
    ```
    # views.py
    def profile(request,username):
        person = get_object_or_404(get_user_model(),username=username)
        context = {
            'person':person
        }
        return render(request, 'accounts/profile.html, context)
    ```





## Follow

* Follow 구현
  
  * ManyToManyField 작성
    
    ```
    class User(AbstractUser):
        followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
    ```
  
  * url 작성
    
    ```
    urlpatterns = [
        path('<int:user_pk>/follow/', views.follow, name='follow')
    ]
    ```
  
  * follow view 함수
    
    ```
    def follow(request, user_pk):
        if request.user.is_authenticated:
            person = get_object_or_404(get_user_model(), pk=user_pk)
            if person != request.user:
                if person.followers.filter(pk=request.user.pk).exists():
                    person.follower.remove(request.user)
                else:
                    person.followers.add(request.user)
            return redirect('accounts:profile', person.username)
        return redirect('accounts:login')
    ```
    
    
