## pure redux

+ type 뒤의 객체로 reducer에 보내고 싶은 데이터를 넣어보냄

+ 절대로 MUTATE DATA 쓰면 안됨

+ 리덕스의 3가지 규칙

  1. Single source of truth

  2. State is read-only

     + store를 수정할 수 있는 유일한 방법은 action을 보내는 것뿐

  3. Changes are made with pure functions

     + mutating state 대신에 new state objects를 리턴

     ```
     # mutation
     
     const friends = ["dal"]
     friends.push("lynn")
     ```

     > ... spread 문법을 사용해서 이전의 내용과 함께 새로운 objects를 리턴

     + splice()도 mutation, filter()를 사용