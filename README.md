# TIL

It's not whether you get knocked down; It's whether you get back up.

# React Study

- Components with JSX
- Props
- PropTypes
- Class Component and State
- ComponentLife Cycle
  <br/>
  <br/>
  <br/>

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

<br/>
<br/>
<br/>

# 🌈 6장 Component 반복

- JS 배열의 Map() 함수
- Data 배열을 Component 배열로 변환하기
- Key 사용

```jsx
import React from "react";

const IterationSample = () => {
  return (
    <ul>
      <li>눈사람</li>
      <li>얼음</li>
      <li>눈</li>
      <li>바람</li>
    </ul>
  );
};

export default IterationSample;

// 지금은 li 태그 하나 뿐이라 그렇게 문제가 되지 않을 것 같습니다.
// 하지만 코드가 좀 더 복잡 하다면 어떨까요?
// 코드 양은 더더욱 늘어 날 것이며, 파일 용량도 쓸데 없이 증가하겠죠. 이는 낭비 입니다.
// 또 보여 주어야 할 데이터가 유동적이라면 이런 코드로는 절대로 관리하지 못합니다.
```

⭐️오늘은 Project에서 반복적인 내용을 효율적으로 보여주고 관리하는 방법을 알아 보겠습니다.⭐️
<br/>
<br/>
<br/>

## 1. JS의 Map() 함수

- JS 배열 객체의 내장 함수인 map 함수를 사용하면 반복되는 Component를 Rendering 할 수 있습니다.
- map 함수는 파라미터로 전달된 함수를 사용해서 배열 내 각 요소를 원하는 규칙에 따라 변환 한 후 그 결과로 새로운 배열을 생성합니다.

```jsx
Arr.map(callback, [thingArg])

// 이 함수의 파라미터는 다음과 같습니다.
// callback : 새로운 배열의 요소를 생성하는 함수로 파라미터는 다음 3가지 입니다.
// - currentValue : 현재 처리하고 있는 요소
// - index : 현재 처리하고 있는 요소의 index 값
// - array : 현재 처리 하고 있는 원본 배열
// thisArg(선택 항목) : callback 함수 내부에서 사용할 this Refernece

ex)

// map 함수를 사용하여 배열 [1,2,3,4,5]의 각 요소를 제곱해서 새로운 배열을 생성하겠습니다.

const number = [1,2,3,4,5];

const processed = number.map(function(num) {  // const processed = number.map(num => num * num);
	return num * num;
});

console.log(processed);
```

- 위에서 만들었던 IterationSample Component를 map() 함수를 통해 수정해 보겠습니다.

```jsx
import React from "react";

const IterationSample = () => {
  const name = ["눈사람", "얼음", "눈", "바람"];
  const nameList = name.map((name) => <li>{name}</li>); // <li></li> JSX 코드로 된 배열을 새로 생성한 후 nameList에 담습니다.
  return <ul>{nameList}</ul>;
};

export default IterationSample;

// 이렇게 작성후 App Componetn에서 IterationSapmle을 Rendering을 합니다.
// 그리고 웹 사이트에서 확인하면 원하는 대로 Rendering 되어 있습니다.
// 하지만 크롬 개발자 도구의 콘솔을 열어 보면 "key" prop이 없다는 경고 메시지를 표시 했습니다.
```

<br/>
<br/>
<br/>

## 2. Key

- React에서 key는 Component 배열을 Rendering 했을 때 어떤 원소에 변동이 있었는지 알아내려고 사용합니다.
- 예를 들어 유동적인 Data를 다룰 때는 원소를 새로 생성 할 수도, 제거 할 수도, 수정 할 수도 있습니다.
- key가 없을 때는 Virtual DOM을 비교하는 과정에서 List를 순차적으로 비교하면서 변화를 감지 합니다.
- 하지만 key가 있다면 이 값을 사용 하여 어떤 변화가 일어났는지 더욱 빠르게 알아 낼 수 있습니다.

