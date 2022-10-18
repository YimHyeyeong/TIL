## Vuex Todo App

* Create Todo
  
  * State 작성
    
    * state에 2개의 todo 작성
    
    * [주의]
      
      * Vuex를 사용한다고 해서 Vuex Store에 모든 상태를 넣어야 하는 것은 아님
    
    ```javascript
    // index.js
    
    export default new Vuex.Store({
        state: {
            todos: [
                {
                    title:'1',
                    isCompleted:false,
                    date:new Date().getTime(),
                },
                {
                   title:'2',
                    isCompleted:false,
                    date:new Date().getTime(), 
                }        
            ]
        },
    ...
    })
    ```
  
  * TodoList 데이터 가져오기
    
    * 컴포넌트에서 Vuex Store의 state에 접근
      
      * `$store.state`
    
    ```html
    // TodoList.vue
    <template>
        <div>
        <todo-list-item
            v-for="todo in $store.state.todos"
            :key="todo.date"
        >
        </todo-list-item>
        </div>
    </template>
    ```
  
  * Computed로 변경
    
    * 현재 state의 todo는 값이 변화하는 것이 아님
    
    * store에 저장된 todo 목록을 가져오는 것이기 때문에 매번 새로 호출하는 것은 비효율적
    
    * 대신 todo가 추가되는 등의 변경 사항이 있을 때만 새로 계산한 값을 반환하는 방향으로 변경(computed)
    
    * this를 사용해(Vue Instance) 접근
    
    ```html
    // TodoList.vue
    <template>
        <div>
        <todo-list-item
            v-for="todo in todos"
            :key="todo.date"
        >
        </todo-list-item>
        </div>
    </template>
    
    <script>
    import TodoListItem from '@/components/TodoListItem'
    
    export default {
        ...
        computed:{
            todos:function () {
                return this.$store.state.todos
            }
        }
    }
    </script>
    ```
  
  * Pass Props (TodoList -> Todo)
    
    ```html
    // TodoList.vue
    <template>
        <div>
        <todo-list-item
            v-for="todo in todos"
            :key="todo.date"
            :todo="todo"
        >
        </div>
    </template>
    ```
    
    ```html
    // TodoListItem.vue
    
    <template>
        <div>
        {{todo.title}}
        </div>
    </template>
    
    <script>
    export default {
        name:'TodoListItem',
        props: {
            todo: {
                type:Object,
            }
        }
    }
    </script>
    ```
  
  * Actions & Mutations
    
    * createTodo 메서드를 통해 createTodo Acrion 함수 호출 (dispatch())
    
    ```html
    // TodoForm.vue
    
    <template>
        <div>
        <input
            type="text"
            v-model.trim="todoTitle",
            @keyup.enter="createTodo"
        >
        <button @click="createTodo">Add</button>
        </div>
    </template>
    
    <script>
    export default {
        ...,
        methods: {
            createTodo:function () {
                const todoItem = {
                    title: this.todoTitle,
                    isCompleted:false,
                    data: new Date().getTime(),
                }
                if (todoItem.title) {
                    this.$store.dispatch('createTodo', todoItem)
                }
                this.todoTitle = null
            }
        }
    }
    </script>
    ```
    
    * Actions
      
      * createTodo 함수
      
      * CREATE_TODO mutation 함수 호출
    
    * Mutations
      
      * CRREATE_TODO 함수
      
      * State의 todo 데이터 조작
    
    ```javascript
    // index.js
    
    export default new Vuex.Store({
        state: {
            todos:[],
        },
        mutations: {
            CREATE_TODO: function (state,todoItem) {
                state.todos.push(todoItem)
            }
        },
        actions: {
            CreateTodo:function (context, todoItem) {
                context.commit('CREATE_TODO', todoItem)
            }
        },
    ```
  
  * Actions의 "context" 객체
    
    * Vuex store의 전반적인 맥락 속성을 모두 포함하고 있음
    
    * 그래서 context.commit을 호출하여 mutation을 호출하거나 context.state와 context.getters를 통해 state와 getters에 접근할 수 있음
      
      * dispatch()로 다른 actions도 호출 가능
    
    * **"할 수 있지만 actions에서 state를 조작하지 말 것"**
    
    ```
    actions : {
        createTdo: function (context, todoItem) {
            console.log(context)
            console.log(context.state)
            console.log(context.getters)
            console.dispatch(...)
            context.commit('CREATE_TODO', todoItem)
        }
    },
    ```
  
  * Mutations handler name
    
    * Mutations 함수(핸들러 함수)의 이름은 사우로 작성하는 것을 권장
      
      * linter와 같은 tool에서 디버깅하기에 유용하며 전체 애플리케이션에서 어떤 것이 mutation인지 한눈에 파악할 수  있음
  
  * Javascript Destructuring assignment
    
    * 배열의 값이나 객체의 속성을 고유한 변수로 압출 해제(unpack)할 수 있는 JavaScript 표현식
    
    ```javascript
    const {state, commit} = context
    ```
    
    * actions 변경
    
    ```javascript
    // index.js
    actions: {
        createTodo:function (context, todoItem) {
            context.commit('CREATE_TODO', todoItem)
        }
    },
    ->
    actions : {
        createTodo: function ({commit}, todoItem) {
            commit('CREATE_TODO', todoItem)
        }
    },
    ```

