## react intro

* react 앱 생성하기

  ```
  npm init react-app .
  ```

  > 폴더 이름을 새폴더로 하고 설치를 진행하니 되지 않음
  >
  > react_intro로 변경 후 진행하니 됨.

* 서버 실행

  ```
  npm run start
  ```

  * 단순히 서버만을 실행하는 것이 아닌 수정 사항을 바로바로 적용시킴

* `public` 에 `index.html` 을 제외한 모든  파일 삭제

* `index.html` 파일

  ```
  <html lang="ko">
  ```

  * 영어에서 한글로 수정

* `src`에서 `index.js` 파일을 제외한 모든 파일 삭제

* 파일의 기능

  * **index.html**

    * 웹 브라우저에서 가장 먼저 실행되는 페이지

  * **index.js**

    * index.html이 실행되고 나서 실행되는 페이지

    * 리액트 코드 중 가장 먼저 실행되는 페이지

    * `render` 메소드로 html 태그를 그려줌

      ```
      ReactDom.render(<h1>안녕</h1>, document.getElementById('root'));
      ```

      * `render` 메소드의 첫 번째 인자값이 h1태그가 됨 (jsx` 코드)
      * 메소드가 실행이 되면 첫 번째 인자값을 바탕으로 새로운 `html` 태그를 만들고 그 요소를 두 번째 인자에 넣는 방식으로 동작함
      * root는 `index.html` 에 있는 div 태그를 말하며 이 곳에 작성한 h1 태그를 넣어준 것
      * render 메소드는 index.js 에서 보통 한 번만 사용됨