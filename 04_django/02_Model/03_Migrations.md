## Migrations

+ MIgrations

  * "django가 model에 생긴 변화(필드를 추가했다던가 모델을 삭제했다던가 등)를 DB에 반영하는 방법"
  * Migration 실행 및 DB 스키마를 다루기 위한 몇가지 명령어
    * **makemigrations**
    * **migrate**
    * sqlmigrate
    * showmigrations

+ Migrations Commands

  > migrations = 설계도/ 모델이 설계도(마이그레이션스)를 지나서 DB로 감 / 마이그레이션은 설계도가 쌓이는 공간

  1. `makemigrations`
     * model을 변경한 것에 기반한 새로운 마이그레이션(like **설계도**)을 만들 때 사용
  2. `migrate`
     * 마이그레이션을 DB에 반영하기 위해 사용
     * 설계도를 실제 DB에 반영하는 과정
     * 모델에서의 변경 사항들과 DB의 스키마가 동기화를 이룸
  3. `sqlmigrate`
     * 마이그레이션에 대한 SQL 구문을 보기 위해 사용
     * 마이그레이션이 SQL 문으로 어떻게 해석되어서 동작할지 미리 확인 할 수 있음
  4. `showmigrations`
     + 프로젝트 전체의 마이그레이션 상태를 확인하기 위해 사용
     + 마이그레이션 파일들이 migrate 됐는지 안됐는지 여부를 확인 할 수 있음

+ 실습

  ```
  $ python manage.py makemigrations
  ```

  ```
  $ python manage.py migrate
  ```

  ```
  $ python manage.py sqlmigrate app_name 0001
  
  # 해당 migratrions 설계도가 SQL 문으로 어떻게 해석되어서 동작할지 미리 학인 할 수 있음
  ```

  ```
  $ python manage.py showmigrations
  
  # migrations 설계도들이 migrate 됐는지 안됐는지 여부를 확인 할 수 있음
  ```

+ DateFiled's options

  * `auto_now_add`
    * 최초 생성 일자
    * django ORM이 최초 insert(데이블에 데이터 입력)시에만 현재 날짜와 시간으로 갱신(테이블에 어떤 값을 최초로 넣을 때)
  * `auto_now`
    * 최종 수정 일자
    * django ORM이 save를 할 때마다 현재 날짜와 시간으로 갱신

+ 반드시 기억해야 할 `migration` 3단계

  1. models.py
     + `model` 변경사항 발생
  2. python manage.py makemigrations
     + `migrations` 파일 생성
  3. python manage.py migrate
     + DB 적용

