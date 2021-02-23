# call/apply와 데코레이터, 포워딩

- 자바스크립트는 함수를 다룰 때 탁월한 유연성을 제공합니다.
- 함수는 이곳저곳 전달될 수 있고, 객체로도 사용될 수 있습니다.
- 이번 챕터에선 함수 간에 호출을 어떻게 포워딩(forwarding) 하는지, 함수를 어떻게 데코레이팅(decorating) 하는지에 대해 알아보겠습니다.

<br>

## 코드 변경 없이 캐싱 기능 추가하기

- CPU를 많이 잡아먹지만 결과는 안정적인 함수 `slow(x)`가 있다고 가정해 봅시다.
- 결과가 안정적이라는 말은 x가 같으면 호출 결과도 같다는 것을 의미입니다.
- `slow(x)`가 자주 호출된다면, 결과를 어딘가에 저장(캐싱)해 재연산에 걸리는 시간을 줄이고 싶을 겁니다.
- 아래 예시에선 `slow()` 안에 캐싱 관련 코드를 추가하는 대신, 래퍼 함수를 만들어 캐싱 기능을 추가할 예정입니다. 곧 정리하겠지만, 이렇게 래퍼 함수를 만들면 여러 가지 이점이 있습니다.

```js
function slow(x) {
  // CPU 집약적인 작업이 여기에 올 수 있습니다.
  alert(`slow(${x})을/를 호출함`);
  return x;
}

function cachingDecorator(func) {
  let cache = new Map();

  return function (x) {
    if (cache.has(x)) {
      // cache에 해당 키가 있으면
      return cache.get(x); // 대응하는 값을 cache에서 읽어옵니다.
    }

    let result = func(x); // 그렇지 않은 경우엔 func를 호출하고,

    cache.set(x, result); // 그 결과를 캐싱(저장)합니다.
    return result;
  };
}

slow = cachingDecorator(slow);

alert(slow(1)); // slow(1)이 저장되었습니다.
alert("다시 호출: " + slow(1)); // 동일한 결과

alert(slow(2)); // slow(2)가 저장되었습니다.
alert("다시 호출: " + slow(2)); // 윗줄과 동일한 결과
```

- `cachingDecorator`같이 인수로 받은 함수의 행동을 변경시켜주는 함수를 데코레이터(decorator) 라고 부릅니다.
- 모든 함수를 대상으로 `cachingDecorator`를 호출 할 수 있는데, 이때 반환되는 것은 캐싱 래퍼입니다.
- 함수에 `cachingDecorator`를 적용하기만 하면 캐싱이 가능한 함수를 원하는 만큼 구현할 수 있기 때문에 데코레이터 함수는 아주 유용하게 사용됩니다.
- 캐싱 관련 코드를 함수 코드와 분리할 수 있기 때문에 함수의 코드가 간결해진다는 장점도 있습니다.
- 아래 그림에서 볼 수 있듯이 `cachingDecorator(func)`를 호출하면 `‘래퍼(wrapper)’`, `function(x)`이 반환됩니다.
- `래퍼 function(x)`는 `func(x)`의 호출 결과를 캐싱 로직으로 감쌉니다(wrapping).

<br>

- 바깥 코드에서 봤을 때, 함수 `slow`는 래퍼로 감싼 이전이나 이후나 동일한 일을 수행합니다.
- 행동 양식에 캐싱 기능이 추가된 것뿐입니다.
- `slow` 본문을 수정하는 것 보다 독립된 래퍼 함수 `cachingDecorator`를 사용할 때 생기는 이점을 정리하면 다음과 같습니다.
  1. `cachingDecorator`를 재사용 할 수 있습니다. 원하는 함수 어디에든 `cachingDecorator`를 적용할 수 있습니다.
  2. 캐싱 로직이 분리되어 `slow` 자체의 복잡성이 증가하지 않습니다.
  3. 필요하다면 여러 개의 데코레이터를 조합해서 사용할 수도 있습니다(추가 데코레이터는 `cachingDecorator` 뒤를 따릅니다).

## 'func.call’를 사용해 컨텍스트 지정하기

- 위에서 구현한 캐싱 데코레이터는 객체 메서드에 사용하기엔 적합하지 않습니다.
- 객체 메서드 `worker.slow()`는 데코레이터 적용 후 제대로 동작하지 않죠.

```js
// worker.slow에 캐싱 기능을 추가해봅시다.
let worker = {
  someMethod() {
    return 1;
  },

  slow(x) {
    // CPU 집약적인 작업이라 가정
    alert(`slow(${x})을/를 호출함`);
    return x * this.someMethod(); // (*)
  },
};

// 이전과 동일한 코드
function cachingDecorator(func) {
  let cache = new Map();
  return function (x) {
    if (cache.has(x)) {
      return cache.get(x);
    }
    let result = func(x); // (**)
    cache.set(x, result);
    return result;
  };
}

alert(worker.slow(1)); // 기존 메서드는 잘 동작합니다.

worker.slow = cachingDecorator(worker.slow); // 캐싱 데코레이터 적용

alert(worker.slow(2)); // 에러 발생!, Error: Cannot read property 'someMethod' of undefined
```

- `(*)`로 표시한 줄에서 `this.someMethod` 접근에 실패했기 때문에 에러가 발생했습니다.
- 원인은 `(**)`로 표시한 줄에서 래퍼가 기존 함수 `func(x)`를 호출하면 `this`가 `undefined`가 되기 때문입니다.
- 아래 코드를 실행해도 비슷한 증상이 나타납니다.

