## CSS Box model

* 네모 세상
  * 모든 HTML요소는 `box` 형태로 되어있음
  * 하나의 박스는 네 부분(영역)으로 이루어짐
    * `content`
    * `padding`
    * `border`
    * `margin`
* Box model 구성
  * `margin` : 테두리 바깥의 외부 여백, 배경색을 지정할 수 없다.
  * `Border`: 테두리 영역
  * `Padding`: 테두리 안쪽의 내부 여백 요소에 적용된 배경색, 이미지는 padding까지 적용
  * `content`: 글이나 이미지 등 요소의 실제 내용
  * 방향을 언급하지 않으면 상하좌우 다 적용
  * shorthand (margin/ padding) 
    * 1- 상하좌우 / 2-상하,좌우 / 3-상,좌우,하 / 4-시계방향
  * border도 `shorthand`가 있긴하나 순서 상관없음
* box-sizing
  * css의 너비는 `conten`t 너비가 기본으로 맞춰져 있음 - border - box 기준으로 바꿔야 함
  * 기본적으로 모든 요소의 `box-sizing`은 `content-box`
    * `padding`을 제외한 순수 `contents` 영역만을 box로 지정
  * 다만 우리가 일반적으로 영역을 볼 때는 `border`까지의 너비를 100px 보는 것을 원함
    * 그 경우 `box-sizing`을 `border-box`로 설정
* Box model
  * 마진 상쇄
    * block A의 top과 block B의 bottom에 적용된 각각의 `margin`이 둘 중에서 큰 마진 값으로 결합(겹쳐지게)되는 현상
    * 상하의 경우만 발생함