## HTML 문서 구조화

* 그룹 컨텐츠(블록요소)
  * < p >
  * < hr >
  * < ol >,< ul >
  * < pre >,< blockquote >
  * < div >
* 텍스트 관련 요소(인라인요소-개별적인 요소임)
  * < a >
  * < b > : 표현상 굵게 표시
  * < strong > : 강조 , 시맨틱 태그임
  * < i > vs < em > : `em`은 시맨택 태그임
  * < span > = < div > : `div`는 블록/ `span`은 인라인
  * < br > : 엔터, 다음 단락으로 넘어가는거
  * < img > : 이미지 태그
* 웹 접근성: 고령자나 장애인들이 웹 사이트에서 제공하는 정보를 비장애인들과 동등하게 접근하고 이용할 수 있도록 보장하는 것으로 웹 접근성 준수를 법적의무사항이다.
  * 사각, 이동성, 청각, 인지
  * ex) 네이버 널리
* table 태그: 잘 안써서 그냥 넘어감
  * < tr >, < td >, < th >
  * < thead >, < tbody >, < tfoot > 
  * < caption >
  * 셀 병합 속성 : `colspan`, `rowspan`
  * scope 속성
  * < col > , < colgroup >
* form(!중요!)
  * < form >은 서버에서 처리하는 데이터를 제공하는 역할
  * 사용자입력데이터를 묶어서 서버(목적지)에 전송
  * 기본 속성
    * `action` - 어디로 보낼지, 목적지 설정
    * `method` - 어떤 걸 선택할 지
  * < submit >도 중요한 태그임. form과 단짝인 태그 . 데이터를 보내는 버튼.
* input 태그 
  *  폼 태그 안에 들어있는 거임. 고로 단독으로 input사용 안함. form태그랑 같이 사용
  * 다양한 타입을 가지는 입력 데이터 필드
  * < label > : 서식 입력 요소의 캡션
  * < input > 공통 속성
    * `name`, `placeholder`
    * `required`
    * `autofocus`
  * < input > 요소의 동작은 `type`에 따라 달라짐.
