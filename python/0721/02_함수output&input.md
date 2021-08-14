## 함수 output



+ 함수의 리턴(return)

  + 함수는 항상 반환되는 값이 있으며 어떠한 객체라도 상관없음

  + 오직 한 개의 객체만 `return` 됨

  + 복수의 객체를 return 하는 경우

    ```
    def foo(a, b):
     return a+b, a-b
    
    foo(1, 2)
    ```

    (3, -1)

    > 하나의 객체(tuple)을 반환

  + 명시적인 return 값이 없는 경우

    ```
    def greeting():
    	print('hi')
    
    greeting()
    ```

    hi

    > Nonetype - 명시적인 리턴이 없어서 none 나옴. 하지만 하나의 객체를 반환한 거임

+ return vs print

  + `return `은 함수 안에서만 사용되는 **키워드**
  + `print` 는 출력을 위해 사용되는 **함수**
  + REPL(주피터노트북 같은 거)환경에서는 마지막에 작성된 코드의 리턴 값을 보여주므로 같은 동작을 하는 것으로 착각할 수 있음

+ 함수의 실습문제 - 사각형 넓이

  + 너비와 높이를 입력 받아 사각형의 넓이와 둘레를 튜플로 반환하는 함수 rectangle을 작성하시오

    ```
    def rectangle(width, height):
    	area = width * height
    	perimeter = 2 * (width + height)
    	return area,perimeter
    print(rectangle(30,20))
    ```





---

## 함수 input



> parameter(매개변수): 함수에 입력으로 전달된 값을 받는 변수

```
def my_func(a, b):
```

> argument(인자,인수): 함수를 호출할 때 함수에 전달하는 입력값

```
my_func(1, 2)
```



+ 위치 인자(Positional Arguments) 

  + 기본적으로 함수 호출 시 인자는 위치에 따라 함수 내에 전달됨

  + 위치는 소듕하게 무조건 1번

    ```
    def add(x,y)
    ```

+ 기본 인자 값

  + 기본값을 지정하여 함수 호출 시 인자 값을 설정하지 않도록 함

    + 정의 된 것 보다 더 적은 개수의 인자들로 호출 될 수 있음

  + 위치인자 -> 기본인자 -> 가변

    ```
    def add(x, y=0)
    ```

+ 키워드 인자

  + 직접 변수의 이름으로 특정 인자를 전달할 수 있음

  + 키워드 인자 다음에 위치 인자를 사용할 수 없음

    ```
    def add(x,y)
    
    add(x=2,y=5)
    ```

+ 가변 인자 리스트

  + 함수가 임의의 개수 인자로 호출될 수 있도록 지정

  + 인자들은 튜플로 묶여 처리되며 매개변수에 *을 붙여 표현

  + 튜플 형태로 인자를 받음

  + 최대한 우측사이드에 위치하도록 해야 함. 마지막에 위치하도록

  + ```
    def add(*args):    args: 이름 바꿔도 되지만 관습적으로 이거씀 그냥 매개변수이름임
    	for arg in args(이거 튜플임):
    		print(arg)
    		
    add(2)
    add(2,3,4,5) - 이려면 ((2,3), (4,5)) 튜플로 묶임
    ```

+ 가변 키워드 인자

  + 함수가 임의의 개수 인자를 키워드 인자로 호출될 수 있도록 지정

  + 인자들은 딕셔너리로 묶여 처리되며 매개변수에 **를 붙여 표현

  + 키워드 인자 갯수를 모를 때 사용

    ```
    def family(**kwargs):
    	for key, value in kwargs:
    	print(key, ":", value)
    	
    family(father = 'john', mother = 'jane')
    ```

+ 함수 정의 주의사함

  + 기본 인자 값을 가지는 인자 다음에 기본 값이 없는 인자로 정의할 수 없음
    + `SytaxError: non- default argument follows default argument`
    + 예를 들어 greeting(40)하면 첫 번째 위치인자로 들어가버림 하지만 매칭이 안됨.

+ 함수 호출 주의 사함

  + 키워드 인자 다음에  위치 인자를 활용할  수 없음

  + 가변 인자리스트가 위치 인자보다 앞쪽에 올 수 없음

    ```
    add(*args, x)
     add(1,2,3)
    ```

    > 어디까지가 가변인자인지 모름 x를 찾지 못함 가변인자는 뒤로 가야함
    >
    > 가변 키워드 인자가 위치 인자보다 앞쪽에 올 수 없음 애초에 선언이 안됨

