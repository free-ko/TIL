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

<br/>
<br/>
<br/>
<br/>

# 🌈 8장 Hooks

- useState
- useEffect
- useReducer
- useMemo
- useCallback
- useRef
- 커스텀 Hooks 만들기
- 다른 Hooks

```jsx
// Hooks은 React v16.8에 새로 도입된 기능입니다.
// Function Component에서도 State(상태)관리를 할 수 있는 useState,
// Rendering 직후 작업을 설정하는 useEffect 등의 기능을 제공하여
// Function Component에서 할 수 없었던 다양한 작업을 할 수 있게 해줍니다.
```

<br/>
<br/>

## 1. useState

---

- 가장 기본적인 Hook이며, Function Component에서도 가변적인 상태를 지닐 수 있게 해줍니다.
- 만약 Function Component에서 State를 관리해야 한다면 useState를 사용하면 됩니다.

```jsx
<Counter.js>

import React, { useState } from "react";

const Counter = () => {

// useState 함수의 파라미터에는 상태의 기본 값을 넣어 줍니다. 현재 0을 넣어 주었는데
// 결국 카운터의 기본 값을 0으로 설정하겠다는 의미 입니다.
// useState가 호출되면 배열을 리턴 합니다.
// 그 배열의 첫 번째 원소는 상태 값, 두 번째 원소는 상태를 설정하는 함수 입니다.
// setValue에 파라미터를 넣어서 호출하면 전달 받은 파라미터로 값이 바뀌고 컴포넌트가 정상적으로 리레런딩 됩니다.
  const [value, setValue] = useState(0);

  return (
    <div>
      <p>
        현재 카운터 값은 <b>{value}</b>입니다.
      </p>
      <button onClick={() => setValue(value + 1)}>+1</button>
      <button onClick={() => setValue(value - 1)}>-1</button>
    </div>
  );
};
```

<br/>
<br/>
- 하나의 useState 함수는 하나의 상태 값만 관리 할 수 있습니다.
- Component에서 관리해야 할 상태가 여러 개라면 useState를 여러 번 사용하면 됩니다.

```jsx
<Info.js파일>

import React, { useState } from "react";

const Info = () => {
  const [name, setName] = useState("");
  const [nickname, setNickname] = useState("");

  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNickname = (e) => {
    setNickname(e.target.value);
  };

  return (
    <>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickname} />
      </div>
      <div>
        <div>
          <b>이름 : </b> {name}
        </div>
        <div>
          <b>닉네임 : </b> {nickname}
        </div>
      </div>
    </>
  );
};

export default Info;
```

<br/>
<br/>

## 2. useEffect

---

- React Component가 Rendering될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook 입니다.
- Class Component의 componentDidMount + componentDidUpdate = useEffect 합친 형태 입니다.

```jsx
<Info.js파일>

import React, { useState, useEffect } from "react";

const Info = () => {
  const [name, setName] = useState("");
  const [nickname, setNickname] = useState("");

  useEffect(() => {
    console.log("랜더링이 완료 되었습니다.");
    console.log({
      name,
      nickname,
    });
  });

  const onChangeName = (e) => {
    setName(e.target.value);
  };

  const onChangeNickname = (e) => {
    setNickname(e.target.value);
  };

  return (
    <>
      <div>
        <input value={name} onChange={onChangeName} />
        <input value={nickname} onChange={onChangeNickname} />
      </div>
      <div>
        <div>
          <b>이름 : </b> {name}
        </div>
        <div>
          <b>닉네임 : </b> {nickname}
        </div>
      </div>
    </>
  );
};

export default Info;
```

<br/>
<br/>

### 1) Mount 될 때만 실행하고 싶을 때

- useEffect에서 설정한 함수를 Component가 화면에 맨 처음 Rendering 때만 실행하고 업데이트 될 때는 실행하지 않으려면 함수의 두번 째 파라미터로 비어 있는 배열을 넣어 주면 됩니다.

