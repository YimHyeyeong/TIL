## TypeScript

+ TypeScript

  1. node.js 설치
  2. VScode 오픈
  3. `npm istall -g typescript`
  4. tsconfig.json 파일에 ts -> js 컴파일시 옵션설정가능

+ 브라우저는 ts  파일을 인식하지 못함 그래서 js로 변환해야 사용가능

  + tsc -w를 터미널에 입력하면 자동으로 ts 파일을 js 파일로 변환해줌 => '컴파일한다' 라고함

+ 변수 타입지정가능

  ```
  let 이름 :string = 'kim';
  ```

  + "이 변수엔 `string type` 만 들어올 수 있습니다"
  + 타입을 엄격하게 관리
  + `string`, `number`, `boolean`, `null`, `undefined`, `bigint`, `[]`, `{}` 등 사용가능

  ```
  # array
  let 이름 :string[] = ['kim', 'park']
  ```

  + "이 변수엔 `string` 담긴 `array` 만 들어올 수 있습니다"
  + array로 지정해주고 싶다면 `[]`
  + array 안의 구성 물품이 무엇인지 지정하려면 `[]` 앞에서 지정해주면 됨

  ```
  # object
  let 이름 :{name? : string} = {name:'kim'}
  ```

  + ? : name 속성은 옵션이라는 뜻, name이 들어와도 되고 안들어와도 되는 것

  ```
  # Union type
  let 이름 :string | number = 'kim';
  ```

  + `string` 혹은 `number` 가 들어올 수 있다

  ```
  # Type alias
  type Name = string | number;
  let 이름 :Name = 123;
  ```

  + 타입지정하는 것이 길다면 타입지정을 변수에 담아도 됨
  + 타입 지정할 때 변수명은 보통 대문자로 시작(차별점을 주기 위해)

  ```
  # 함수에 타입지정 가능
  function 함수(x :number) :number {
  	return x * 2
  }
  ```

  + 처음 `number` 은 파라미터의 타입을 지정
  + 두 번째 `number`은 리턴 값의 타입을 지정

  ```
  # array에 쓸 수 있는 tuple 타입
  type Member = [number, boolean];
  let john:Member = [123, true]
  ```

  + `tuple` 타입
  + 무조건 첫 번째 자료는 `number`, 두 번째는 `boolean`

  ```
  # object에 타입지정해야 할 속성이 너무 많으면
  type Member = {
  	[key: string] :string,
  }
  let john :Member = {name:'kim'}
  ```

  + `[key:string]` :  모든 object 속성 = 글자로 된 모든 object 속성의 타입은 : string

  ```
  # class
  class User {
  	name; string;
  	constructor(name:string){
  		this.name = name;
  	}
  }
  ```

  + 변수를 미리 만들어서 타입을 지정