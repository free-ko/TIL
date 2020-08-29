# TIL

It's not whether you get knocked down; It's whether you get back up.

# React Study

- Components with JSX
- Props
- PropTypes
- Class Component and State
- ComponentLife Cycle

## 1. Components with JSX

```jsx
import React from "react";

// 정말로 Food Component에 값들을 전달 했는지 console.log(props)를 통해서 확인해 보면 됩니다.
// 그러면 object로 전달 된 것을 확인할 수 있습니다.
// 우리는 object에서 fav를 꺼내서 사용해 보겠습니다.
// function Food({ fav }) 는 props.fav를 의미 하며 이 값은 kimchi입니다.
// 그리고 return <h1>I like {fav}</h1>를 작성하게 되면
// 출력 값은 I like kimchi가 됩니다.
function Food(props) {
  console.log(props);
  return <h1>I like {}</h1>;
}

function App() {
  return (
    <div>
      <h1>Hello</h1>
      // Food Component에 fav라는 이름의 Property를 kimchi라는 value로 주었습니다.
      // 여러개의 value를 전달 할 수도 있습니다. // 누군가가 Food Component에 정보를
      보내려고 하면, React는 이 모든 속성을 가져 옵니다. // 그리고 Food Component의
      argument(인자)로 Property를 넣습니다.
      <Food fav="kimchi" something={true} seafood={["water", 2, 3, 4, true]} />
    </div>
  );
}
```

```jsx
import React from "react";

// 원래는 인자를 props가 되며
// props.fav를 작성해야 합니다.
// 그러나 비구조화 할당(객체에서 값을 추출하는 문법)을 통해 밑에 처럼 간단 하게 작성이 가능합니다.
// const { fav } = props 작성해 줌으로써
// 결국 props.fav를 작성 하지 않고 fav만 작성해서 값을 얻을 수 있도록 간단하게 표현 했습니다.

function Food({ fav }) {
  return <h1>I like {fav}</h1>;
}

function App() {
  return (
    <div>
      <h1>Hello</h1>
      <Food fav="kimchi" />
      <Food fav="kim" />
      <Food fav="kimbab" />
    </div>
  );
}

// 출력 값은
// I like kimchi
// I like kim
// I like kimbab이 나옵니다.
```
