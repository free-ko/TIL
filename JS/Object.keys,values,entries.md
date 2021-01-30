# Object.keys, values, entries

- 순회에 관한 다루어 보겠습니다.
- `map.keys()`, `map.values()`, `map.entries()`와 같은 메서드들에 대해 알아보았습니다.
- 이 메서드들은 포괄적인 용도로 만들어졌기 때문에 이 메서드들이 적용될 자료구조는 일련의 합의를 준수해야 합니다.
- 자료구조를 직접 만들어서 사용하려면 기존에 구현된 메서드를 쓰지 못하고 직접 커스텀 메서드를 구현해야 합니다.

- `keys()`, `values()`, `entries()`는 아래와 같은 자료구조에 적용할 수 있습니다.
  - Map
  - Set
  - Array

1. `Object.keys(obj)` – 키가 담긴 배열을 반환합니다.
2. `Object.values(obj)` – 값이 담긴 배열을 반환합니다.
3. `Object.entries(obj)` – [key, value] 쌍이 담긴 배열을 반환합니다.

<br>

## Map, Set, Array에 적용하는 메서드와 객체에 적용하는 메서드의 차이를 맵을 기준으로 비교하면 다음과 같습니다.

|           | 맵          | 객체                               |
| --------- | ----------- | ---------------------------------- |
| 호출 문법 | map.keys()  | obj.keys()가 아닌 Object.keys(obj) |
| 반환 값   | iterable 객 | ‘진짜’ 배열                        |

<br>

1. 첫 번째 차이는 obj.keys()가 아닌 Object.keys(obj)를 호출한다는 점입니다. 이렇게 문법에 차이가 있는 이유는 유연성 때문입니다.

   - 아시다시피 자바스크립트에선 복잡한 자료구조 전체가 객체에 기반합니다.
   - 그러다 보니 객체 data에 자체적으로 메서드 data.values()를 구현해 사용하는 경우가 있을 수 있습니다.
   - 이렇게 커스텀 메서드를 구현한 상태라도 Object.values(data)같이 다른 형태로 메서드를 호출할 수 있으면 커스텀 메서드와 내장 메서드 둘 다를 사용할 수 있습니다.

2. 두 번째 차이는 메서드 Object.\*를 호출하면 iterable 객체가 아닌 객체의 한 종류인 배열을 반환한다는 점입니다. ‘진짜’ 배열을 반환하는 이유는 하위 호환성 때문입니다.

```js
let user = {
  name: "John",
  age: 30,
};

// Object.keys(user) = ["name", "age"]
// Object.values(user) = ["John", 30]
// Object.entries(user) = [ ["name","John"], ["age",30] ]

let user = {
  name: "John",
  age: 30,
};

// 값을 순회합니다.
for (let value of Object.values(user)) {
  alert(value); // John, 30
}
```

<br>

### Object.keys, values, entries는 심볼형 프로퍼티를 무시합니다.

- for..in 반복문처럼, Object.keys, Object.values, Object.entries는 키가 심볼형인 프로퍼티를 무시합니다.
- 대개는 심볼형 키를 연산 대상에 포함하지 않는 게 좋지만, 심볼형 키가 필요한 경우엔 심볼형 키만 배열 형태로 반환해주는 메서드, Object.getOwnPropertySymbols를 사용하면 됩니다.
- getOwnPropertySymbols 이외에도 모든 키를 배열 형태로 반환하는 메서드, Reflect.ownKeys(obj)를 사용할 수 있습니다.

<br>

# 객체 변환하기

- 객체엔 map, filter 같은 배열 전용 메서드를 사용할 수 없습니다.
- 하지만 Object.entries와 Object.fromEntries를 순차적으로 적용하면 객체에도 배열 전용 메서드 사용할 수 있습니다.
- 적용 방법은 다음과 같습니다.
  1. Object.entries(obj)를 사용해 객체의 키-값 쌍을 요소로 갖는 배열을 얻습니다.
  2. 1.에서 만든 배열에 map 등의 배열 전용 메서드를 적용합니다.
  3. 2.에서 반환된 배열에 Object.fromEntries(array)를 적용해 배열을 다시 객체로 되돌립니다.
- 위 방법을 사용해 가격 정보가 저장된 객체 prices의 프로퍼티 값을 두 배로 늘려보도록 합시다.

```js
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // 객체를 배열로 변환해서 배열 전용 메서드인 map을 적용하고 fromEntries를 사용해 배열을 다시 객체로 되돌립니다.
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);

alert(doublePrices.meat); // 8
```

<br>

[출처]
https://ko.javascript.info/keys-values-entries#ref-253