```jsx
// key 값을 설정 할 때는 map 함수의 인자로 전달되는 함수 내부에서 Component props를 설정하듯이 설정하면 됩니다.
// key 값은 언제나 유일해야 합니다.
// 따라서 Data가 가진 고윳값을 key 값으로 설정해야 합니다.
// 밑에 예시를 예를 들면 게시판의 게시물을 Rendering한다면 게시물의 번호를 key 값으로 설정해야 합니다.

const articleList = articles.map(article => (
	<Article
		title = {article.title}
		write = {article.writer}
		key = {article.id}
	/>
);

// 하지만 앞서 만들었던 IterationSample Component는 고유 번호가 없습니다.
// 이때에는 map 함수에 전달되는 콜백 함수의 인수인 index 값을 사용 하면 됩니다.
```

```jsx
import React from "react";

const IterationSample = () => {
  const name = ["눈사람", "얼음", "눈", "바람"];
  const nameList = name.map((name, index) => <li key={index}>{name}</li>);
  return <ul>{nameList}</ul>;
};

export default IterationSample;

// 이렇게 key 값을 index로 설정을 하게되면 개발자 도구에서 더이상 "key" prop이 없다고 경고창이 뜨지 않습니다.
// 단 주의해야 할 부분은 고유한 값이 없을 때만 index 값을 key로 사용해야 합니다.
// index를 key로 사용하면 배열이 변경될 때 효율적으로 리 랜더링 하지 못합니다.
```

<br/>
<br/>
<br/>

## 3.응용

- 지금 까지 배운 개념을 응용하여 고정된 배열을 Rendering 하는 것이 아닌, 동적인 배열을 Rendering 하는 것을 구현해 보겠습니다.
- 그리고 index 값을 key로 사용하려면 리랜더링이 비 효율적이라고 배웠는데
- 이러한 상황에서 어떻게 고윳값을 만들 수 있는지도 알아보겠습니다.

```jsx
// 초기 상태 설정하기 ---> 데이터 추가 기능 구현하기 ---> 데이터 제거 기능 구현하기
```

### 1) 초기 상태 설정하기

- IterationSample Component에서 useState를 사용하여 상태(State)를 설정하겠습니다.
- 3가지 State를 사용하겠습니다. ( Data 배열, input, Data 배열에서 새로운 항목을 추가 할 때 사용할 고유 id )
- 이번에는 객체 형태로 이루어진 배열을 만들겠습니다. 해당 객체는 문자열과 고유 id 값이 있습니다.

```jsx
import React, { useState } from "react";

const IterationSample = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

  const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);
  return <ul>{nameList}</ul>;
};

export default IterationSample;
```

### 2) 데이터 추가 기능 구현하기

- ul 태그의 상단에 input과 button을 Rendering 하고, input의 상태를 관리해 보겠습니다.

```jsx
import React, { useState } from "react";

const IterationSample = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5);

  const onChange = (e) => setInputText(e.target.value);

  const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);
  return (
    <>
      <input value={inputText} onChange={onChange} />
      <button>추가</button>
      <ul>{nameList}</ul>;
    </>
  );
};

export default IterationSample;
```

- 그 다음에는 버튼이 클릭 했을 때 호출할 onClick 함수를 선언하여 버튼의 onClick 이벤트로 설정해 보겠습니다.
- onClick 함수 에서는 배열의 내장 함수 concat을 사용하여 새로운 항목을 추가한 배열을 만들고, setNames를 통해 상태를 업그레이드 해주겠습니다.

```jsx
import React, { useState } from "react";

const IterationSample = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5);

  const onChange = (e) => setInputText(e.target.value);

  const onClick = () => {
    const nextNames = names.concat({
      id: nextId, // nextId 값을 id로 설정하고
      text: inputText,
    });
    setNextId(nextId + 1); //nextId 값에 1을 더해 준다.
    setNames(nextNames); // names 값을 업데이트 한다
    setInputText(""); // inputText를 비운다
  };

  const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);
  return (
    <>
      <input value={inputText} onChange={onChange} />
      <button onClick={onClick}>추가</button>
      <ul>{nameList}</ul>;
    </>
  );
};

export default IterationSample;

// onClick 함수에서 새로운 항목을 추가 할 때 객체의 id 값은 nextId를 사용하도록 하고
// 클릭 될 때 마다 값이 1씩 올라가도록 구현 했습니다.
// 추가로 button이 클릭될 때 기존의 Input 내용을 비우는 것도 구현해 주었습니다.

// 배열에 새 항목을 추가 할 때 배열의 push 함수를 사용하지 않고 concat을 사용 했는데요
// push 함수는 기존 배열 자체를 변경 해 주는 반면
// concat은 새로운 배열을 만들어 준다는 차이가 있습니다.
// React에서 상태를 업데이트 할 때는 기존 상태를 그대로 두면서 새로운 값을 상태로 설정해야 합니다.
// 이를 불변성 유지라고 합니다.
// 불변성 유지를 해 주어야 나중에 React Component의 성능을 최적화 할 수 있습니다.
```

