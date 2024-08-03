# CRUD with Authentication

* JWT 인증 문제

  * 현재 TodoList 와 CreateTodo 컴포넌트에서 모두 401 에러가 발생
  * 요청 시 토큰이 없는(==인증되지 않은) 요청은 IsAuthenticated의 유효성 검사를 통과하지 못하기 때문
  * 인증이 필요한 요청에 브라우저에 저장된 JWT를 함께 보내야 함

* JWT 요청을 보내는 방법

  * JWt는 HTTP request header에 `"Authorization: JWT <my_token>"` 형식으로 보내야 함

* JWT 설정 및 적용

  * 토큰만 세팅해주는 `setToken` 메서드 작성

    ```
    // TodoList.vue
    
    methods: {
     setToken: function () {
     	// 1. LocalStorage에서 jwt 토큰을 가져옴
     	const token = localStorage.getItem('jwt')
     	// 2. header에 token을 넣기 위한 준비
     	const config = {
     		Authorization: `JWT ${token}`
     	}
     	// 3. 응답
     	return config
     }
    }
    ```

  * getTodos, deleteTodos, updateTodoState 메서드에 모두 setToken 메서드를 호출하여 요청 시 JWT를 함께 보낼 수 있도록 수정

    ```
    // TodoList.vue
    
    getTodos: function () {
    	axios({
    		method:'get',
    		url:''
    		headers: this.setToken()
    	})
    	.then(res => {
    		console.log(res)
    		this.todos = res.data
    	})
    	.catch(err => {
    		console.log(err)
    	})
    },
    deleteTodo: function (todo) {
    	axios({
    		method: 'delete',
    		url:'',
    		headers: this.setToken()
    	})
    	.then(res => {
    		console.log(res)
    		this.getTodos()
    	})
    	.catch(err => {
    		console.log(err)
    	})
    },
    updateTodoStatus: function (todo) {
    	const todoItem = {
    		...todo,
    		is_completed: !todo.is_completed
    	}
    	axios({
    		method:'put',
    		url:'',
    		data: todoItem,
    		headers: this.setToken(),
    	})
    	.then(res => {
    		console.log(res)
    		todo.is_completed = !todo.is_completed
    	})
    }
    ```

  * CreateTodo 컴포넌트의 createTodo 메서드에서도 setToken 메서드를 호출하여 요청 시 JWT를 함께 보낼 수 있도록 수정

    ```
    // CreateTodo.vue
    
    methods: {
    	setToken:function (0 {
    		const token = localStorage.getItem('jwt')
    		const config = {
    			Authorization: `JWT ${token}`
    		}
    		return config
    	}),
    	createTodo: function () {
    		const todoItem = {
    			title: this.title
    		}
    		if (todoItem.title) {
    			axios({
    				method:'post',
    				url:'',
    				data: todoItem,
    				headers:this.setToken(),
    			})
    			.then(res => {
    				console.log(res)
    				this.$router.push({name:'TodoList'})
    			})
    			.catch(err => {
    				console.log(err)
    			})
    			this.title = null
    		}
    	}
    }
    ```

* Todo 저장 문제

  * TodoList 컴포넌트는 접속이 되지만 (200) 게시글 작성 시 다음과 같은 에러 발생
  * User와 Todo를 1:N으로  설정하는데 Todo가 작성될 때 User 정보가 없어 DB에 저장하려고 하는 시점에 Validation을 통과하지 못한 것

* Server - view 함수 수정

  * 현재 로그인한 유저가 작성한 todo만 보여줄 수 있도록 함

    ```
    # todos/views.py
    @api_view(['GET','POST'])
    def todo_list_create(request):
    	if reqeust.method == 'GET':
    		# todos = Todo.objects.all()
    		todos = request.user.todo_set.all()
    		serializer = TodoSerializer(todos, many=True)
    		return Response(serializer.data)
    	elif request.method == 'POST':
    		serializer = TodoSerializer(data=request.data)
    		if serializer.is_valid(raise_exception=True):
    			serializer.save(user=request.user)
    			return Response(serializer.data, status=status.HTTP_201_CREATED)
    ```

  * 현재 로그인한 유저가 작성한 todo만 보여줄 수 있도록 함

    ```
    # todos/views.py
    
    @api_view(['PUT', 'DELETE'])
    def todo_update_delete(request, todo_pk):
    	todo = get_object_or_404(Todo, pk=todo_pk)
    	# 해당 todo의 작성자가 아닌 경우 todo를 수정하거나 삭제하지 못하게 설정
    	if not request.user.todo_set.filter(pk=todo_pk).exists():
    		return Response({'detail': '권한이 없습니다'}, status=status.HTTP_403_FORBIDDEN)
    	if request.method == 'PUT':
    		serializer = TodoSerializer(todo, data=request.data)
    		if serializer.is_valid(raise_exception=True):
    			serializer.save()
    			return Response(serializer.data)
    	elif request.method == 'DELETE':
    		todo.delete()
    		return Response({'id':todo_pk}, status=status.HTTP_204_NO_CONTENT)
    ```

## 추가 구현

1. 회원가입 후 자동 로그인 설정

   * 회원가입 요청이 이행되었을 때 login 요청이 진행 될 수 있도록 작성

     ```
     // Signup.vue
     methods: {
     	signup: function() {
     		axios({
     			method: 'post',
     			url:'',
     			data:this.credentials
     		})
     		.then(() => {
     			axios({
     				method:'post',
     				url:'',
     				data:this.credentials
     			})
     			.then(res => {
     				localStorage.setItem('jwt',res.data.token)
     				this.$emit('login')
     				this.$router.push({name:'TodoList'})
     			})
     			.catch(err => {
     				console.log(err)
     			})
     		})
     		.catch(err => {
     			console.log(err)
     		})
     	}
     }
     ```

2. 환경변수 적용

   ```
   # .env.local
   VUE_APP_SERVER_URL = http://127~
   ```

   ```
   // 해당되는 곳 모두 변경
   const SERVER_URL = process.env.VUE_APP_SERVER_URL
   `${SERVER_URL}/todos/`
   ```
