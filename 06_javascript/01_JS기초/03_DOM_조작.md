## DOM 조작

* DOM 조작 - 개념
  
  * Document는 문서 한 장(HTML)에 해당하고 이를 조작
  
  * DOM 조작 순서
    
    1. 선택 (Select)
    
    2. 변경 (Manipulation)

* DOM 조작 - Document 위치

![](assets/2022-07-03-12-47-16-image.png)

* DOM 관련 객체의 상속 구조
  
  * `EventTarget`
    
    * Event Listener를 가질 수 있는 객체가 구현하는 DOM 인터페이스
  
  * `Node`
    
    * 여러 가지 DOM 타입들이 상속하는 인터페이스
  
  * `Element`
    
    * Document 안의 모든 객체가 상속하는 가장 범용적인 기반 클래스
    
    * 부모인 Node와 그 부모인 EventTarget의 속성을 상속
  
  * `Document`
    
    * 브라우저가 불러온 웹 페이지를 나타냄
    
    * DOM 트리의 진입점(entry point)역할을 수행
  
  * `HTMLElement`
    
    * 모든 종류의 HTML 요소
    
    * 부모 element의 속성 상속
  
  ![](assets/2022-07-03-12-51-08-image.png)

* DOM 선택 - 선택 관련 메서드
  
  * Document **.querySelector(selector)**
    
    * 제공한 선택자와 일치하는 element 하나 선택
    
    * 제공한 **CSS selector**를 만족하는 **첫 번째 element 객체를 반환** (없다면 null)
  
  * Document **.querySelectorAll(selector)**
    
    * 제공한 선택자와 일치하는 여러 element를 선택
    
    * 매칭 할 하나 이상의 셀렉터를 포함하는 유효한 CSS selector를 인자(문자열)로 받음
    
    * 지정된 셀렉터에 일치하는 `NodeList`를 반환
  
  * getElementById(id)
  
  * getElementsByTagName(name)
  
  * getElemntsByClassName(names)
  
  * **guerySelector(), querySelectorAll()를 사용하는 이유**
    
    * id, class 그리고 tag 선택자 등을 모두 사용 가능하므로, 더 구체적이고 유연하게 선택 가능
    
    * ex) document.querySelector('#id'), document.querySelectAll('.name')

* DOM 선택 - 선택 메서드별 반환 타입
  
  1. 단일 element
     
     * `getElementById()`
     
     * `querySelector()`
  
  2. HTMLCollection
     
     * `getElementsByTagName()`
     
     * `getElementsByClassName()`
  
  3. NodeList
     
     * `querySelectorAll()`

* DOM 선택 - HTMLCollection & NodeList
  
  * 둘 다 배열과 같이 각 항목에 접근하기 위한 `index`를 제공 (유사 배열)
  
  * HTMLCollection
    
    * name, id, index 속성으로 각 항목에 접근 가능
  
  * NodeList
    
    * `index`로만 각 항목에 접근 가능
    
    * 단, HTMLCollection과 달리 배열에서 사용하는 forEach 함수 및 다양한 메서드 사용 가능
  
  * 둘 다 `Live Collection`으로 DOM의 변경사항을 실시간으로 반영하지만 `querySelectorAll()`에 의해 반환되는 NodeList는 Static Collection으로 실시간 반영되지 않음

* DOM 선택 - Collection
  
  * Lice Collection
    
    * 문서가 바뀔 때 실시간으로 업데이트 됨
    
    * DOM의 변경하상을 실시간으로 `collection`에 반영
    
    * ex) HTMLCollection, NodeList
  
  * Static Collection(non-live)
    
    * DOM이 변경되어도 `collection` 내용에는 영향을 주지 않음
    
    * `querySelectorAll()`의 반환 **NodeList만 static collection**

* DOM 변경 - 변경 관련 메서드 (Creation)
  
  * Document **.createElement()**
    
    * 작성한 태그 명의 HTML 요소를 생성하여 반환

* DOM 변경 - 변경 관련 메서드 (append DOM)
  
  * Element **.append()**
    
    * 특정 부모 Node의 자식 NodeList` 중 마지막 자식 다음에 Node 객체나 DOMString을 삽입
    
    * 여러 개의 Node 객체, DOMString 을 추가 할 수 있음
    
    * 반환 값이 없음
  
  * Node.appendChild()
    
    * 한 Node를 특정 부모 Node의 자식 `NodeList` 중 마지막 자식으로 삽입(Node만 추가 가능)
    
    * 한번에 오직 하나의 Node만 추가할 수 있음
    
    * 만약 주어진 Node가 이미 문서에 존재하는 다른 Node를 참조한다면 새로운 위치로 이동

* ParentNode.append() vs Node.appendChild()
  
  * `append()`를 사용하면 DOMString 객체를 추가할 수도 있지만 `.appendChile()`는 Node 객체만  허용
  
  * `append()`는 반환 값이 없지만 `appendChild()`는 추가된 Node 객체를 반환
  
  * `append()`는 여러 Node 객체와 문자열을 추가할 수 있지만, `.appendChiled()`는 하나의 Node객체만 추가할 수 있음

* DOM 변경 - 변경 관련 속성 (property)
  
  * Node **.innerText**
    
    * Node 객체와 그 자손을 텍스트 컨텐츠(DOMString)를 표현 (해당 요6소 내부의 raw text) (사람이 읽을 수 있는 요소만 남김)
    
    * 즉, 줄 바꿈을 인식하고 숨겨진 내용을 무시하는 등 최종적으로 스타일링이 적용된 모습으로 표현
  
  * Element **.innerHTML**
    
    * 요소(element) 내에 포함된 HTML 마크업을 반환
    
    * [참고] XSS 공격에 취약하므로 사용 시 주의

* XSS(Cross-site Scripting)
  
  * 공격자가 웹 사이트 클라이언트 측 코드에 악성 스크립트를 삽입해 공격하는 방법
  
  * 피해자(사용자)의 브라우저가 악성 스크립트를 실행하며 공격자가 엑세스 제어를 우회하고 사용자가를 가장 할 수 있도록 함(CSRF 공격과 유사)

* DOM 삭제 - 삭제 관련 메서드
  
  * ChildNode **.remove()**
    
    * Node가 속한 트리에서 해당 Node를 제거
  
  * Node **.removeChiled()**
    
    * DOM에서 자식 Node를 제거하고 제거된 Node를 반환
    
    * Node는 인자로 들어가는 자식 Node의 부모 Node

* DOM 속성 - 속성 관련 메서드
  
  * Element **.setAttribute(name, value)**
    
    * 지정된 요소의 값을 설정
    
    * 속성이 이미 존재하면 값을 갱신, 존재하지 않으면 지정된 이름과 값으로 새 속성을 추가
  
  * Element **.getAttribute(attributeName)**
    
    * 해당 요소의 지정된 값(문자열)을 반환
    
    * 인자(attributeName)는 값을 얻고자 하는 속성의 이름
