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
