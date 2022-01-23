## JSX

* react에서 사용하는 문법

  * html 태그를 js에서 사용함
  * js의 확장 문법

* html과 같이 태그에 속성을 추가할 수 있음

  ```
  ReactDOM.render(
    <p id="hello">안녕 리액트!</p>,
    document.getElementById('root')
  );
  ```

* `jsx` 는 js의 확장 문법이기 때문에 완전히 html 을 사용할 순 없음

  1. class

     * html에서는 css에 class 속성을 적용하는 문법이지만 자바스크립트는 객체지향의 개념임

       ```
       # javascript
       class Dice {
       	roll() {
       		console.log('Roll');
       	}
       }
       ```

       ```
       # html
       <p class="hello">안녕</p>
       ```

     * 따라서, `jsx` 에서는 `className`이라는 속성을 사용해야 함

  2. for

     * html에서는 label 태그와 input 태그를 연결할 때 사용하지만 javascript에서는 반복문 for를 만들 때에 사용함
     * 따라서, `jsx` 에서는 `htmlFor`이라는 속성을 사용해야 함

  3. 이벤트 핸들러

     * html에서 input태그의 이벤트 핸들러는 모두 소문자였음

       ```
       <input id="name" type="text" onblur="" onfocus="" onmousedown=""/>
       ```

     * `jsx`에서는 두번째 단어부터 **대문자**로 작성해주어야 함 (카멜케이스)

       ```
       <input id="name" type="text" onBlur="" onFocus="" onMouseDown=""/>
       ```

     * `jsx` 문법에서는 여러 단어가 조합된 경우에는 **카멜 케이스**로 작성됨
     
       * 하지만, 예외적으로 `HTML` 에서 비표준 속성을 다룰 때 활용하는 `data-*` 속성은 카멜 케이스가 아니라 기존의 `HTML` 문법 그대로 작성해야 함
     
         ```
         <>
         	<button data-status="대기중"></button>
         </>
         ```
     
         

* **반드시 jsx는 한 개의 html 태그로 감싸져야 함!!**

* Fragment

  * 하지만 굳이 div 태그로 감싸고 싶지 않다면 리액트에서 제공하는 `Fragment`를 사용함

    ```
    import { Fragment } from 'react';
    
    <Fragment>
    <h1>안녕 리액트!</h1>
    <p>하이하이</p>
    </Fragment>,
    ```
  
  
    * 축약형
  
      ```
      <>
          <h1>안녕 리액트!</h1>
          <p>하이하이</p>
      </>,
      ```
  
      * fragment를 지우고 사용하여도 됨
  


* `JSX` 에서 javascript 표현식 넣기

  * `{}` 중괄호를 사용

    ```
    const product = '맥북';
    
    ReactDOM.render(
      <h1>나만의 {product} 주문하기</h1>,
      document.getElementById('root')
    );
    ```

  * 중괄호 안에서는 자바스크립트로 된 표현식은 모두 사용가능

    ```
    # 대문자로 출력
    <h1>나만의 {product.toUpperCase()} 주문하기</h1>,
    ```

  * 덧셈 연산자 사용

    ```
    const product = 'Macbook';
    const model ='Air';
    
    <h1>나만의 {product + model} 주문하기</h1>,
    ```

    * 하지만 이것보다는 미리 값을 정해서 jsx에서는 보기 쉽게 하는 것이 좋음

      ```
      const product = 'Macbook';
      const model ='Air';
      const item = product + model;
      
      <h1>나만의 {item} 주문하기</h1>,
      ```

  * 이미지 태그

    ```
    const imageUrl = '';
    
    <img src={imageUrl} alt=''/>
    ```

    * src에서 따옴표가 아닌 중괄호

  * 이벤트 핸들러

    * `addEventListener` 보다는 요소의 속성 값으로 등록함

      ```
      function handleClick() {
        alert('춘식쓰')
      }
      
      <button onClick={handleClick}>확인</button>
      ```

  * `{}` 중괄호 안에는 javascript 표현식만 사용할 수 있기 때문에 if문이나 for문, 함수 선언과 같은 문장은 사용할 수 없다