```jsx
useEffect(() => {
	console.log('마운트 될 때만 실행됩니다.');
}, **[]**) // 여기 두번 째 파라미터에 빈 배열을 넣어 주면 됩니다.
```

<br/>
<br/>

### 2) 특정 값이 업데이트 될 때만 실행하고 싶을 때

```jsx
// Class Component

componentDidUpdate(prevProps, preState) {
	if (preProps.value !== this.props.value) {
		doSomething();
	}
}

// 이 코드는 props 안에 들어 있는 value 값이 바뀔 때만 특정 작업을 수행합니다.
```

```jsx
// Function Component
// useEffect의 두번째 파라미터로 전달 되는 배열 안에 검사하고 싶은 값을 넣어 주면 됩니다.

useEffect(() => {
  console.log(name);
}, [name]);
```

<br/>
<br/>
### 3) 뒷 정리하기

- useEffect는 기본적으로 Rendering 되고 난 직후 마다 실행 되며, 두 번째 파라미터 배열에 무엇을 넣는지 에 따라 실행되는 조건이 달라집니다.
- Component가 Unmount되기 전이나 Update 되기 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리(cleanup) 함수를 반환(Return)해 주어야 합니다.

```jsx
<Info.js파일>

useEffect(() => {
	console.log('effect');
	console.log(name);
	return () => {
		console.log('cleanup');
		console.log(name):
	}
})

<App.js파일>

import React, { useState } from "react";
import Info from "./info";

function App() {
  const [visible, setVisible] = useState(false);
  return (
    <div>
      <button
        onClick={() => {
          setVisible(!visible);
        }}
      >
        {visible ? "숨기기" : "보이기"}
      </button>
      <hr />
      {visible && <Info />}
    </div>
  );
}

export default App;

// Component 나타날 때 콘솔에 effect가 나타나고, 사라질 때 cleanup이 나타납니다.
// 그 다음에 input에 이름을 적어 보고 콘솔을 확인해 보겠습니다.
// Rendering 될 때마다 뒷 정리 함수가 계속 나타나는 것을 확인 할 수 있습니다.
// 그리고 뒷 정리 함수가 호출 될 때는 업데이트 되기 직전의 값을 보여 줍니다.

// 오직 Unmount 될 때만 뒷정리 함수를 호출되고 싶다면 useEffect 함수의 두번 째 파라미터에 비어 있는 배열을 넣으면 됩니다.
useEffect(() => {
	console.log('effect');
	console.log(name);
	return () => {
		console.log('cleanup');
		console.log(name);
	};
}, [name]);

```

<br/>
<br/>

## 3. useReducer

---

- useState 보다 더 다양한 Component 상황에 따라 다양한 상태를 다른 값으로 Update 해주고 싶을 때 사용하는 Hook 입니다.
- 현재 상태 그리고 업데이트를 위해 필요한 정보를 담은 액션 값을 전달 받아 새로운 상태를 반환하는 함수 입니다.
- Reducer 함수는 새로운 상태를 만들 때는 반드시 불변성(기존의 값을 유지)을 지켜야 합니다.

```jsx
function reducer(state, action) {
	return {...}  // 불변성을 지키면서 업데이트한 새로운 상태를 반환합니다.
}

// 액션 값은 주로 다음과 같은 형태로 이루어져 있습니다.

{
	type: 'INCREMENT',
	// 다른 값들이 필요하다면 추가로 들어 갑니다.
}

// Redux에서 사용하는 action 객체에는 어떤 action인지 알려주는 type 필드가 꼭 있어야 하지만
// useReducer에서 사용하는 action 객체는 반드시 type을 지니고 있을 필요가 없습니다.
// 심지어 객체가 아니라 문자열이나 숫자여도 상관 없습니다.
```

<br/>
<br/>

### 1) 카운터 구현하기

