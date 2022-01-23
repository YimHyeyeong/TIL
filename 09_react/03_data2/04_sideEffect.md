## sideEffect

+ 이미지 파일 미리보기

  + objectURL은 `createdObjectURL`이라는 함수로 만들 수 있고 이 함수는 문자열을 리턴함
  + blob:https:// ... 하는 이미지 주소를 만들어 줌 이것이 바로 objectURL
  + objectURL을 만들면 웹 브라우저는 메모리를 할당하고 해당하는 주소를 만듦
  + 파일 인풋은 렌더링하는 과정에서 리액트 외부의 상태를 변경하게 됨
  + 이러한 것은 sideEffect라고 함
  + 네트워크 리퀘스트도 일종의 sideEffect / 웹 브라우저의 상태를 바꿔서 네트워크리퀘스트를 보내고 받아온 리스폰스를 활용하기 때문
  + sideEffect를 활용할 때 **useEffect**를 사용함

  ```
  useEffect(() => {
      if (!value) return;
      const nextPreview= URL.createdObjectURL(value);
      setPreview(nextPreview)
  
      return () => {
        setPreview();
        URL.revokeObjectURL(nextPreview)
      }
    }, [value])
  ```

  + 파일을 선택하면 value의 값이 바껴서 재렌더링이 되고 useEffect의 *콜백함수* 가 실행이 됨
  + objectURL을 만들면서 웹 브라우저가 할당한 메모리가 sideEffect
  + 콜백함수는 마지막으로 정리함수를 리턴함
  + 리액트는 일단 이 정리함수를 기억해뒀다가 다시 value의 값이 바뀌면 다시 렌더링을 해줌
  + 그 전에 정리함수를 실행해 sideEffect를 정리함

+ 사이드 이펙트란?

  + 외부에 부수적인 작용을 하는 것

  ```
  let count = 0;
  
  function add(a, b) {
    const result = a + b;
    count += 1; // 함수 외부의 값을 변경
    return result;
  }
  
  const val1 = add(1, 2);
  const val2 = add(-4, 5);
  ```

  + add 함수는 실행하면서 함수외부의 상태(count변수)가 바뀌기때문에 사이드이펙트가 있다고 함

+ 사이드 이펙트와 useEffect

  + `useEffect` 는 리액트 컴포넌트 함수 안에서 **사이드 이펙트를 실행하고 싶을 때** 사용하는 함수

  + DOM 노드를 직접 변경한다거나, 브라우저에 데이터를 저장하고, 네트워크 리퀘스트를 보내는 것처럼 말이죠.

  + **리액트 외부에 있는 데이터나 상태를 변경할 때** 사용

  + 페이지 정보 변경

    ```
    useEffect(() => {
      document.title = title; // 페이지 데이터를 변경
    }, [title]);
    ```

  + 네트워크 요청

    ```
    useEffect(() => {
      fetch('https://example.com/data') // 외부로 네트워크 리퀘스트
        .then((response) => response.json())
        .then((body) => setData(body));
    }, [])
    ```

  + 데이터 저장

    ```
    useEffect(() => {
      localStorage.setItem('theme', theme); // 로컬 스토리지에 테마 정보를 저장
    }, [theme]);
    ```

    + `localStorage` 는 웹 브라우저에서 데이터를 저장할 수 있는 기능

  + 타이머

    ```
    useEffect(() => {
      const timerId = setInterval(() => {
        setSecond((prevSecond) => prevSecond + 1);
      }, 1000); // 1초마다 콜백 함수를 실행하는 타이머 시작
      
      return () => {
        clearInterval(timerId);
      }
    }, []);
    ```

    + `setInterval` 이라는 함수를 쓰면 일정한 시간마다 콜백 함수를 실행

