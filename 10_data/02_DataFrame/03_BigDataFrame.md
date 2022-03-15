## BigDataFrame

* Big DataFrame 다루기

  * 상위의 n개 행만 보기

    ```
    데이터셋.head(n)
    ```

  * 하위의 n개 행만 보기

    ```
    데이터셋.tail(n)
    ```

  * 크기 구하기

    ```
    데이터셋.shape
    ```

  * 열 정보 구하기

    ```
    데이터셋.columns
    ```

  * 데이터의 전반적인 정보

    ```
    데이터셋.info()
    ```

  * 통계정보

    ```
    데이터.describe()
    ```

  * 오름차순 정렬하기

    ```
    데이터셋.sort_values(by='기준 열', inplace=True/False)
    ```

  * 내림차순 정렬하기

    ```
    데이터셋.sort_values(by='기준 열', ascending=False, inplace=True/False)
    ```

* Series 다루기

  * Series 뽑기

    ```
    데이터셋['열']
    ```

  * Series 종류 뽑기

    ```
    데이터셋['열'].unique()
    ```

  * 종류별 개수

    ```
    데이터셋['열'].value_counts()
    ```

  * 정보확인

    ```
    데이터셋['열'].describe()
    ```

    