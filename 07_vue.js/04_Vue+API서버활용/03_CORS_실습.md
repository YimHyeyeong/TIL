## CORS 실습

* Server 살펴보기

  * Todo Model

    ```django
    # todos/models.py
    from django.db import model
    
    class Todo(models.Model):
    	title = models.CharField(max_length=50)
    	is_completed = models.BooleanField(defult=False)
    
    	def __str__(self):
    		return self.title
    ```

  * Serializer

    ```
    # todos/serializer.py
    
    from rest_framework import serializers
    from .models import Todo
    
    class TodoSerializer(serializers.ModelSerializer):
    	class Meta:
    		model = Todo
    		fields = ('id', 'title', 'is_completed',)
    ```

  * Todo 생성 및 조회 Urls

    ```
    # mypjt/urls.py
    
    urlpatterns = [
    	path('admin/', admin.site.urls),
    	path('todos/', include('todos.urls')),
    	path('accounts/', include('accounts.urls'))
    ]
    ```

    ```
    # todos/urls.py
    
    urlpatterns = [
    	path('', views.todo_list_create),
    	path('<int:todo_pk>/', views.todo_update_delete),
    ]
    ```

  * Todo 생성 및 조회 views

    ```
    # todos/views.py
    
    @api_view(['GET', 'POST'])
    def todo_list_create(request):
    	if request.method == 'GET':
    		todos = Todo.objects.all()
    		serializer = TodoSerializer(todos, many=True)
    		return Response(serilizer.data)
    	elif request.method == 'POST':
    		serializer = TodoSerializer(data=request.data)
    		if serializer.is_valid(raise_exception=True):
    			serializer.save()
    			return Response(serializer.data, status=status.HTTP_201_created)
    ```

  * Todo 수정 및 삭제 urls

    ```
    # todos/urls.py
    urlpatterns = [
    	path('', views.todo_list_create),
    	path('<int:todo_pk>/', views.todo_update_delete)
    ]
    ```

  * Todo 수정 및 삭제 views

    ```
    # todos/views.py
    @api_view(['PUT', 'DELETE'])
    def todo_update_delete(request, todo_pk):
    	todo = get_object_or_404(Todo, pk=todo_pk)
    	if request.method = 'PUT':
    		serializer = TodoSerializer(todo, data=request.data)
    		if serializer.is_valid(raise_exception=True):
    			serializer.saave()
    			return Response(serializer.data)
    		elif request.method == 'DELETE':
    			todo.delete()
    			return Response({'id':todo_pk}, status=statue.HTTP_204_NO_CONTENT)
    ```

* Client 살펴보기

  * Router 설정확인

    ```
    // router/index.js
    import TodoList from '@/views/todos/TodoList'
    
    Vue.use(VueRouter)
    const routes = [
    	{
    		path:'/todos',
    		name:'TodoList',
    		component: TOdoList
    	}
    ]
    ```

    ```
    // App.vue
    <template>
    	<div id="app">
    		<div id="nav">
    			<router-link :to="{name:'TodoList'}">Todo List</router-link>
    			...
    		</div>
    		<router-view/>
    	</div>
    </template>
    ```

* CORS 이슈

  * TodoList 컴포넌트에서 GET Todos 버튼 클릭 후 브라우저 콘솔 확인
  * Todo를 가져오는 과정에서 발생하는 CORS 관련 에러 메시지
  * 반면에 Server의 로그를 확인해보면 정상적으로 200으로 응답하고 있음
  * 요청한 리소스에 `Access-Control-Allow-Origin` 헤더가 없기 때문에 CORS 정책에 의해 차단된 것
    * 기본적으로 `SOP(Same-Origin Policy)`를 준수하기 때문

* CORS 설정

  ```
  $ pip install django-cors-headers
  ```

  ```
  # settings.py
  INSTALLED_APPS = [
  	...
  	'corsheaders'
  ]
  MIDDLEWARE = [
  	'corsheaders.middleware.CorsMiddleware'
  ]
  ```

  ```
  # 1. 특정 Origin만 선택적으로 허용
  CORS_ALLOWED_ORIGINS = [
  	'https://example.com'
  ]
  
  # 2. 모든 Origin 허용
  CORS_ALLOW_ALL_ORIGINS = True
  ```

* Life Cycle Hook - 'Created'

  * 버튼을 눌러서 Todo를 가져오는게 아닌 해당 페이지에 들어오는 시점에 자동으로 todo 목록을 조회해서 출력할 수 있도록 변경하기
  * Life Cycle Hook의 Created Hook 활용하여 특정 시점에 어떠한 행위를 자동화 할 수 있음
    * Vue Instance가 created 되는 시점에 getTodos 함수를 호출하도록 작성

  ```
  // TodoList.vue
  <script>
  	...
  	methods: {
  		getTodos: function() {
  			...
  		}
  	},
  	created: function () {
  		thid.getTodos()
  	}
  }
  </script>
  ```

