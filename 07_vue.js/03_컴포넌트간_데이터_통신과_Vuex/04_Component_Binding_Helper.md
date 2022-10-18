## Component Binding Helper

* Component Binding Helper
  
  * JS Array Helper Method를 통해 배열 조작을 편하게 하는 것과 유사
    
    * 논리적인 코드 자체가 변하는 것이 아니라 "쉽게" 사용할 수 있도록 되어 있음에 초점
  
  * 종류
    
    * mapState
    
    * mapGetters
    
    * mapActions
    
    * mapMuatations

* Component Binding Helper - **"mapState"**
  
  * computed와 Store의 state를 매핑
  
  * Vuex Store의 하위 구조를 반환하여 component 옵션을 생성함
  
  * 매핑된 computed 이름이 state 이름과 같을 때 문자열 배열을 전달 할 수 있음
  
  ```javascript
  computed: {
      todos:function () {
          return this.$store.state.todos
      }
  }
  ```
  
  ```html
  // TodoList.vue
  import {mapState} from 'vuex'
  ...
  computed: mapState([
      'todos'
  ])
  ```
  
  * 하지만 다른 computed 값을 함께 사용할 수 없기 때문에 최종 객체를 computed에 전달할 수 있도록 다음과 같이 객체 전개 연산자(Object Spread Operator)로 객체를 복사하여 작성
    
    * `mapState()`는 객체를 반환함
    
    ```html
    // TodoList.vue
    // 1. vuex 모듈에서 mapState 메서드만 가져옴
    import {mapState} from 'vuex'
    export default {
        ...
        computed: {
        //2. todos는 state에 지정한 이름을 그대로 사
        ...mapState([
        'todos'
        ])
    }
    }
    ```

* Component Binding Helper - **"mapGetters"**
  
  * Computed와 Getters를 매핑
  
  * getters를 객체 전개 연산자 (Object Spread Operator)로 계산하여 추가
  
  * 해당 컴포넌트 내에서 매핑하고자 하는 이름이 index.js에 정의해놓은 getters의 이름과 동일하면 배열의 형태로 해당 이름만 문자열로 추가
  
  ```html
  // App.vue
  import {mapGetters} from 'vuex'
  export default {
      ...
      computed: {
      //mapGetters 안에 정의한 메서드 중에서 아래 3개만 가져와서 Array에 추
      ...mapGetters([
      'completedTodosCount',
      'uncompletedTodosCount',
      'allTodosCount'
          ])
      }
  }
  ```

* Component Binding Helper - **"mapActions"**
  
  * action을 전달하는 컴포넌트 method 옵션을 만듦
  
  * actions를 객체 전개 연산자(Object Spread Operator)로 계산하여 추가하기
  
  * [주의] mapActions를 사용하면 이전에 dispatch()를 사용했을 때 payload로 넘겨줬던 this.todo를 pass prop으로 변경해서 전달해야 함
  
  ```html
  // TodoListItem.vue
  <template>
      <div>
          <div>
              <span
              @click="updateTodoStatus(todo)"
              :class="{'is-completed':todo.isCompleted}"
              >
              {{todo.title}}
              </span>
              <button @click="deleteTodo(todo)">delete</button>
          </div>
      </div>
  </template>
  <script>
  import {mapActions} from 'vuex'
  export default {
      ...,
      methods: {
      ...mapActions([
          'deleteTodo'
          'updateTodoStatus'
          ])
      }
  }
  </script>
  ```
  
  
