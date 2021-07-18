# Proxy와 Reflect

- `Proxy`는 특정 객체를 감싸 프로퍼티 읽기, 쓰기와 같은 객체에 가해지는 작업을 중간에서 가로채는 객체로, 가로채진 작업은 `Proxy` 자체에서 처리되기도 하고, 원래 객체가 처리하도록 그대로 전달되기도 합니다.
- 프락시는 다양한 라이브러리와 몇몇 브라우저 프레임워크에서 사용되고 있습니다.

<br>

## Proxy

```js
let proxy = new Proxy(target, handler);
```

- `target` : 감싸게 될 객체로, 함수를 포함한 모든 객체가 가능합니다.
- `handler` : 동작을 가로채는 메서드인 `'트랩(trap)'`이 담긴 객체로, 여기서 프락시를 설정합니다(예시: `get` 트랩은 `target`의 프로퍼티를 읽을 때, `set` 트랩은 `target`의 프로퍼티를 쓸 때 활성화됨).
- `proxy`에 작업이 가해지고, `handler`에 작업과 상응하는 트랩이 있으면 트랩이 실행되어 프락시가 이 작업을 처리할 기회를 얻게 됩니다.
- 트랩이 없으면 `target`에 작업이 직접 수행됩니다.
- 먼저, 트랩이 없는 프락시를 사용한 예시를 살펴봅시다.

```js
let target = {};
let proxy = new Proxy(target, {}); // 빈 핸들러

proxy.test = 5; // 프락시에 값을 씁니다. -- (1)
alert(target.test); // 5, target에 새로운 프로퍼티가 생겼네요!

alert(proxy.test); // 5, 프락시를 사용해 값을 읽을 수도 있습니다. -- (2)

for (let key in proxy) alert(key); // test, 반복도 잘 동작합니다. -- (3)
```

- 위 예시의 프락시엔 트랩이 없기 때문에 `proxy`에 가해지는 모든 작업은 `target`에 전달됩니다.

1. `proxy.test=`를 이용해 값을 쓰면 `target`에 새로운 값이 설정됩니다.
2. `proxy.test`를 이용해 값을 읽으면 `target`에서 값을 읽어옵니다.
3. `proxy`를 대상으로 반복 작업을 하면 `target`에 저장된 값이 반환됩니다.

- 트랩이 없으면 `proxy`는 `target`을 둘러싸는 투명한 래퍼가 됩니다.
- `Proxy`는 일반 객체와는 다른 행동 양상을 보이는 `'특수 객체(exotic object)'`입니다.
- 프로퍼티가 없죠. `handler`가 비어있으면 `Proxy`에 가해지는 작업은 `target`에 곧바로 전달됩니다.
- 자 이제 트랩을 추가해 Proxy의 기능을 활성화해봅시다.
- 그 전에 먼저, 트랩을 사용해 가로챌 수 있는 작업은 무엇이 있는지 알아봅시다.
- 객체에 어떤 작업을 할 땐 자바스크립트 명세서에 정의된 `'내부 메서드(internal method)'`가 깊숙한 곳에서 관여합니다.
- 프로퍼티를 읽을 땐 `[[Get]]`이라는 내부 메서드가, 프로퍼티에 쓸 땐 `[[Set]]`이라는 내부 메서드가 관여하게 되죠.
- 이런 내부 메서드들은 명세서에만 정의된 메서드이기 때문에 개발자가 코드를 사용해 호출할 순 없습니다.
- 프락시의 트랩은 내부 메서드의 호출을 가로챕니다. 프락시가 가로채는 내부 메서드 리스트는 명세서에서 확인할 수 있습니다.
- 모든 내부 메서드엔 대응하는 트랩이 있습니다.
- `new Proxy`의 `handler`에 매개변수로 추가할 수 있는 메서드 이름은 아래 표의 ‘핸들러 메서드’ 열에서 확인하실 수 있습니다.

<br>

### 규칙

