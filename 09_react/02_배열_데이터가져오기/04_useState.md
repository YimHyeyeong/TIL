## useState

+ 초깃값 지정하기
  
  ```
  const [state, setState] = useState(initialState);
  ```
  
  + `useState` 함수에 값을 전달하면 초깃값으로 지정할 수 있음
  
  + 콜백으로 초깃값 지정하기
    
    ```
    const [state, setState] = useState(() => {
      // 초기값을 계산
      return initialState;
    });
    ```
    
    + 초깃값을 계산해서 넣는 경우 사용
    
    ```
    function ReviewForm() {
      const savedValues = getSavedValues(); // ReviewForm을 렌더링할 때마다 실행됨
      const [values, setValues] = useState(savedValues);
      // ...
    }
    ```
    
    + 매 렌더링마다 불필요하게 `getSavedValues` 함수를 실행해서 저장된 값을 가져옴
    
    ```
    function ReviewForm() {
      const [values, setValues] = useState(() => {
        const savedValues = getSavedValues(); // 처음 렌더링할 때만 실행됨
        return savedValues
      });
    ```
    
    + 처음 렌더링 할 때 한 번만 콜백을 실행해서 초깃값을 만듦
    + 이 경우 리턴될 때까지 리액트가 렌더링하지 않고 기다리기 때문에 콜백함수 실행이 오래 걸릴 수록 초기 렌더링이 늦어질 수 있음

+ Setter 함수 사용하기
  
  + 기본
  
  ```
  const [state, setState] = useState(0);
  
  const handleAddClick = () => {
    setState(state + 1);
  }
  ```
  
  + 참조형 State 값 변경하기
    
    ```
    const [state, setState] = useState({ count: 0 });
    
    const handleAddClick = () => {
      setState({ ...state, count: state.count + 1 }); // 새로운 객체 생성
    }
    ```
    
    + 콜백으로 State 변경
      
      ```
      setState((prevState) => {
        // 다음 State 값을 계산
        return nextState;
      });
      ```
      
      + 비동기 함수에서 State를 변경하게 되면 최신 값이 아닌 State 값을 참조하는 문제가 있을 수 있음
      + 이땐 콜백을 사용해서 파라미터로 올바른 State값을 가져와서 사용해야 함