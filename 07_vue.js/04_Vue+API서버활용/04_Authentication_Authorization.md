## Authentication & Authorization

* Authentication
  * 인증, 입증
  * 자신이라고 주장하는 사용자가 누구인지 확인하는 행위
  * 모든 보안 프로세스의 첫 번째 단계 (가장 기본 요소)
  * 즉, 내가 누구인지를 확인하는 과정
  * `401 Unauthorized`
    * 비록 HTTP 표준에서는 "미승인(unauthorized)"을 하고 있지만, 의미상 이 응답은 "비인증(unauthenticated)"을 의미
* Authorization
  * 권한 부여, 허가
  * 사용자에게 특정 리소스 또는 기능에 대한 액세스 권한을 부여하는 과정(절차)
  * 보안 환경에서 권한 부여는 항상 인증을 따라야 함
    * 예를 들어, 사용자는 조직에 대한 액세스 권한을 부여 받기 전에 먼저 자신의 ID가  진짜인지 먼저 확인해야 함
  * 서류의 등급, 웹 페이지에서 글을 조회 & 삭제 & 수정 할 수 있는 방법, 제한 구역
    * 인증이 되었어도 모든 권한을 부여 받는 것은 아님
  * `403 Forbidden`
    * 401과 다른 점은 서버는 클라이언트가 누구인지 알고 있음
* Authentication vs Authorization
  * `Authentication`(인증)
    * "Authentication is the process of **verifying who a user is**"
    * 401 Unauthorized
    * 자신이라고 주장하는 유저 확인
    * *Credentials*(비밀번호, 얼굴인식) 검증
    * Django -> 게시판 서비스 로그인
    * 인증 이후에 획득하는 권한(생성, 수정, 삭제)
  * `Authorization`(권한/허가)
    * "Authorization is the process of **verifying what they have access to**"
    * 403 Forbidden
    * 유저가 자원에 접근할 수 있는지 여부 확인
    * 규칙/규정에 의해 접근할 수 있는지 확인
    * DJango -> 일반 유저 vs 관리자 유저
    * 인증 이후에 부여되는 권한
      * 예시 -로그인 후 글 작성 여부
* Authentication and authorization work together
  * 회원 가입을 하고 로그인을 하면 할 수 있는 권한 생성
    * 인증 이후에 권한이 따라오는 경우가 많음
  * 단, 모든 인증을 거쳐도 권한이 동일하게 부여되는 것은 아님
    * Django에서 로그인을 했더라도 다른 사람의 글까지 수정 / 삭제가 가능하진 않음
  * 세션, 토큰, 제 3자를 활용하는 등의 다양한 인증 방식이 존재