* useEffect를 쓰면 좋은 경우

  * useEffect는 동기화에 쓰면 유용함

  * 동기화는 컴포넌트 안에 데이터와 리액트 바깥에 있는 ㅔ이터를 일치시키는 것을 의미함

  * 인풋 입력에 따라 페이지 제목을 바꾸는  컴포넌트: 핸들러 함수만 사용한 예시

    ```
    const INITIAL_TITLE = 'Untitled';
    
    function App() {
      const [title, setTitle] = useState(INITIAL_TITLE);
    
      const handleChange = (e) => {
        const nextTitle = e.target.value;
        setTitle(nextTitle);
        document.title = nextTitle;
      };
    
      const handleClearClick = () => {
        const nextTitle = INITIAL_TITLE;
        setTitle(nextTitle);
        document.title = nextTitle;
      };
    
      return (
        <div>
          <input value={title} onChange={handleChange} />
          <button onClick={handleClearClick}>초기화</button>
        </div>
      );
    }
    ```

    * 모두 `title` 스테이트를 변경한 후에 `document.title` 도 함께 변경
    * `document.title` 값을 바꾸는 건 외부의 상태를 변경하는 거니까 사이드 이펙트
    * 만약 새로 함수를 만들어서 `setTitle` 을 사용하는 코드를 추가할 때마다`document.title` 값도 변경해야 한다는 걸 기억해뒀다가 관련된 코드를 작성해야 한다는 점이 단점

  * 인풋 입력에 따라 페이지 제목을 바꾸는  컴포넌트: useEffect를 사용한 예시

    ```
    import { useEffect, useState } from 'react';
    
    const INITIAL_TITLE = 'Untitled';
    
    function App() {
      const [title, setTitle] = useState(INITIAL_TITLE);
    
      const handleChange = (e) => {
        const nextTitle = e.target.value;
        setTitle(nextTitle);
      };
    
      const handleClearClick = () => {
        setTitle(INITIAL_TITLE);
      };
    
      useEffect(() => {
        document.title = title;
      }, [title]);
    
      return (
        <div>
          <input value={title} onChange={handleChange} />
          <button onClick={handleClearClick}>초기화</button>
        </div>
      );
    }
    ```

    +  `document` 를 다루는 사이드 이펙트 부분만 따로 처리
    +  `setTitle` 함수를 쓸 때마다 `document.title` 을 변경하는 코드를 신경 쓰지 않아도 되니까 편리
    +  `useEffect` 는 리액트 안과 밖의 데이터를 일치시키는데 활용
    +  `useEffect` 를 사용했을 때 반복되는 코드를 줄이고, 동작을 쉽게 예측할 수 있는 코드를 작성할 수 있기때문

* 정리 함수

  ```
  useEffect(() => {
    // 사이드 이펙트
  
    return () => {
      // 사이드 이펙트에 대한 정리
    }
  }, [dep1, dep2, dep3, ...]);
  ```

  + `useEffect` 의 콜백 함수에서 사이드 이펙트를 만들면 정리가 필요한 경우

  + 콜백 함수에서 리턴 값으로 정리하는 함수를 리턴

  + 리턴한 정리 함수에서는 사이드 이펙트에 대한 뒷정리를 함

  + 정리 함수가  실행되는 시점

    + **콜백을 한 번 실행했으면, 정리 함수도 반드시 한 번 실행된다**
    + 정확히는 새로운 콜백 함수가 호출되기 전에 실행되거나 (앞에서 실행한 콜백의 사이드 이펙트를 정리), 컴포넌트가 화면에서 사라지기 전에 실행됩니다 (맨 마지막으로 실행한 콜백의 사이드 이펙트를 정리).
    + 예시: 타이머

    ```
    import { useEffect, useState } from 'react';
    
    function Timer() {
      const [second, setSecond] = useState(0);
    
      useEffect(() => {
        const timerId = setInterval(() => {
          console.log('타이머 실행중 ... ');
          setSecond((prevSecond) => prevSecond + 1);
        }, 1000);
        console.log('타이머 시작 🏁');
    
        return () => {
          clearInterval(timerId);
          console.log('타이머 멈춤 ✋');
        };
      }, []);
    
      return <div>{second}</div>;
    }
    
    function App() {
      const [show, setShow] = useState(false);
    
      const handleShowClick = () => setShow(true);
      const handleHideClick = () => setShow(false);
    
      return (
        <div>
          {show && <Timer />}
          <button onClick={handleShowClick}>보이기</button>
          <button onClick={handleHideClick}>감추기</button>
        </div>
      );
    }
    ```

    + 일정한 시간 간격마다 콜백 함수를 실행하는 `setInterval` 이라는 함수도 정리가 필요한 사이드 이펙트
    + 렌더링이 끝나면 타이머를 시작하고, 화면에서 사라지면 타이머를 멈춤
    + 사용자가 '보이기' 버튼을 눌렀을 때 `show` 값이 참으로 바뀌면서 조건부 렌더링에 의해서 `Timer` 컴포넌트를 렌더링 
    + 조건부 렌더링에 의해서 `Timer` 컴포넌트를 렌더링 
    + 다시 사용자가 '감추기' 버튼을 누르면 `show` 값이 거짓으로 바뀌면서 조건부 렌더링에 의해서 이제 `Timer` 컴포넌트를 렌더링 하지 않음
    + 그럼 리액트에선 마지막으로 앞에서 기억해뒀던 정리 함수를 실행