```jsx
<Counter.js파일>

import React, { useReducer } from "react";

function reducer(state, action) {
    // action.type에 따라 다른 작업 수행
    switch (action.type) {
        case 'INCREMENT':
            return {value: state.valuse + 1};
        case 'DECREMENT':
            return {valuse: state.value -1};
        default:
            // 아무것도 해당되지 않을 때 기존 상태 반환
            return state;
    }
}

const Counter = () => {

// useReducer의 첫 번째 파라미터에는 reducer함수를 넣고
// 두 번째 파라미터에는 해당 reducer의 기본 값을 넣어 줍니다.
// 이 Hook을 사용하면 state 값과 dispatch 함수를 받아 오는데요
// 여기서 state는 현재 가리키고 있는 상태이고, dispatch는 action을 발생시키는 함수 입니다.
// dispatch(action)과 같은 형태로, 함수 안에 파라미터로 action 값을 넣어 주면 reducer 함수가 호출 되는 구조 입니다.
// useReducer를 사용 했을 때의 가장 큰 장점은 Component Update Logic을 Component 밖으로 빼낼 수 있다는 것 입니다.
	const [state, dispatch] = useReducer(reducer, {value:0})

  return (
    <div>
        <p>
            현재 카운터 값은 <b>{state.value</b>입니다.
        </p>
        <button onClick = {() => dispatch({type: 'INCREMENT'})}>+1</button>
        <button onClick = {() => dispatch({type: 'DECREMENT'})}>-1</button>
    </div>
};

export default Counter;
```

<br/>
<br/>

### 2) Input 상태 관리하기

- 이번에는 useReducer를 사용하여 Info Component에 input 상태를 관리해 보겠습니다.
- 기존에는 input이 여러 개여서 useState를 여러 번 사용 했지만
- useReducer를 사용하면 기존에 Class Copomponent에서 input 태그에 name 값을 할당하고 e.target.name을 참조하여 setState를 해준 것과 유사한 방식으로 처리 할 수 있습니다.

```jsx
<Info.js파일>

import React, { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, {
    name: "",
    nickname: "",
  });

  const { name, nickname } = state;

  const onChange = (e) => {
    dispatch(e.target);
  };

  return (
    <>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름 : </b> {name}
        </div>
        <div>
          <b>닉네임 : </b> {nickname}
        </div>
      </div>
    </>
  );
};

export default Info;
```

- useReducer에서의 action은 그 어떤 값도 사용 가능 합니다.
- 그래서 이번에는 객체가 지니고 있는 e.target 값 자체를 action 값으로 사용 했습니다.
- 이런 식으로 input을 관리하면 아무리 input의 개수가 많아 져도 코드를 짧고 깔끔하게 유지 할 수 있습니다.
  <br/>
  <br/>

## 4. useMemo

---

- useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 최적화 할 수 있습니다.

```jsx
<Average.js파일>

import React, { useState } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중..");
  if (numbers.length === 0) return 0;

  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (e) => {
    setNumber(e.target.value);
  };

  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {getAverage(list)}
      </div>
    </div>
  );
};

export default Average;

// 그런데 숫자를 등록 할 때뿐만 아니라 input 내용이 수정 될 때도 우리가 만든 getAverage 함수가 호출 되는 것을 확인 할 수 있습니다.
// input 내용이 바귈 때는 평균 값을 다시 계산할 필요가 없습니다.
// useMemo Hook을 사용하면 이러한 작업을 최적화 할 수 있습니다.
// 랜더링 하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, 원하는 값이 바뀌지 않았다면
// 이전에 연산했던 결과를 다시 사용하는 방식입니다.
```

- useMemo사용

```jsx
<Average.js파일>

import React, { useState, useMemo } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중..");
  if (numbers.length === 0) return 0;

  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = (e) => {
    setNumber(e.target.value);
  };

  const onInsert = (e) => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;

// 이제 list 배열의 내용이 바뀔 때만 getAverage 함수가 호출 됩니다.
```