```js
let func = worker.slow;
func(2);
```

- 래퍼가 기존 메서드 호출 결과를 전달하려 했지만 `this`의 컨텍스트가 사라졌기 때문에 에러가 발생하는 것이죠.
- 에러가 발생하지 않게 코드를 수정해 봅시다.
- 먼저, `this`를 명시적으로 고정해 함수를 호출할 수 있게 해주는 특별한 내장 함수 메서드 `func.call(context, …args)`에 대해 알아봅시다.
- 문법은 다음과 같습니다.

```js
// 메서드를 호출하면 메서드의 첫 번째 인수가 this, 이어지는 인수가 func의 인수가 된 후, func이 호출됩니다.

func.call(context, arg1, arg2, ...)

// 아래 함수와 메서드를 호출하면 거의 동일한 일이 발생합니다.
func(1, 2, 3);
func.call(obj, 1, 2, 3)

```

- 둘 다 인수로 `1, 2, 3`을 받죠. 유일한 차이점은 `func.call`에선 `this`가 `obj로 고정`된다는 점입니다.

<br>

- 다른 컨텍스트(다른 객체) 하에 `sayHi`를 호출하는 예시를 살펴봅시다.
- `sayHi.call(user)`를 호출하면 `sayHi`의 컨텍스트가 `this=user`로, `sayHi.call(admin)`을 호출하면 `sayHi`의 컨텍스트가 `this=admin`로 설정됩니다.

```js
function sayHi() {
  alert(this.name);
}

let user = { name: "John" };
let admin = { name: "Admin" };

// call을 사용해 원하는 객체가 'this'가 되도록 합니다.
sayHi.call(user); // this = John
sayHi.call(admin); // this = Admin

// call을 사용해 컨텍스트와 phrase에 원하는 값을 지정해 보았습니다.

function say(phrase) {
  alert(this.name + ": " + phrase);
}

let user = { name: "John" };

// this엔 user가 고정되고, "Hello"는 메서드의 첫 번째 인수가 됩니다.
say.call(user, "Hello"); // John: Hello

// call을 사용해 컨텍스트를 원본 함수로 전달하면 에러가 발생하지 않습니다.
```

```js
let worker = {
  someMethod() {
    return 1;
  },

  slow(x) {
    alert(`slow(${x})을/를 호출함`);
    return x * this.someMethod(); // (*)
  },
};

function cachingDecorator(func) {
  let cache = new Map();
  return function (x) {
    if (cache.has(x)) {
      return cache.get(x);
    }
    let result = func.call(this, x); // 이젠 'this'가 제대로 전달됩니다.
    cache.set(x, result);
    return result;
  };
}

worker.slow = cachingDecorator(worker.slow); // 캐싱 데코레이터 적용

alert(worker.slow(2)); // 제대로 동작합니다.
alert(worker.slow(2)); // 제대로 동작합니다. 다만, 원본 함수가 호출되지 않고 캐시 된 값이 출력됩니다.
```

- 이제 에러 없이 모든 게 정상적으로 동작합니다.
- 명확한 이해를 위해 `this`가 어떤 과정을 거쳐 전달되는지 자세히 살펴보겠습니다.
  1. 데코레이터를 적용한 후에 `worker.slow`는 래퍼 `function (x) { ... }`가 됩니다.
  2. `worker.slow(2)`를 실행하면 래퍼는 2를 인수로 받고, `this = worker`가 됩니다(점 앞의 객체).
  3. 결과가 캐시되지 않은 상황이라면 `func.call(this, x)`에서 현재 `this (=worker)와 인수(=2)`를 원본 메서드에 전달합니다.

<br>

## 여러 인수 전달하기

- `cachingDecorator`를 좀 더 다채롭게 해봅시다.
- 지금 상태론 인수가 하나뿐인 함수에만 `cachingDecorator`를 적용할 수 있습니다.
- 복수 인수를 가진 메서드, `worker.slow`를 캐싱하려면 어떻게 해야 할지 생각해 봅시다.

```js
let worker = {
  slow(min, max) {
    return min + max; // CPU를 아주 많이 쓰는 작업이라고 가정
  },
};

// 동일한 인수를 전달했을 때 호출 결과를 기억할 수 있어야 합니다.
worker.slow = cachingDecorator(worker.slow);
```

- 지금까진 인수가 `x` 하나뿐이었기 때문에 `cache.set(x, result)`으로 결과를 저장하고 `cache.get(x)`으로 저장된 결과를 불러오기만 하면 됐습니다.
- 그런데 이제부턴 `(min,max)`같이 인수가 여러 개이고, 이 인수들을 넘겨 호출한 결과를 기억해야 합니다. 네이티브 맵은 단일 키만 받지만 말이죠.
- 해결 방법은 여러 가지입니다.
  1. 복수 키를 지원하는 맵과 유사한 자료 구조 구현하기(서드 파티 라이브러리 등을 사용해도 됨)
  2. 중첩 맵을 사용하기. `(max, result)` 쌍 저장은 `cache.set(min)`으로, `result`는 `cache.get(min).get(max)`을 사용해 얻습니다.
  3. 두 값을 하나로 합치기. 맵의 키로 문자열 `"min,max"`를 사용합니다. 여러 값을 하나로 합치는 코드는 해싱 함수`(hashing function)` 에 구현해 유연성을 높입니다.

<br>

[출처]
https://ko.javascript.info/call-apply-decorators#ref-2471