- 내부 메서드나 트랩을 쓸 땐 자바스크립트에서 정한 몇 가지 규칙(invariant)을 반드시 따라야 합니다.
- 대부분의 규칙은 반환 값과 관련되어있습니다.
- 값을 쓰는 게 성공적으로 처리되었으면 `[[Set]]`은 반드시 `true`를 반환해야 합니다. 그렇지 않은 경우는 `false`를 반환해야 합니다.
- 값을 지우는 게 성공적으로 처리되었으면 `[[Delete]]`는 반드시 `true`를 반환해야 합니다. 그렇지 않은 경우는 `false`를 반환해야 합니다.
- 프락시 객체를 대상으로 `[[GetPrototypeOf]]`가 적용되면 프락시 객체의 타깃 객체에 `[[GetPrototypeOf]]`를 적용한 것과 동일한 값이 반환되어야 합니다.
- 프락시의 프로토타입을 읽는 것은 타깃 객체의 프로토타입을 읽는 것과 동일해야 하죠.
- 트랩이 연산을 가로챌 땐 위에서 언급한 규칙을 따라야 합니다.
- 이와 같은 규칙은 자바스크립트가 일관된 동작을 하고 잘못된 동작이 있으면 이를 고쳐주는 역할을 합니다. 규칙 목록은 명세서에서 확인할 수 있습니다. 아주 이상한 짓을 하지 않는한 이 규칙을 어길 일은 거의 없을겁니다.

<br>

## get 트랩으로 프로퍼티 기본값 설정하기

- 가장 흔히 볼 수 있는 트랩은 프로퍼티를 읽거나 쓸 때 사용되는 트랩입니다.
- 프로퍼티 읽기를 가로채려면 `handler`에 `get(target, property, receiver)` 메서드가 있어야 합니다.
- `get`메서드는 프로퍼티를 읽으려고 할 때 작동합니다. 인수는 다음과 같습니다.
- `target` : 동작을 전달할 객체로 `new Proxy`의 첫 번째 인자입니다.
- `property` : 프로퍼티 이름
- `receiver` : 타깃 프로퍼티가 `getter`라면 `receiver`는 `getter`가 호출될 때 `this` 입니다.
- 대개는 `proxy` 객체 자신이 `this`가 됩니다.
- 프락시 객체를 상속받은 객체가 있다면 해당 객체가 `this`가 되기도 하죠.
- `get`을 활용해 객체에 기본값을 설정해보겠습니다.
- 예시에서 만들 것은, 존재하지 않는 요소를 읽으려고 할 때 기본값 `0`을 반환해주는 배열입니다.
- 존재하지 않는 요소를 읽으려고 하면 배열은 원래 `undefined`을 반환하는데, 예시에선 배열(객체)을 프락시로 감싸서 존재하지 않는 요소(프로퍼티)를 읽으려고 할 때 `0`이 반환되도록 해보겠습니다.

```js
let numbers = [0, 1, 2];

numbers = new Proxy(numbers, {
  get(target, prop) {
    if (prop in target) {
      return target[prop];
    } else {
      return 0; // 기본값
    }
  },
});

alert(numbers[1]); // 1
alert(numbers[123]); // 0 (해당하는 요소가 배열에 없으므로 0이 반환됨)
```

- 예시를 통해 알 수 있듯이 `get`을 사용해 트랩을 만드는 건 상당히 쉽습니다.
- `Proxy`를 사용하면 ‘기본’ 값 설정 로직을 원하는 대로 구현할 수 있죠.
- 구절과 번역문이 저장되어있는 사전이 있다고 가정해봅시다.

```js
let dictionary = {
  Hello: "안녕하세요",
  Bye: "안녕히 가세요",
};

alert(dictionary["Hello"]); // 안녕하세요
alert(dictionary["Welcome"]); // undefined
```

- 지금 상태론 `dictionary`에 없는 구절에 접근하면 `undefined`가 반환됩니다.
- 사전에 없는 구절을 검색하려 했을 때 `undefined`가 아닌 구절 그대로를 반환해주는 게 좀 더 나을 것 같다는 생각이 드네요.
- `dictionary`를 프락시로 감싸서 프로퍼티를 읽으려고 할 때 이를 프락시가 가로채도록 하면 우리가 원하는 기능을 구현할 수 있습니다.