<br/>
<br/>

## 5. useCallback

---

- useCallback은 useMemo와 상당히 비슷한 함수 입니다.
- 주로 랜더링 성능을 최적화 해야 하는 상황에서 사용합니다.
- 이 Hook을 사용하면 이벤트 핸들러 함수를 필요할 때만 생성 할 수 있습니다.
- 위의 코드를 보면 onChange와 onInsert 라는 함수를 선언 해 주었지요?
- 이렇게 선언하면 Component가 리랜더링 될 때마다 이 함수들이 새로 생성 됩니다.
- 대부분 이러한 방식은 문제 없지만, Component가 랜더링이 자주 발생하거나 랜더링 해야 할 Component의 개수가 많아지면 이 부분을 최적화해 주는 것이 좋습니다.

```jsx
<Average.js파일>

import React, { useState, useMemo, useCallback } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중..");
  if (numbers.length === 0) return 0;

  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");

  const onChange = useCallback((e) => {
    setNumber(e.target.value);
  }, []); // Component가 처음 랜더링 될 때만 함수 생성

  const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
  }, [number, list]);  // number 혹은 list가 바뀌 었을 때만 함수 생성

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;

// useCallback의 첫 번째 파라미터에는 생성하고 싶은 함수를 넣고,
// 두 번째 파라미터에는 배열을 넣으면 됩니다.
// 이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시해야 합니다.
// onChange처럼 비어 있는 배열을 넣게 되면 컴포넌트가 랜더링 될 때 단 한번만 함수가 생성되며,
// onInsert처럼 배열 안에 number와 list를 넣게 되면 input 내용이 바뀌거나 새로운 항목이 추가 될 때마다 함수가 생성됩니다.
// 함수 내부에서 상태 값에 의존해야 할 때는 그 값을 반드시 두 번째 파라미터 안에 포함시켜 주어야 합니다.
// 예를 들어 conChange의 경우 기존의 값을 조회하지 않고 바로 설정만 하기 때문에 배열이 비어 있어도 상관 없지만
// onInsert는 기존의 number와 list를 조회해서 nextList를 생성하기 때문에 배열 안에 number와 list를 꼭 넣어 주어야 합니다.

// 참고로 다음 두 코드는 완전히 똑같은 코드 입니다.
// useCallback은 결국 useMemo로 함수를 반환하는 상황에서 더 편하게 사용할 수 있는 Hook입니다.
// 숫자, 문자열, 객체처럼 일반 값을 재사용하려면 useMemo를 사용하고,
// 함수를 재 사용 하려면 useCallback을 사용하세요

useCallback(() => {
	console.log('hello world');
},[])

useMemo(() => {
	const fn = () => {
		console.log('hello world');
	};
	return fn;
}, [])
```

<br/>
<br/>

## 6. useRef

---

- useRef Hook은 함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해줍니다.

```jsx
<Average.js>

import React, { useState, useMemo, useCallback, useRef } from "react";

const getAverage = (numbers) => {
  console.log("평균값 계산 중..");
  if (numbers.length === 0) return 0;

  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState("");
  const inputEl = useRef(null);

  const onChange = useCallback((e) => {
    setNumber(e.target.value);
  }, []); // Component가 처음 랜더링 될 때만 함수 생성

  const onInsert = useCallback(() => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber("");
    inputEl.current.focus();
  }, [number, list]); // number 혹은 list가 바뀌 었을 때만 함수 생성

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={onChange} ref={inputEl} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;

// useRef를 사용하여 ref를 설정하면 useRef를 통해 만든 객체 안의 current 값이 실제 element를 가리킵니다.
```

<br/>
<br/>

### 1) 로컬 변수 사용하기

