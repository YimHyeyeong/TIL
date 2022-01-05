## Bootstrap

* The most popular HTML, CSS, and JS library in the world
  * 트위터에서 시작된 오픈 소스 프론트엔드 라이브러리
  * 웹 페이지에서 많이 쓰이는 요소 거의 전부를 내장하고 있음 
  * 별도의 디자인을 할 시간이 크게 줄어들고 여러 웹 브라우저를 지원하기 위한 크로스 브라우징에 불필요한 시간을 사용하지 않도록 함
* CDN
  * `Content Delivery(Distribution) Network`
  * 컨텐츠(CSS, JS, Image 등)을 효율적으로 전달하기 위해 여러 노드에 가진 네트워크에 데이터를 제공하는 시스템
    * 개별 `end-user`의 가까운 서버를 통해 빠르게 전달 가능(지리적 이점) 외부 서버를 활용함으로써 본인 서버의 부하가 적어짐
* Responsive Web Design
  * 다양한 화면 크기를 가진 디바이스들이 등장함에 따라 `responsive web design` 개념이 등장
  * 반응형 웹은 별도의 기술 이름이 아닌 웹 디자인에 대한 접근 방식, 반응형 레이아웃 작성에 도움이 되는 사례들의 모음 등을 기술하는데 사용되는 용어





---

## Bootstrap Grid System

* Grid system
  * Bootstrap Grid system은 `flexbox`로 제작됨
  * `container`,`rows`,`column`으로 컨텐츠를 배치하고 정렬
  * 반드시 기억해야 할 2가지
    1. 12개의 `column`
    2. 6개의 `grid breakpoints`
* Grid system class
  * row
    * columns의 `wrapper`
  * gutters
    * grid 시스템에서 반응적으로 공간을 확보하고 컨텐츠를 정렬하는데 사용되는 column사이의 `padding`
  * col, col-*
    * column class는 row당 가능한 12개 중 사용하려는 `column`수를 나타냄
    * columns 너비는 백분율로 설정 되므로 항상 부모 요소를 기준으로 유동적으로 크기가 조정됨
    * `grid layout`에서 내용은 반드시 columns 안에 있어야 하며 오직 columns 만 row의 바로 하위 자식일 수 있음
  * offset
    * 지정한 만큼의 `column` 공간을 무시하고 다음 공간부터 컨텐츠를 적용
  * Nesting(중첩)
    * row > col-* > row > col-* 의 방식으로 중첩 사용 가능
* Grid breakpoints
  * 다양한 디바이스에서 적용하기 위해 특정 px(픽셀) 조건에 대한 지점을 정해 두었는데 이를 `breakpoints` 라고 함
  * bootstrap은 배부분의 크기를 정의하기위해 `em` 또는 `rem` 을 사용하지만 px은 `gird breakpoint` 에 사용
    * `viewport` 너비가 픽셀 단위이고 글꼴 크기에 따라 변하지 않기 때문 