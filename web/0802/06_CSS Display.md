## CSS Display

* CSS
  * 모든 요소는 네모(박스모델)이고 어떻게 보여지는지(display)에따라 문서에서의 배치가 달라질 수 있다

* display

  * HTML 요소들을 시각적으로 어떻게 보여줄지 결정하는 속성
  * `display` : `block`
    * 줄 바꿈이 일어나는 요소
    * 화면 크기 전체의 가로 폭을 차지한다
    * 블록 레벨 요소 안에 인라인 레벨 요소가 들어갈 수 있음
    * div / ul, ol, li / p / hr / form
    * 기본은 너비의 100%

  * `display` : `inline`
    * 줄 바꿈이 일어나지 않는 행의 일부 요소
    * `content` 너비만큼 가로 폭을 차지한다.
    * `width`, `height`, `margin-top`, `margin-bootom`을 지정할 수 없다.(좌우마진, 패딩은 지정 가능함)
    * 상하 여백은 `line-height`로 지정한다.
    * span / a / img / input, label / b, em, i, strong

  * `display` : `inline-block`
    * `block` 과 `inline`레벨 요소의 특징을 모두 갖는다
    * `inline`처럼 한 줄에 표시 가능하며
    * block처럼 `width, height, margin` 속성을 모두 지정할 수 있다.

  * `display` : `none`
    * 해당 요소를 화면에 표시하지 않는다(공간조차 사라진다)
    * 이와 비슷한 `visibility: hidden` 은 해당 요소가 공간은 차지하나 화면에 표시만 하지 않는다.

* 속성에 따른 수평 정렬

  * 왼쪽에 붙이기 

    1. `margin-right:auto;`
    2. `text-align:left;`

  * 오른쪽에 붙이기

    1. `margin-left:auto;`
    2. `text-align:right;`

  * 중앙에 붙이기

    1. `margin-right:auto;`

       `margin-left:auto;`

    2. `text-align:center;`