- 추가로 컴포넌트 로컬 변수를 사용해야 할 때도 useRef를 활용 할 수 있습니다.
- 여기서 로컬 변수랑 랜더링과 상관없이 바뀔 수 있는 값을 의미 합니다.

```jsx
// 클래스 형태로 작성된 컴포넌트의 경우에는 로컬 변수를 사용해야 할 때 밑에 처럼 작성 가능합니다.

import React, { Component } from "react";

class MyComponent extends Component {
  id = 1;
  setId = (n) => {
    this.id = n;
  };
  printId = () => {
    console.log(this.id);
  };
  render() {
    return <div>MyComponent</div>;
  }
}

export default MyComponent;
```

```jsx
// 위의 코드를 함수형 코드로 변환해 보겠습니다.

import React, { useRef } from "react";

const RefSample = () => {
  const id = useRef(1);
  const setId = (n) => {
    id.current = n;
  };
  const printId = () => {
    console.log(id.current);
  };
  return <div>refsample</div>;
};

export default RefSample;

// 이렇게 ref 안의 값이 바뀌어도 컴포넌트가 렌더링되지 않는다는 점에는 주의해야 합니다.
// 렌더링과 관련되지 않은 값을 관리할 때만 이러한 방식으로 코드를 작성합니다.
```

<br/>
<br/>

## 7. 커스텀 Hooks 만들기

---

- 여러 컴포넌트에서 비슷한 기능을 공유할 경우, 이를 우리만의 Hook으로 작성하여 로직을 재사용 할 수 있습니다.
- 기존에 Info 컴포넌트에서 여러 개의 input을 관리하기 위해 useReducer로 작성했던 로직을
- useInputs라는 Hook으로 따로 분리해 보겠습니다.

```jsx
<useInfo.js파일>

import { useReducer } from "react";

function reducer(state, action) {
  return {
    ...state,
    [action.naem]: action.value,
  };
}

export default function useInputs(initialForm) {
  const [state, dispatch] = usereducer(reducer, initialForm);
  const onChange = (e) => {
    dispatch(e.target);
  };
  return [state, onChange];
}

<Info.js파일>

import React, { useReducer } from "react";
import useInputs from "./useInput";

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  };
}

const Info = () => {
  const [state, onChange] = useInputs({
    name: "",
    nickname: "",
  });

  const [name, nickname] = state;

  const onChange = (e) => {
    dispatch(e.target);
  };

  return (
    <>
      <div>
        <input name="name" value={name} onChange={onChange} />
        <input name="nickname" value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름 : </b> {name}
        </div>
        <div>
          <b>닉네임 : </b> {nickname}
        </div>
      </div>
    </>
  );
};

export default Info;
```

<br/>
<br/>

## 8.정리

---

- React에서 Hooks 패턴을 사용하면 클래스형 컴포넌트를 작성하지 않고도 대부분의 기능을 구현 할 수 있습니다.
- 이러한 기능이 React에 릴리즈 되었다고 해서 기존의 setState를 사용하는 방식이 잘못된 것은 아닙니다.
- 물론 useState 혹은 useReducer를 통해 구현할 수 있더라도 말입니다.
- React 메뉴얼에 따르면, 기존의 클래스형 컴포넌트는 앞으로도 계속해서 지원될 예정입니다.
- 그렇기 때문에 만약 유지 보수하고 있는 프로젝트에서 클래스형 컴포넌트를 사용하고 있다면, 이를 굳이 함수형 컴포넌트와 Hooks를 사용하는 형태로 전환할 필요는 없습니다.
- 다만 메뉴얼에서는 새로 작성하는 컴포넌트의 경우 함수형 컴포넌트와 Hooks를 사용할 것을 권장하고 있습니다.
- 앞으로 프로젝트를 개발할 때는 함수형 컴포넌트의 사용을 첫 번째 옵션으로 두고, 꼭 필요한 상황에서만 클래스형 컴포넌트를 구현하면 좋습니다.
