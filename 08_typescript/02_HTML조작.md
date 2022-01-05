## HTML 조작

+ narrowing

  ```
  <h3 id="title"> 안녕하세요 </h4>
  ```

  ```
  let 제목 = document.querySelector('#title')
  if (제목 != null) {
  	제목.innerHTML = '반가워요'
  }
  ```

  + 제목이라는 변수는 union 타입 - `Element` 나 `null`을 가질 수 있음(변수명 위에 마우스를 오버하면 뜨는 정보)
  + 정확한 타입을 나타내주지 않아서 에러뜸
  + 타입을 하나로 `narrowimg`해주면 됨
  + 왜 `union` 타입이냐? HTML 요소를 잘못찾을 경우 `null`이 남기때문에, 잘 찾으면 `html object` 자료 남음

+ HTML 조작시 narrowing 하는 방법 5개

  2. `instanceof` 연산자

     ```
     ...
     if (제목 instanceof Element){
     	제목.innerHTML = '반가워요'
     }
     ```

     + 가장 많이 사용함
     + class 에서 `object instanceof class명` = object가 오른쪽에 기입한 클래스의 자식(인스턴스)냐? 묻는것, 맞다면 `true` 반환

  3. `as` 키워드 사용

     ```
     let 제목 = document.querySelector('#title') as Element;
     ```

     + 이 요소는 타입을 이걸(Element)로 간주해주세요
     + 잘못입력해서 `null`이 들어와도 `Element`로 간주
     + 자주 쓰면 안됨.

  4. 오브젝트에 붙이는 ?

     ```
     let 제목 = document.querySelector('#title')
     if (제목?.innerHTML) {
     	제목.innerHTML = '반가워요'
     }
     ```

     + 제목에 `innerHTML`이 있으면 출력해주고
     + 없으면 `undefined`
     + = `optional chaining`

  5. `strict` 모드 끄기

     + tsconfig.json 파일에서 `strictNullChecks`를 `false`로 바꾸기

+ 그냥 자바스크립트로 써보기

  ```
  let 제목 = document.querySelector('#title')
  if (제목?.innerHTML != undefined) {
  	제목.innerHTML = '반가워요'
  }
  ```

+ <a> 태그의 href 속성 바꾸기

  ```
  let 링크 = document.querySelector('.link');
  if (링크 instanceof HTMLAnchorElement){
  	링크.href = 'https://'
  }
  ```

  + <a> 에 필요한 정확한 타입명이 있음
  + `HTMLAnchorElement`, `HTMLHeadingElement`,`HTMLButtonElement`는 모두 Element를 상속받음
    + HTMLAnchorElement
      + href, style,class .. => object 타입정의 잘되어있음
    + HTMLHeadingElement
      + h1..
    + HTMLButtonElement
      + button

+ eventListener 부착하는 법

  ```
  let 버튼 = document.querySelector('#button');
  버튼?.addEventListener('click', function(){
  	
  })
  ```

  + 버튼에 `addEventListener` 가능하면 해주고 아니라면 `undefined`