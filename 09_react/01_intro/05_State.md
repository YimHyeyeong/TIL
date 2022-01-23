## State

* State

  * `State` 값이 변경될 때마다 화면을 새롭게 `rendering` 함

  * `State` 사용하기

    ```
    import { useState } from 'react';
    ```

    * react 패키지에서 `useState` 함수 불러오기

    ```
    function App() {
      const [num, setNum] = useState(1);
      ...
     }
    ```

    * `useState` 함수를 파라미터로 초기값을 받음
    * 함수가 실행되고 나서는 배열의 형태로 요소 2개를 리턴함
    * 따라서. 배열의 `destructuring` 형태로 작성함
    * 2개의 요소 중 첫 번째 (num)는 `State` 값임 = 현재 변수의 값
      * 처음에는 호출할 때 지정한 초기값을 가지고 있음
    * 두 번째는 `Setter` 함수(setNum)
      * 함수를 호출할 때 파라미터로 전달하는 값
      * `State`(=num)이 변경이 되는 것
    * State를 사용할 때는 `num`에 새로운 값을 할당하면서 변경이 되는 것이 아니라 `setNum`을 거쳐서 변경이 됨
      * 그래서. **const** 로 선언한 것
    * `setNum`은 컨벤션: 일반적으로 `setter` 함수의 변수명 앞에 *set* 을 붙임

* random 함수 만들기

  ```
  function random(n) {
    return Math.ceil(Math.random() * n)
  };
  ```

  * 파라미터 n으로 숫자 값을 전달받아서 랜덤한 정수를 반환하는 함수
  
* 참조형 State

  * 배열이나 객체형의 `State`

  ```
  const [gameHistory, setGameHistory] = useState([]);
  ...
  gameHistory.push(nextNum);
  setGameHistory(gameHistory);
  
  # 아무 반응이 없음
  ```

  * gameHistory가 배열이라서 **참조형**이기 때문

    * gameHistory는 그 배열이 가리키고 있는 주소 값을 가지고 있음
    * 메소드를 이용해서 값을 집어넣더라도 gameHistory가 가진 주소값은 전혀 변하지 않음
    * `State` 값이 바뀌어야 새롭게 화면은 렌더하는데 아무리 값이 바뀐 배열을 setter 함수에 담았다고 하더라도 주소값이 변하지 않음
      * `State` 가 바뀌었다고 판단하지 않음

    ```
    const prevHistory = gameHistory;
    gameHistory.push(nextNum);
    console.log(prevHistory===gameHistory);
    
    # True가 출력됨
    ```

    * 배열의 값은 변경되었으나 가리키는 주소값이 바뀌지 않았기 때문에

  * 해결법

    * 전체를 새로 만든다고 생각하는 것

    ```
    setGameHistory([...gameHistory, nextNum]);
    ```

    * 기존의 값을 빈 배열 안에서 펼쳐주고 새로운 값을 추가함.