* Delete Todo
  
  * TodoListItem 컴포넌트
    
    * deleteTodo action 함수 호출
    
    ```html
    // TodoListItem.vue
    <template>
        <div>
        <span>{{todo.title}}</span>
        <button @click="deleteTodo">Delete</button>
        </div>
    <template>
    <script>
    exportdefault {
        name:'TodoListItem',
        props: {
            todo: {
                type:Object,
            }
        },
        methods: {
            deleteTodo:function () {
                this.$store.dispatch('deleteTodo', this.todo)
            }
        }
    }
    </script>
    ```
  
  * Actions & Mutations
    
    ```javascript
    // index.js
    actions: {
        ...,
        deleteTodo:function({commit}, todoItem) {
            commit('DELETE_TODO', todoItem)
        }
    },
    ```
    
    ```javascript
    // index.js
    mutations: {
        ...,
        DELETE_TODO:function (state,todoItem) {
            // 1. todoItem이 첫 번째로 만나는 요소의 index를 가져옴
            const index = state.todos.indexOf(todoItem)
            //2. 해당 index 1개만 삭제하고 나머지 요소를 토대로 새로운 배열을 생성
            state.todos.splice(index,1)
        }
    }
    ```

* Update Todo
  
  * TodoListItem 컴포넌트
    
    * updateTodoStatus action 함수 호출
    
    ```html
    // TodoListItem.vue
    <template>
        <div>
        <span @click="updateTodoStatus">{{todo.title}}</span>
        <button @click="deleteTodo">Delete</button>
        </div>
    <template>
    <script>
    exportdefault {
        name:'TodoListItem',
        props: {
            todo: {
                type:Object,
            }
        },
        methods: {
            deleteTodo:function () {
                this.$store.dispatch('deleteTodo', this.todo)
            },
            updateTodoStatus: function() {
                this.$store.dispatch(''updateTodoStatus',this.todo)
            }
        }
    }
    </script>
    ```
  
  * Actions & Mutations
    
    ```javascript
    // index.js
    actions: {
        ,,,.
        updateTodoStatus: function ( {commit}, todoItem) {
            commit('UPDATE_TODO_STATUS', todoItem)
        }
    }
    ```
    
    ```javascript
    // index.js
    mutations: {
        UPDATE_TODO_STATUS:function (state,todoItem) {
            //4. 배열의 각 요소에 함수가 적용된 새로운 배열을 state.todos에 할당
            state.todos = state.todos.map(todo => {
            // 1. 선택된 todoItem과 현 todos의 요소가 todo가 서로 일치하면
            if (todo === todoItem) {
                //2. isCompleted의 값을 변경한 새로운 object return
                return {
                    title:todoItem.title,
                    date: new Date().getTime(),
                    isCompleted: !todo.isCompleted
                }
            } else {
            // 3. 일치하지 않으면 기존 배열 return
                return todo
                }    
            })
        }
    },
    ```
  
  * Javascript Spread Syntax
    
    * "전개 구문"
    
    * 배열이나 문자열과 같이 반복 가능한 (iterable) 문자를 요소 (배열 리터럴의 경우)로 화장하여 0개 이상의 `key-value`의 쌍으로 된 객체로 확장시킬 수 있음
    
    * '...'을 붙여서 요소 또는 키가 0개 이상의 iterable object를 하나의 object로 간단하게 표현하는 법
    
    * ECMAScript2015에서 추가 됨
    
    * Spread Syntax의 대상은 반드시 iterable 객체여야 함
    
    * 주 사용처
      
      1. 함수 호출
         
         - 배열의 목록을 함수의 인수로 활용 시
      
      2. 배열
         
         - 배열 연결
         
         - 배열 복사
      
      3. ***객체***
         
         - ***객체 복사***
    
    * 객체에서의 전개 구문
      
      * 객체 복사(shallow copy)
        
        ```javascript
        const obj1 = {foo:'bar', x:42}
        const obj2 = {foo:'baz', y: 13}
        
        const clonedObj = { ...obj1}
        console.log(clonedObj)
        // {foo:'bar', x: 42}
        
        const mergedObj = {...obj1, ...obj2}
        console.log(mergedObj)
        //{{foo:'baz', x:42,y:13}
        ```
        
        ```javascript
        const todoItem = {
            todo:'첫 번째 할 일',
            dueDate:'1999-12-12',
            importance:'high',
            isComleted:false
        }
        
        // isCompleted 값만 변경한다고 가정
        // 1
        const myUpdateTodo = {
            todo:'첫번째 할 일 ',
            duteDate:'1999-12-12',
            importance:'high',
            isCompleted:true
        }
        
        //2.
        const myUpdateTodo2 = {
            ...todoItem,
            isCompleted:true
        }
        ```
    
    * Muttions 변경
    
    * 변경 전
      
      * title:todoItem.title,
    
    * 변경 후
      
      * ...todo,
      
      ```javascript
      //index.js
      mutations: {
          UPDATE_TODO_STATUS:function(state,todoItem) {
              state.todos = state.todos.map(todo => {
              if (todo === todoItem) {
                  return {
                  ...todo,
                  isCompleted:!todo.isCompleted
                  }
              } else {
                  return todo
              }
          })
          }
      }
      ```
  
  * 취소선 긋기
    
    * v-bind를 사용한 class binding
      
      ```html
      // TodoListItem.vue
      <template>
          <div>
          <span 
              @click="updateTodoStatus"
              :class="{'is-completed': todo.isCompleted}"
          >{{todo.title}}</span>
          <button @click="deleteTodo">Delete</button>
          </div>
      <template>
      ...
      <style scoped>
      .is-completed {
          text-decoration:line-through;
      }
      </style>
      ```

