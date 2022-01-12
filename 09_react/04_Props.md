## Props

* Props

  * 컴포넌트에 지정한 속성들
  * 각각의 속성은 `Prop` 이라고 부름
  * 컴포넌트 태그에 지정해준 속성은 *하나의 객체 형태* 로 컴포넌트 함수의 **첫 번째 파라미터**로 전달된다.

  ```
  <Dice color="blue"/>
  ```

  ```
  function Dice(props) {
  	const diceImg = props.color === 'red' ? diceRed01 : diceBlue01;
  	return <img src={diceImg} alt="주사위" />;
  }
  ```

  * props 파라미타를 계속 쓰는 것이 지저분하다 => **구조 분해 할당(destructuring)** 사용 

    ```
    function Dice({color="blue",num="1"}) {
      const src = DICE_IMAGES[color][num - 1];
      const alt = `${color} ${num}`;
      return <img src={src} alt={alt} />;
    }
    ```

  * 컴포넌트 태그에 prop을 숫자로 주려면 `{}` 중괄호를 사용해야 함

    ```
    <Dice color="red" num={2} />
    ```






## children

* children

  * component의 자식들을 props로 받는것 
  * 바로 보여질 값을 다룰 때 prop을 만드는 것 보다 *children* 을 만드는 것이 직관적으로 코드를 볼 수 있음
  * 컴포넌트 안에 컴포넌트를 작성할 수도 있고 컴포넌트 안에 복잡한 태ㅡ들을 더 작성할 수 있음
  
  ```
  # 하위 컴포넌트
  
  function Button({text}) {
    return <button>{text}</button>;
  }
  
  ⬇
  
  # children
  
  function Button({children}) {
    return <button>{children}</button>;
  }
  ```
  
  ```
  # 상위 컴포넌트
  
  <div>
  	<Button text="던지기">
  </div>
  
  ⬇
  
  # children
  
  <div>
      <Button>던지기</Button>
  </div>
  ```
  
  