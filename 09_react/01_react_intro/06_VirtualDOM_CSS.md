## 컴포넌트가 좋은 이유

1. 반복적인 개발이 줄어든다.
2. 오류를 고치기 쉽다.
3. 일을 쉽게 나눌 수 있다.

## 리액트가 렌더링하는 방식

+ 여러 요소를 직접 변경하는 것은 번거롭기 때문에 아예 새롭게 렌더링을 함
+ 하지만 변경할 필요가 없는 요소도 렌더링이 됨
+ 이를 해결하기 위해 `Virtual DOM`(가상 DOM)을 리액트에서 사용함
+ Virtual DOM은 DOM 트리를 본따서 만든 것
+ 엘리먼트를 새로 렌더링 할 때 바로 DOM에 반영하는 것이 아니라 Virtual DOM 에 적용함
+ 미리 적용해보고 실제 DOM과 Virtual DOM을 비교해서 바뀐 부분만 실제 DOM에  반영함
+ 장점:  DOM을 신경쓸 필요 없어서 단순하고 깔끔한 코드를 작성할 수 있다. 변경사항을 모아서 적당히 나눠서 DOM에 전달함- 변경사항을 효과적으로 처리할 수 있다

## 인라인 스타일

+ 리액트에서는 스타일 속성값을 ***객체***로 지정해주어야 함

  ```
  const style = {
    속성: '값',
  };
  ```

  ```
  <button style={style}></button>
  ```

+ CSS에서 - 대쉬 기호가 들어간 값은 -을 지우고 **카멜 케이스**로 작성해야 함

  ```
  background-color:'pink'
  
  backgroundColor:'pink'
  ```

+ style={}에 바로 지정가능

  ```
  style={{backgroundColor:'yellow'}}
  ```

+ 기본 CSS 스탈일 지정

  ```
  const baseCSS= {};
  
  const style1 = {
  	...baseCSS,
  	backgroundColor:'red',
  }
  ```




## CSS 클래스네임

+ css파일을 import할 때에 바로 뒤에 파일 경로를 붙여줌 from 키워드 없이

+ margin과 같은 외부에서 스타일을 지정하는 것은 부모 컴포넌트에서 스타일을 주는 것이 훨씬 더 직관적임

+ 편리하게 클래스네임을 쓰는 방법

  + 템플릿 문자열

    ```
    function Button({ isPending, color, size, invert, children }) {
      const classNames = `Button ${isPending ? 'pending' : ''} ${color} ${size} ${invert ? 'invert' : ''}`;
      return <button className={classNames}>{children}</button>;
    }
    ```

  + 배열

    ```
    function Button({ isPending, color, size, invert, children }) {
      const classNames = [
        'Button',
        isPending ? 'pending' : '',
        color,
        size,
        invert ? 'invert' : '',
      ].join('');
      return <button className={classNames}>{children}</button>;
    }
    ```

  + classnames 라이브러리 

    ```
    $ npm install classnames
    ```

    ```
    import classNames from 'classnames';
    
    function Button({ isPending, color, size, invert, children }) {
      return (
        <button
          className={classNames(
            'Button',
            isPending && 'pending',
            color,
            size,
            invert && 'invert',
          )}>
         { children }
       </button >
      );
    }
    ```

    + 참고 주소: https://www.npmjs.com/package/classnames