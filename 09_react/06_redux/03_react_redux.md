## react redux

+ store를 index.js에 연결시켜 줘야 함

  ```
  import { Provider } from "react-redux";
  import store from "./store"
  ```

  ```
  <Provider store={store}>
      <App/>
  </Provider>
  ```

+ connect

  + `mapStateToProps` 라고 함

  ```
  function mapStateToProps(state, ownProps?)
  ```

  + 2개의 인자를 받아옴

  + state는 store에서 오는 state 값

  + ownProps는 react-router를 통해 component에 전달된 props

  + 무조건 `return` 을 줘야 함

  + connect()는 컴포넌트로 보내는 props에 추가될 수 있도록 허용함

  + `mapDispatchToProps`는 connect의 2번째 인자

  + 만약 mapState가 필요하지 않고 mapDispatch만 필요한 경우

    ```
    connect(mapStateToProps, mapDispatchToProps) (컴포넌트 이름)
    ```

  + mapDispatchToProps의 첫번째인자로 dispatch, 두 번째 인자로 ownProps

  + ownProps은 현재 받아온 prop=state를 의미

  + 게시물 중 현재 선택한 id와 같은 id를 찾기 위해서 find 메서드 사용