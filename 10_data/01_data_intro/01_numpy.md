## numpy

* numpy intro

  * numpy 불러오기

    ```
    import numpy
    ```

  * numpy array 만들기

    ```
    array1 = numpy.array([])
    ```

  * numpy의 배열 모양 확인

    ```
    array1.shape
    ```

    > 1차원 배열일 경우: (11,)
    >
    > 2차원 배열일 경우: (3, 4)

  * numpy 길이

    ```
    array1.size
    ```

* 균일한 값으로 생성

  * numpy 모듈의 `full` 메서드

    ```
    array = numpy.full(6,7)
    ```

    > [7 7 7 7 7 7]

  * 모든 값이 0일 numpy array 생성

    ```
    array1 = numnpy.full(6,0)
    array2 = numpy.zeros(6, dtype=int)
    ```

    > 둘 다 [0 0 0 0 0 0]

  * 모드 값이 1인 numpy array 생성

    ```
    array1 = numnpy.full(6,0)
    array2 = numpy.ones(6, dtype=int)
    ```

    > 둘 다 [1 1 1 1 1 1]

* 랜덤한 값들로 생성

  * numpy 모듈 안에 `random` 이라는 모듈이 있고 그 안에 또 `random`이라는 함수가 있음

    ```
    array1 = numpy.random.random(6)
    array2 = numpy.random.random(6)
    ```

    > [0.42214929 0.45275673 0.57978413 0.61417065 0.39448558 0.03347601]
    >
    > [0.42521953 0.65091589 0.94045742 0.18138103 0.27150749 0.8450694 ]

* 연속된 값들이 담긴 numpy array 생성

  * numpy 모듈의 `arange` 함수

    * `arange` 함수는 파이썬의 기본 함수인 `range`와 비슷함

    * 파라미터 1개

      * `arange(m)`을 하면 `0`부터 `m-1` 까지의 값들이 담긴 numpy array가 리턴

      ```
      array1 = numpy.arange(6)
      ```

      > [0 1 2 3 4 5]

    * 파라미터 2개

      * `arange(n, m)`을 하면 `n`부터 `m-1`까지의 값들이 담긴 numpy array가 리턴

      ```
      array1 = numpy.arange(2, 7)
      ```

      > [2 3 4 5 6]

    * 파라미터 3개

      * `arange(n, m, s)`를 하면 `n`부터 `m-1`까지의 값들 중 간격이 `s`인 값들이 담긴 numpy array가 리턴

      ```
      array1 = numpy.arange(3, 17, 3)
      ```

      > [ 3  6  9 12 15]

* 모듈 별명 붙여주기

  ```
  import numpy as np
  ```

* 인덱싱, 슬라이싱

  * 파이썬에서와 같이 인덱스로 접근 가능

    ```
    array[0]
    array[-1]
    ```

  * 배열로 인덱스줘도 됨

    ```
    array1[[1,3,4]]
    ```

  * numpy array로 인덱스 줘도 됨

    ```
    array2 = np.array([2,1,3])
    array1[array2]
    ```

* 기본 연산

  * array에 연산을 해도 원본 값은 변하지 않음

    ```
    array1 = array * 2
    ```

    * 원본 값을 바꾸려면 다시 저장해주어야 함

  * array 끼리 더하기는 같은 위치의 값들끼리의 더하기

    ```
    array1 + array2
    ```

    > array([10, 12, 14, 16, 18, 20, 22, 24, 26, 28])

* boolean

  * 비교 연산자의 경우. True/False 값으로 반환함

    ```
    array1> 4
    ```

    > array([False, False,  True,  True,  True,  True,  True,  True,  True,
    >         True,  True])

  * `where` 을 사용하는 경우 True의 인덱스를 반환함

    ```
    np.where(booleans)
    ```

    > array([ 0,  1,  3,  4,  6,  7,  8, 10], dtype=int64),)

  * 조건에 맞는 값들을 반환하고 싶은 경우

    ```
    filter = np.where(array1>4)
    filter
    
    array1[filter]
    ```

    > array([ 5,  7, 11, 13, 17, 19, 23, 29, 31])

* 최댓값, 최솟값

  * `max` 메소드와 `min` 메소드

    ```
    array1 = np.array([14, 6, 13, 21, 23, 31, 9, 5])
    
    print(array1.max()) # 최댓값
    print(array1.min()) # 최솟값
    ```

    > 31
    >
    > 5

* 평균값

  * `mean` 메소드

    ```
    array1 = np.array([14, 6, 13, 21, 23, 31, 9, 5])
    
    print(array1.mean()) # 평균값
    ```

    > 15.25

* 중앙값

  * `median` 메소드

    ```
    array1 = np.array([8, 12, 9, 15, 16])
    array2 = np.array([14, 6, 13, 21, 23, 31, 9, 5])
    
    print(np.median(array1)) # 중앙값
    print(np.median(array2)) # 중앙값
    ```

    > 12.0
    >
    > 13.5 (짝수개의 요소이기 때문에 중앙값 13과 14의 평균)

* 표준 편차, 분산

  ```
  array1 = np.array([14, 6, 13, 21, 23, 31, 9, 5])
  
  print(array1.std()) # 표준 편차
  print(array1.var()) # 분산
  ```

  > 8.496322733983215
  >
  > 72.1875