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

    
