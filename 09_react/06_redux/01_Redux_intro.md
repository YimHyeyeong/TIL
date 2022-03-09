## Redux intro

+ Redux intro

  + `Javascript application`의 `state`를 관리하는 방법
  + *React*와 *Redux*는 별개
    + 꼭 react에서만 써야하는 것은 아님
    + `Javascript`언어에서 자유롭게 사용가능

+ Redux 설치

  ```
  npm install redux
  ```

+ Store

  ```
  import {createStore} from "redux";
  ```

  + store는 data(=state)를 넣는 곳

+ reducer

  ```
  const reducer = (state) => {
  	return state
  };
  
  const store = createStore(reducer)
  ```

  + 함수로 만들어져야 함

  + store를 만들면 reducer를 만들어야 한다.

  + 오직 reducer만 데이터를 수정함

  + reducer가 return 하는 것은 application의 data가 됨

  + store를 만들면 reducer는 `initial state`(=undefined)를 불러옴

    ```
    # initializing 해주기
    
    const countModifier = (state = 0) => {
      return state
    };
    ```

+ Action

  + 현재 state와 action 2개의 인자를 가짐

  + reducer의 state를 변경시킴

  + store.dispatch 형태로 Action을 불러옴

  + Action은 type: 객체 형태로 써주어야 함

  + dispatch를 통해 reducer를 부르면 reducer(curentState=0, {type:""}) 형태로 불러짐

  + HTML 과 dispatch를 연결할 때 무제함수여야 함
  
    ```
    add.addEventListener("click",() => countStore.dispatch({type:"ADD"}))
    ```
  
    ```
    const handleAdd = () => {
    	countStore.dispatch({type:"ADD"})
    }
    
    add.addEventListener("click",handleAdd)
    ```

  + dispatch는 store와 커뮤니케이션을 함

  + subscribe는 state에 변화가 있을 때마다 불러지는 메서드

  + dispatch를 사용해 reducer를 불러서 current sate와 action을 더함

  + action은 반드시 object여야 함 string 안됨

  + action은 반드시 type이 있어야 하고 이것은 임의로 이름은 바꿀 수 없음

  + reducer를 사용해 state를 바꿀 때에는 if else 보다 switch를 사용하는 게 더 나음

  + type : 뒤에는 string보다 string 을 변수로 만들어서 변수를 주는 것이 훨씬 낫고 에러 잡기에 쉬움
  
    ```
    const ADD = "ADD"
    
    const handleAdd = () => {
    	countStore.dispatch({type:ADD})
    }
    ```
  
    