* Todo Create

  * Router 설정 확인

    ```
    // index.js
    import CreateTodo from '@/views/todos/CreateTodo'
    Vue.use(VueRouter)
    const routes = [
    	...
    	{
    		path:'/todos/create',
    		name:'CreateTodo',
    		component: CreateTodo
    	}
    ]
    ```

    ```
    // App.vue
    <template>
    	<div id="app">
    		<div id="nav">
    			<span>
    				...
    				<router-link :to="{name:'CreateTodo'}">Create Todo</router-link>
    			</span>
    			...
    		</div>
    		<router-view/>
    	</div>
    </template>
    ```

  * enter를 누르거나 버튼을 Click하는 경우 모두 todo를 생성할 수 있도록 구성하기

  * v-model을 활용해 data의 title과 양방향 바인딩

  * requestBody에 todo를 담아 `Server`에 전송

  * todos 배열에 새로운 todo 할당

  * todo가 잘 생성되지만 TOdoList 컴포넌트로 다시 이동해서 조회해야 생성 여부를 확인할 수 있음

    ```
    // CreateTodo.vue
    <script>
    export default {
    	name: 'CreateTOdo',
    	data: function () {
    		return {
    			title:null
    		}
    	},
    	methods: {
    		createTodo:function () {
    			const todoItem = {
    				title: this.title,
    			}
    			if (todoItem.title) {
    				axios({
    					method:'post',
    					url:'',
    					data:todoItem
    				})
    					.then(res => {
    						console.log(res)
    					})
    					.catch(err => {
    						console.log(err)
    					})
    			}
    		}
    	}
    }
    </script>
    ```

  * todo를 작성하고 TodoList 컴포넌트에서 이를 바로 확인할 수 있도록 router 구성

    ```
    // CreateTodo.vue
    
    methods: {
    	createTodo: function() {
    		const todoItem = {
    			title:this.title
    		}
    		if (todoItem.title) {
    			axios({
    				method:'post',
    				url:'',
    				data:todoItem
    			})
    				.then((res) => {
    					console.log(res)
    					this.$router.push({name:'TodoList'})
    				})
    				.catch(err => {
    						console.log(err)
    					})
    		}
    	}
    }
    ```

  * deleteTodo 메서드 확인

    ```
    // TodoList.vue
    methods: {
    	getTodos: function () {
    		...
    	},
    	deleteTodo: function (todo) {
    		axios({
    			method:'delete',
    			url:''
    		})
    			.then(res => {
                console.log(res)
                })
                .catch(err => {
                console.log(err)
                })
    	}
    }
    ```

  * 각 Todo의 삭제버튼확인

    ```
    // TodoList.vue
    <template>
    	<div>
    		<ul>
    			<li v-for="todo in todos" :key="todo.id">
    				<span @click="updateTodoStatus(todo)" :class="{
    				'is-complated': todo.is_completed
    				}">{{todo.title}}</span>
    			</li>
    		</ul>
    	</div>
    </template>
    ```

  * Todo 삭제 후 DOM이 실시간으로 반영되지 않으므로 삭제 후 getTodo 메서드를 호출

    ```
    // TodoList.vue
    deleteTodo: function(todo) {
    	axios({
    		method:'delete',
    		url:''
    	})
    		.then(res => {
    			console.log(res)
    			this.getTodos()
    		})
    		.catch(err => {
    			console.log(err)
    		})
    }
    ```

  * 요청에 담아 보내는 todo는 complted 필드를 제외하고는 모두 동일하기 때문에 Sprea Operator를 활용해서 표현

    ```
    // TodoList.vue
    updateTodoStatus: function(todo) {
    	const todoItem = {
    		...todo,
    		is_complated: !todo.is_completed
    	}
    	axios({
    		method:'put',
    		url:''
    		data:todoItem
    	})
    		.then(res => {
    			console.log(res)
    		})
    }
    ```

  * updateTodosStatus 메서드를 통해 특정 todo의 상태를 토글

  * todo 요소를 클릭했을 때 CSS 활용해 취소선 반영

    ```
    // TodoList.vue
    <template>
    	<div>
    		<ul>
    			<li v-for="todo in todos" :key="todo.id">
    				<span @click="updateTodoStatus(todo)" :class="{
    				'is-complated': todo.is_completed
    				}">{{todo.title}}</span>
    				<button @click="deleteTodo(todo)" class="todo-btn">X<button>
    			</li>
    		</ul>
    	</div>
    </template>
    ```

  * Delete 로직과 마찬가지로 Server로 간 요청은 업데이트된 todo의 상태를 잘 반영 DB에 잘 반영하지만 수정된 결과 화면의 표현이 업데이트 되지 않음을 해결

    ```
    // TodoList.vue
    updateTodoStatus: function(todo) {
    	const todoItem = {
    		...todo,
    		is_complated: !todo.is_completed
    	}
    	axios({
    		method:'put',
    		url:''
    		data:todoItem
    	})
    		.then(res => {
    			console.log(res)
    			todo.is_completed = !todo.is_completed
    		})
    }
    ```

  * SPA의 특징 복습

    * 각 라우터를 클릭해도 개발자도구 - Network 탭에는 아무런 변화가 없음
    * 최초에 서버로부터 받은 빈 HTML 문서 안에서 DOM을 만들어 가는 것이 핵심