```js
let dictionary = {
  Hello: "안녕하세요",
  Bye: "안녕히 가세요",
};

dictionary = new Proxy(dictionary, {
  get(target, phrase) {
    // 프로퍼티를 읽기를 가로챕니다.
    if (phrase in target) {
      // 조건: 사전에 구절이 있는 경우
      return target[phrase]; // 번역문을 반환합니다
    } else {
      // 구절이 없는 경우엔 구절 그대로를 반환합니다.
      return phrase;
    }
  },
});

// 사전을 검색해봅시다!
// 사전에 없는 구절을 입력하면 입력값이 그대로 반환됩니다.
alert(dictionary["Hello"]); // 안녕하세요
alert(dictionary["Welcome to Proxy"]); // Welcome to Proxy (입력값이 그대로 출력됨)
```

<br>

### 주의

- 프락시 객체가 변수를 어떻게 덮어쓰고 있는지 눈여겨보시기 바랍니다.

```js
dictionary = new Proxy(dictionary, ...);
```

- 타깃 객체의 위치와 상관없이 프락시 객체는 타깃 객체를 덮어써야만 합니다.
- 객체를 프락시로 감싼 이후엔 절대로 타깃 객체를 참조하는 코드가 없어야 합니다.
- 그렇지 않으면 엉망이 될 확률이 아주 높아집니다.

<br>

## set 트랩으로 프로퍼티 값 검증하기

- 숫자만 저장할 수 있는 배열을 만들고 싶다고 가정해봅시다. 숫자형이 아닌 값을 추가하려고 하면 에러가 발생하도록 해야겠죠.
- 프로퍼티에 값을 쓰려고 할 때 이를 가로채는 `set` 트랩을 사용해 이를 구현해보도록 하겠습니다.
- `set` 메서드의 인수는 아래와 같은 역할을 합니다.
- `set(target, property, value, receiver):`
- `target` : 동작을 전달할 객체로 `new Proxy`의 첫 번째 인자입니다.
- `property` : 프로퍼티 이름
- `value` : 프로퍼티 값
- `receiver` : `get` 트랩과 유사하게 동작하는 객체로, `setter` 프로퍼티에만 관여합니다.
- `value` : 프로퍼티 값
- `receiver` : `get` 트랩과 유사하게 동작하는 객체로, `setter` 프로퍼티에만 관여합니다.
- 우리가 구현해야 할 `set` 트랩은 숫자형 값을 설정하려 할 때만 `true`를, 그렇지 않은 경우엔(`TypeError`가 트리거되고) `false`를 반환하도록 해야 합니다.
- `set` 트랩을 사용해 배열에 추가하려는 값이 숫자형인지 검증해봅시다.

```js
let numbers = [];

numbers = new Proxy(numbers, {
  // (*)
  set(target, prop, val) {
    // 프로퍼티에 값을 쓰는 동작을 가로챕니다.
    if (typeof val == "number") {
      target[prop] = val;
      return true;
    } else {
      return false;
    }
  },
});

numbers.push(1); // 추가가 성공했습니다.
numbers.push(2); // 추가가 성공했습니다.
alert("Length is: " + numbers.length); // 2

numbers.push("test"); // Error: 'set' on proxy

alert("윗줄에서 에러가 발생했기 때문에 이 줄은 절대 실행되지 않습니다.");
```

- 배열 관련 기능들은 여전히 사용할 수 있다는 점에 주목해주시기 바랍니다.
- `push`를 사용해 배열에 새로운 요소를 추가하고 `length` 프로퍼티는 이를 잘 반영하고 있다는 것을 통해 이를 확인할 수 있었습니다.
- 프락시를 사용해도 기존에 있던 기능은 절대로 손상되지 않습니다.
- `push`나 `unshift` 같이 배열에 값을 추가해주는 메서드들은 내부에서 `[[Set]]`을 사용하고 있기 때문에 메서드를 오버라이드 하지 않아도 프락시가 동작을 가로채고 값을 검증해줍니다.
- 코드가 깨끗하고 간결해지는 효과가 있죠.

<br>

### true를 잊지 말고 반환해주세요.

- 위에서 언급했듯이 꼭 지켜야 할 규칙이 있습니다.
- `set` 트랩을 사용할 땐 값을 쓰는 게 성공했을 때 반드시 `true`를 반환해줘야 합니다.
- `true`를 반환하지 않았거나 `falsy`한 값을 반환하게 되면 `TypeError`가 발생합니다.

<br>

## ownKeys와 getOwnPropertyDescriptor로 반복 작업하기

