## redux toolkit

+ redux toolkit
  + 적은 양의 코드로 redux를 실행하도록 도와줌
  
+ createAction

  + Action하는 것과 똑같음
  + 액션에게 보내고 싶어하는 정보가 무엇이던지 `payload`와 함께 보내짐

+ createRuducer

  + Reducer 함수를 간결하게 해줌

  + 첫 번째 인자는 `initialState`

  + mutate 하기 쉬워짐

  + 이전의 코드에선 새로운 배열을 만들었지만 toolkit에선 mutate가 가능

    + redux toolkit은 뒤에서 immer를 바탕으로 동작하기 때문

  + 새로운 state를 리턴할 수도 있고 mutate할 수도 있음

  + return을 할 때는 무조건 새로운 state여야 하고 mutate할 때는 return 하면 안됨

    ```
    # 리턴하는 경우
    [deleteToDo] : (state, action) => 
        state.filter(toDo => toDo.id !== action.payload)
    ```

    ```
    # 새로운 state를 return하는데 다음과 같이 중괄호로 감싸면 리턴이 아님
    [deleteToDo] : (state, action) =>
        {state.filter(toDo => toDo.id !== action.payload)}
    ```

+ configureStore

  + 미들웨어와 함께 store를 생성
  + default를 추가
  + redux developer tool을 자동으로 실행시켜줌

+ createSlice

  + reducer, actions 등등의 역할을 함
  + toDos.reducer / toDos.actions 하면서 reducer, actions에 접근