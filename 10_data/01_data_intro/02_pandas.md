## pandas

* DataFrame

  * 표형식의 데이터를 담는 자료형
    * 행(row/index)과 열(column)을 가짐
    * `column`: 데이터의 특징
    * `row` : 레코드
  * pandas의 **dataframe**은 `numpy array`를 기반으로 만들어짐
    * 이차원 numpy array의 부가적인 기능들이 추가된 것이라고 보면 됨
    * pandas dataframe은 index 대신 행에 이름을 붙여줄 수 있음
    * 이차원 numpy array에서는 모든 값이 같은 자료형이여야 한다는 제한이 있음, 반면에 pandas dataframe은 다양한 자료형을 담을 수 있음

* From **list of lists**, **array of arrays**, **list of series**

  * 2차원 리스트, 2차원 `numpy array`, `pandas Series`로 ***DataFrame***을 만들 수 있음

  * 따로 `column`과 `row`/`index`에 대한 설정이 없으면 그냥 0,1,2 순서로 값이 매겨짐

    ```
    import numpy as np
    import pandas as pd
    
    two_dimensional_list = [['dongwook', 50, 86], ['sineui', 89, 31], ['ikjoong', 68, 91], ['yoonsoo', 88, 75]]
    
    two_dimensional_array = np.array(two_dimensional_list)
    list_of_series = [
        pd.Series(['dongwook', 50, 86]), 
        pd.Series(['sineui', 89, 31]), 
        pd.Series(['ikjoong', 68, 91]), 
        pd.Series(['yoonsoo', 88, 75])
    ]
    
    # 아래 셋은 모두 동일
    df1 = pd.DataFrame(two_dimensional_list)
    df2 = pd.DataFrame(two_dimensional_array)
    df3 = pd.DataFrame(list_of_series)
    
    print(df1)
    ```
    
    > ```
    >           0   1   2
    > 0  dongwook  50  86
    > 1    sineui  89  31
    > 2   ikjoong  68  91
    > 3   yoonsoo  88  75
    > ```

* From **dict of lists**, **dict of arrays**, **dict of series**

  * 파이썬 `dictionary`로도 ***DataFrame***을 만들 수 있음

  * 사전의 `key`로는 **column** 이름을 쓰고, 그 **column**에 해당하는 리스트, numpy array, 혹은 pandas Series를 사전의 **value**로 넣어주면 됨

    ```
    import numpy as np
    import pandas as pd
    
    names = ['dongwook', 'sineui', 'ikjoong', 'yoonsoo']
    english_scores = [50, 89, 68, 88]
    math_scores = [86, 31, 91, 75]
    
    dict1 = {
        'name': names, 
        'english_score': english_scores, 
        'math_score': math_scores
    }
    
    dict2 = {
        'name': np.array(names), 
        'english_score': np.array(english_scores), 
        'math_score': np.array(math_scores)
    }
    
    dict3 = {
        'name': pd.Series(names), 
        'english_score': pd.Series(english_scores), 
        'math_score': pd.Series(math_scores)
    }
    
    
    # 아래 셋은 모두 동일합니다
    df1 = pd.DataFrame(dict1)
    df2 = pd.DataFrame(dict2)
    df3 = pd.DataFrame(dict3)
    
    print(df1)
    ```

    > ```
    >        name  english_score  math_score
    > 0  dongwook             50          86
    > 1    sineui             89          31
    > 2   ikjoong             68          91
    > 3   yoonsoo             88          75
    > ```

* From **list of dicts**

  * 리스트가 담긴 사전이 아니라, 사전이 담긴 리스트로도 DataFrame을 만들 수 있음.

    ```
    import numpy as np
    import pandas as pd
    
    my_list = [
        {'name': 'dongwook', 'english_score': 50, 'math_score': 86},
        {'name': 'sineui', 'english_score': 89, 'math_score': 31},
        {'name': 'ikjoong', 'english_score': 68, 'math_score': 91},
        {'name': 'yoonsoo', 'english_score': 88, 'math_score': 75}
    ]
    
    df = pd.DataFrame(my_list)
    print(df)
    ```

    > ```
    >    english_score  math_score      name
    > 0             50          86  dongwook
    > 1             89          31    sineui
    > 2             68          91   ikjoong
    > 3             88          75   yoonsoo
    > ```

* dtypes

  * DataFrame에는 다양한 종류의 데이터를 담을 수 있고, 이는 `dtypes`를 사용해서 각 column이 어떤 데이터 타입을 보관하는지 확인할 수 있음

    ```
    import pandas as pd
    
    two_dimensional_list = [['dongwook', 50, 86], ['sineui', 89, 31], ['ikjoong', 68, 91], ['yoonsoo', 88, 75]]
    
    my_df = pd.DataFrame(two_dimensional_list, columns=['name', 'english_score', 'math_score'], index=['a', 'b', 'c', 'd'])
    
    print(my_df.dtypes)
    ```

    > name             object
    > english_score     int64
    > math_score        int64
    > dtype: object

  * `dtypes`의 종류

    | dtype      | 설명            |
    | ---------- | --------------- |
    | int64      | 정수            |
    | float64    | 소수            |
    | object     | 텍스트          |
    | bool       | 불린(참과 거짓) |
    | detetime64 | 날짜와 시간     |
    | category   | 카테고리        |

* CSV

  * `Comma-separated values`

    * 값들이 쉼표로 나누어져 있음

  * pandas로 csv 파일 읽어들이기

    ```
    iphone_df = pd.read_csv('파일경로')
    ```

  * header가 없는 경우

    * 젤 처음의 행이 header가 됨

    * 이를 방지하기 위해

      ```
      iphone_df = pd.read_csv('파일경로' , header=None)
      ```

      * 0,1,2, ... 가 열이 됨

  * header에 값이 없고 쉼표만 있는 경우

    * 필드명이 `Unnamed:0`으로 나타남

    * csv파일에서는 특정 컬럼을 행으로 지정할 수 있음

      ```
      iphone_df = pd.read_csv('파일경로' , index_col=0)
      ```

      