* Getters
  
  * Getters 정의 및 활용
    
    * 완료된 todo 개수 계산
    
    ```javascript
    //index.js
    getters: {
        completedTodosCount:function (state) {
            return state.todos.filter(todo => {
                return todo.isCompleted === true    
            }).length
        }
    }
    ```
    
    * getters 사용
    
    * computed 반환 값으로 사용
    
    ```html
    //app.vue
    <template>
        <div id="app">
        <h1>TOdo List</h1>
        <h2>COmpleted Todo: {{completedTodosCount}}</h2>
        <todo-list></todo-list>
        <todo-form></todo-form>
        </div>
    <template>
    <script>
    ...
        computed: {
            completedTodosCount:function () {
                return this.$store.getters.completedTodosCount
            },
        }
    }
    </script>
    ```
    
    * 완료되지 않은 todo 개수 계산
    
    ```javascript
    // index.js
    getters: {
        completedTodosCount: function (...},
        uncompletedTodosCount:function(state) {
            return state.todos.filter(todo => {
                return todo.isCompleted ===false
            }).length
        }
    }
    ```
    
    * getters 사용
    
    * computed 반환 값으로 사용
    
    ```html
    // App.vue
    <template>
        <div id="app">
        <h1>TOdo List</h1>
        <h2>COmpleted Todo: {{completedTodosCount}}</h2>
        <h2>Uncompleted Todo:{{uncompletedTodosCount}}</h2>
        <todo-list></todo-list>
        <todo-form></todo-form>
        </div>
    <template>
    <script>
    ...
        computed: {
            uncompletedTodosCount:function () {
                return this.$store.getters.uncompletedTodosCount
            },
        }
    }
    </script>
    ```
    
    * 전체 todo 개수 계산
    
    ```javascript
    // index.js
    getters: {
        allTodosCount:function (state) {
            return state.todos.length
        }
    }
    ```
    
    * getters 사용
    
    * computed 반환 값으로 사용
    
    ```html
    // App.vue
    <template>
        <div id="app">
        <h1>All Todo:{{allTodosCount}} </h1>
        <h2>COmpleted Todo: {{completedTodosCount}}</h2>
        <h2>Uncompleted Todo:{{uncompletedTodosCount}}</h2>
        <todo-list></todo-list>
        <todo-form></todo-form>
        </div>
    <template>
    <script>
    ...
        computed: {
            allTodosCount:function () {
                return this.$store.getters.allTodosCount
            },
        }
    }
    </script>
    ```