### 3) 데이터 제거 기능 구현하기

- 이번에는 각 항목을 더블 클릭 했을 때 해당 항목이 화면에서 사라지는 기능을 구현해 보겠습니다.
- 이번에도 마찬가지로 불변성을 유지하면서 업데이트해 주어야 합니다.
- 불변성을 유지 하면서 배열의 특정 항목을 지울 때는 배열의 내장 함수 Filter를 사용합니다.
- Filter 함수를 사용하면 배열에서 특정 조건을 만족하는 원소들만 쉽게 분류 할 수 있습니다.

```jsx
const numbers = [1, 2, 3, 4, 5];
const biggerThanTree = numbers.filter((number) => number > 3); // 결과: [4,5,6]

// Filter 함수의 인자에 분류하고 싶은 조건을 반환하는 함수를 넣어 주면 쉽게 분류 할 수 있습니다.
// 이 Filter 함수를 응용하여 특정 배열에서 특정 원소만 제외 시킬 수도 있습니다.
// 예를 들어 numbers 배열에서 3만 없애고 싶다면 다음과 같이 하면 됩니다.

const withoutTree = numbers.filter((number) => number !== 3); // 결과 : [1,2,4,5]
```

- 이제 Filter 함수를 사용하여 IterationSample Component의 항목 제거 기능을 구현해 봅시다.
- HTML 요소를 더블 클릭 할 때 사용하는 이벤트 이름은 onDoubleClick 입니다.
- onRemove라는 함수를 만들어서 각 li 요소에 이벤트 등록을 하겠습니다.

```jsx
import React, { useState } from "react";

const IterationSample = () => {
  const [names, setNames] = useState([
    { id: 1, text: "눈사람" },
    { id: 2, text: "얼음" },
    { id: 3, text: "눈" },
    { id: 4, text: "바람" },
  ]);
  const [inputText, setInputText] = useState("");
  const [nextId, setNextId] = useState(5);

  const onChange = (e) => setInputText(e.target.value);

  const onClick = () => {
    const nextNames = names.concat({
      id: nextId,
      text: inputText,
    });
    setNextId(nextId + 1);
    setNames(nextNames);
    setInputText("");
  };

  const onRemove = (id) => {
    const nextNames = names.filter((name) => name.id !== id);
    setNames(nextNames);
  };

  const nameList = names.map((name) => (
    <li key={name.id} onDubleClick={() => onRemove(name.id)}>
      {name.text}
    </li>
  ));
  return (
    <>
      <input value={inputText} onChange={onChange} />
      <button onClick={onClick}>추가</button>
      <ul>{nameList}</ul>;
    </>
  );
};
```

<br/>
<br/>
<br/>

## 4.정리

- 이번 시간에는 반복되는 Data를 Rendering 하는 방법을 배웠으며
- 이를 응용하여 유동적인 배열을 다루어 보았습니다.
- Component Array를 Rendering 할 때는 key 값을 설정에 항상 주의해야 합니다.
- 또 key 값은 언제나 유일해야 합니다.
- key 값이 중복된다면 Rendering 하는 과정에서 오류가 발생합니다.
- State 안에서 배열을 변형 할 때에는 배열에 직접 접근하여 수정하는 것이 아니라
- `concat, filter` 등의 배열 내장 함수를 사용하여 새로운 배열을 만든 후 이를 새로운 상태로 설정해 주어야 한다는 점을 명심해야 합니다.
