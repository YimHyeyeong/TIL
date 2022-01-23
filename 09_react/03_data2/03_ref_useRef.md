## ref / useRef

+ ref

  + 원하는 시점에 DOM에 접근하고 싶을 때 사용
  + DOM 노드는 반드시 렌더링이 끝나야 생기니까 ref의 `current` 값도 컴포넌트가 렌더링 됐을 때에만 생성됨
  + 조건부 렌더링으로 컴포넌트가 사라지면 current 값이 사라질 수 있음

+ Ref 객체 생성

  ```
  import { useRef } from 'react';
  
  const ref = useRef();
  ```

  + useRef 함수로 Ref 객체를 만들 수 있음

+ ref Prop 사용하기

  ```
  const ref = useRef();
  
  <div ref={ref}> ... </div>
  ```

  + `ref` Prop에다가 앞에서 만든 Ref 객체를 내려주면 됨

+ Ref 객체에서 DOM 노드 참조하기

  ```
  const node = ref.current;
  if (node) {
    // node 를 사용하는 코드
  }
  ```

  + Ref 객체의 `current` 라는 프로퍼티를 사용하면 DOM 노드를 참조할 수 있음.
  + `current` 값은 없을 수도 있으니까 반드시 값이 존재하는지 검사하고 사용

+ 예시: 이미지 크기 구하기

  + img 노드의 크기를 ref를 활용해서 출력하는 예시
  + img 노드에는 너비 값인 `width`와 높이 값인 `height` 라는 속성이 있음
  + Ref 객체의 `current`로 DOM노드를 참조해서 두 속성 값을 가져옴

  ```
  import { useRef } from 'react';
  
  function Image({ src }) {
    const imgRef = useRef();
  
    const handleSizeClick = () => {
      const imgNode = imgRef.current;
      if (!imgNode) return;
  
      const { width, height } = imgNode;
      console.log(`${width} x ${height}`);
    };
  
    return (
      <div>
        <img src={src} ref={imgRef} alt="크기를 구할 이미지" />
        <button onClick={handleSizeClick}>크기 구하기</button>
      </div>
    );
  }
  ```

+ FileInput 초기화

  + FileInput의 value 속성은 사용자만 직접 바꿀 수 있음
  + 자바스크립트로 바꿀 때에는 빈문자열로만 바꿀 수 있음
  + 빈 문자열로 바꾸면 선택한 파일이 초기화 됨