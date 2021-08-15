## CSS Layout

* CSS Layout
  * 웹 페이지에 포함되는 요소들을 어떻게 취합하고 그것들이 어느 위치에 놓일 것인지를 제어
* CSS page layout techniques
  * `display`
  * `position`
  * `float`
  * `felxbox`
  * `Grid system`





---

## Float

* Float
  * 한 요소가 정상흐름으로 부터 빠져 텍스트 및 인라인 요소가 그 주위를 감싸 요소의 좌,우측을 따라 배치되어야 함을 지정
  * 본래는 이미지를 한쪽으로 띄우고 텍스트를 둘러싸는 레이아웃을 위해 도입
  * 더 나아가 이미지가 아닌 다른 요소들에도 적용해 웹 사이트의 전체 레이아웃을 만드는데까지 발전
* Float 속성
  * `none` : 기본값
  * `left` : 요소를 왼쪽으로 띄움
  * `right` : 요소를 오른쪽으로 띄움
* Float clear - 가상 요소
  * 선택한 요소의 맨 마지막 자식으로 가상(의사)요소를 하나 생성
  * 보통 `content` 속성과 함께 짝지어, 요소에 장식용 콘텐츠를 추가할 때 사용
  * 기본값은 `inline`

  * 선행 `floating` 요소 다음일 수 있는지 또는 그 아래로 내려가(해제되어 cleared)야 하는 지를 지정
  * `clear`속성은 부동(float) 및 비부동(float) 요소 모두에 적용됨
* Float 정리
  * `flexbox` 및 그리드 레이아웃과 같은 기술이 나오기 이전에 `float`은 열 레이아웃을 만드는데 사용됨
  * `flexbox`와 `grid`의 출현과 함께 결국 원래 텍스트 블록 내에서 `float` 이미지를 위한 역할로 돌아감
    * MDN에서는 더 새롭고 나은 레이아웃 기술이 나와있으므로 "legacy 레이아웃 기술"로 분류
  * 웹에서 여전히 사용하는 경우도 있음 (ex. naver의 nav bar)
