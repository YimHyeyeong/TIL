## ManyToManyField

* ManyToManyField's 개념 및 특징
  
  * 다대다 (M:N, many-to-many) 관계 설정 시 사용하는 모델 필드
  
  * 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스)가 필요
  
  * 모델 필드의 `RelatedManager`를 사용하여 관련 개체를 추가, 제거 또는 만들 수 있음
    
    * **add()**, **remove()**, create(), clear() ...
  
  * [참고] RelatedManager
    
    * 일대다 또는 다대다 관련 컨텍스트에서 사용되는 manager

* ManyToManyField's Arguments
  
  1. related_name
     
     * `target model`(관계 필드를 가지지 않은 모델)이 `source model`(관계 필드를 가진 모델)을 참조할 때(역참조 시) 사용할 **manager**의 이름을 설정
     
     * ForeignKey의 related_name과 동일 
  
  2. through
     
     * 중개 테이블을 직접 작성하는 경우, `through`옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정할 수 있음
     
     * 일반적으로 중개 테이블에 추가 데이터를 사용하는 다대다 관계와 연결하려는 경우(extra data with a many-to-many relationship)에 주로 사용됨
  
  3. symmetrical
     
     * ManyToManyField와 **동일한 모델**(on self)을 가리키는 정의에서만 사용
     
     * symmetrical=True(기본값)일 경우 django는 person_set 매니저를 추가하지 않음
     
     * source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면 target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함
       
       * 즉, 내가 당신의 친구하면 당신도 내 친구가 되는 것
       
       * 대칭을 원하지 않는 경우 False로 설정
     
     ```
     class Person(models.Model):
         friends = models.ManyToManyField('self')
         friends = models.ManyToManyField('self', symmeetricals=False)
     ```

* Related Manager
  
  * 1:N 또는 M:N 관련 컨텍스트에서 사용되는 매니저
  
  * 같은 이름의 메서드여도 각 관계(1:N, M:N)에 따라 다르게 사용 및 동작
    
    * 1:N에서는 target 모델 인스턴스만 사용 가능
    
    * M:N 관계에서는 관련된 두 객체에서 모두 사용 가능
  
  * 메서드 종류
    
    * **add()**, **remove()**, create(), clear(), set() 등
  
  * add()
    
    * "지정된 객체를 관련 객체 집합에 추가"
    
    * 이미 존재하는 관계에 사용하면 관계가 복제되지 않음
    
    * 모델 인스턴스, 필드 값(PK)을 인자로 허용
    
    ```
    doctor1 = Doctor.objects.create(name='justin')
    patient1 = Doctor.objects.create(name='tony')
    
    doctor1.patient_set.add(patient1)
    
    # 또는
    
    patient1.doctors.add(doctor1)
    ```
  
  * remove()
    
    * "관련 객체 집합에서 지정된 모델 객체를 제거"
    
    * 내부적으로 `QuerySet.delete()`를 사용하여 관계가 삭제됨
    
    * 모델 인스턴스, 필드 값(PK)을 인자로 허용
    
    ```
    doctor1 = Doctor.objects.get(pk=1)
    patient1 = Doctor.objects.get(pk=1)
    
    doctor1.patient_set.remove(patient1)
    
    # 또는
    
    patient1.doctors.remove(doctor1)
    ```

* through 예시
  
  * 모델 관계 설정
  
  ```
  # models.py
  
  class Doctor(models.Model):
      name = models.TextField()
  
  class Patient(models.Model):
      doctors = models.ManyToManyField(Doctor, through='Resercation')
      name = models.TextField()
  
  class Reservation(models.Model):
      doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
      patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
      symptom = models.TextField()
      reserved_at = models.DateTimeField(auto_now_add=True)
  ```

* 데이터베이스에서의 표현
  
  * django는 다대다 관계를 나타내는 중개 테이블을 만듦
  
  * 데이블 이름은 다대다 필드의 이름과 이를 포함하는 모델의 테이블 이름을 조합하여 생성됨

* 중개 테이블의 필드 생성 규칙
  
  1. source model 및 target model 모델이 다른 경우
     
     * `id`
     
     * `<containing_model>_id`
     
     * `<other_model>_id`
  
  2. ManyToManyField가 동일한 모델을 가리키는 경우
     
     * `id`
     
     * `from_<model>_id`
     
     * `to_<model>_id`
