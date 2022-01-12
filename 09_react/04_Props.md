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

    