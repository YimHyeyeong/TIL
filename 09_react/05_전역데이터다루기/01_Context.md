## Context

+ Prop Drilling
  + 드릴로 땅을 파듯이 상위 컴포넌트에서 하우 컴포넌트로 반복해서 `Prop`을 내려주는 상황
+ React Context
  + =맥락 / 상황에 대한 정보
  + 많은 컴포넌트에서 사용하는 데이터를 반복적인 Prop전달(Prop Drilling) 없이 공유
+ Context.Provider
  + 최상위 태그로 내용을 감싸면 그 안의 컴포넌트는 context가 제공하는 데이터는 자유롭게 사용가능
  + value prop으로 공유할 데이터를 지정해주면 됨
  + Provider는 태그지만 아무것도 렌더링되지 않음
+ useContext
  + 원하는 컴포넌트에서 useContext를 사용해 데이터를 가져올 수 있음







## 상태관리의 역사

+ 상태관리 (State Management)
  + 화면에서 사용하는 데이터를 관리하는 것