- `Object.keys`, `for..in` 반복문을 비롯한 프로퍼티 순환 관련 메서드 대다수는 내부 메서드 `[[OwnPropertyKeys]]`(트랩 메서드는 `ownKeys`임)를 사용해 프로퍼티 목록을 얻습니다.
- 그런데 세부 동작 방식엔 차이가 있습니다.
- `Object.getOwnPropertyNames(obj)` – 심볼형이 아닌 키만 반환합니다.
- `Object.getOwnPropertySymbols(obj)` – 심볼형 키만 반환합니다.
- `Object.keys/values()` – `enumerable` 플래그가 `true`이면서 심볼형이 아닌 키나 심볼형이 아닌 키에 해당하는 값 전체를 반환합니다(프로퍼티 플래그에 관한 내용은 프로퍼티 플래그와 설명자에서 찾아보실 수 있습니다).
- `for..in` 반복문 – `enumerable` 플래그가 `true`인 심볼형이 아닌 키, 프로토타입 키를 순회합니다.
- 메서드마다 차이는 있지만 `[[OwnPropertyKeys]]`를 통해 프로퍼티 목록을 얻는다는 점은 동일합니다.
- 아래 예시에선 `ownKeys` 트랩을 사용해 `_`로 시작하는 프로퍼티는 `for..in` 반복문의 순환 대상에서 제외하도록 해보았습니다.
- `ownKeys`를 사용했기 때문에 `Object.keys`와 `Object.values`에도 동일한 로직이 적용되는 것을 확인할 수 있습니다.

```js
let user = {
  name: "John",
  age: 30,
  _password: "***",
};

user = new Proxy(user, {
  ownKeys(target) {
    return Object.keys(target).filter((key) => !key.startsWith("_"));
  },
});

// "ownKeys" 트랩은 _password를 건너뜁니다.
for (let key in user) alert(key); // name, age

// 아래 두 메서드에도 동일한 로직이 적용됩니다.
alert(Object.keys(user)); // name,age
alert(Object.values(user)); // John,30
```

- 지금까진 의도한 대로 예시가 잘 동작하네요.
- 그런데 객체 내에 존재하지 않는 키를 반환하려고 하면 `Object.keys`는 이 키를 제대로 보여주지 않습니다.

```js
let user = {};

user = new Proxy(user, {
  ownKeys(target) {
    return ["a", "b", "c"];
  },
});

alert(Object.keys(user)); // <빈 문자열>
```

- 이유가 무엇일까요? 답은 간단합니다.
- `Object.keys`는 `enumerable` 플래그가 있는 프로퍼티만 반환하기 때문이죠.
- 이를 확인하기 위해 `Object.keys`는 내부 메서드인 `[[GetOwnProperty]]`를 호출해 모든 프로퍼티의 설명자를 확인합니다.
- 위 예시의 프로퍼티는 설명자가 하나도 없고 `enumerable` 플래그도 없으므로 순환 대상에서 제외되는 것이죠.
- `Object.keys` 호출 시 프로퍼티를 반환하게 하려면 `enumerable` 플래그를 붙여줘 프로퍼티가 객체에 존재하도록 하거나 `[[GetOwnProperty]]`가 호출될 때 이를 중간에서 가로채서 설명자 `enumerable: true`를 반환하게 해주면 됩니다.
- `getOwnPropertyDescriptor` 트랩이 바로 이때 사용되죠.
- 예시를 살펴봅시다.

```js
let user = {};

user = new Proxy(user, {
  ownKeys(target) {
    // 프로퍼티 리스트를 얻을 때 딱 한 번 호출됩니다.
    return ["a", "b", "c"];
  },

  getOwnPropertyDescriptor(target, prop) {
    // 모든 프로퍼티를 대상으로 호출됩니다.
    return {
      enumerable: true,
      configurable: true,
      /* 이 외의 플래그도 반환할 수 있습니다. "value:..."도 가능합니다. */
    };
  },
});

alert(Object.keys(user)); // a, b, c
```

- 객체에 프로퍼티가 없을 때 `[[GetOwnProperty]]`만 가로채면 된다는 점을 다시 한번 상기하시기 바랍니다.

<br>

## deleteProperty와 여러 트랩을 사용해 프로퍼티 보호하기

<br>

[출처]
https://ko.javascript.info/proxy
