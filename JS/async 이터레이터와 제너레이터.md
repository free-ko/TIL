# async 이터레이터와 제너레이터

- 비동기 이터레이터(asynchronous iterator)를 사용하면 비동기적으로 들어오는 데이터를 필요에 따라 처리할 수 있습니다. 네트워크를 통해 데이터가 여러 번에 걸쳐 들어오는 상황을 처리할 수 있게 되죠. 비동기 이터레이터에 더하여 비동기 제너레이터(asynchronous generator)를 사용하면 이런 데이터를 좀 더 편리하게 처리할 수 있습니다.
- 먼저 간단한 예시를 살펴보며 문법을 익힌 후, 실무에서 벌어질 법한 사례를 가지고 `async` 이터레이터와 제너레이터가 어떻게 사용되는지 알아보겠습니다.

<br>

## async 이터레이터

- 비동기 이터레이터는 일반 이터레이터와 유사하며, 약간의 문법적인 차이가 있습니다.
- `iterable` 객체 챕터에서 살펴본 바와 같이 ‘일반’ 이터러블은 객체입니다.

```js
let range = {
  from: 1,
  to: 5,

  // for..of 최초 실행 시, Symbol.iterator가 호출됩니다.
  [Symbol.iterator]() {
    // Symbol.iterator메서드는 이터레이터 객체를 반환합니다.
    // 이후 for..of는 반환된 이터레이터 객체만을 대상으로 동작하는데,
    // 다음 값은 next()에서 정해집니다.
    return {
      current: this.from,
      last: this.to,

      // for..of 반복문에 의해 각 이터레이션마다 next()가 호출됩니다.
      next() {
        // (2)
        //  next()는 객체 형태의 값, {done:.., value :...}를 반환합니다.
        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      },
    };
  },
};

for (let value of range) {
  alert(value); // 1, 2, 3, 4, 5
}
```

- 일반 이터레이터에 대한 설명은 `iterable` 객체에서 자세히 다루고 있으니, 꼭 살펴보시기 바랍니다.
- 이제, 이터러블 객체를 비동기적으로 만들려면 어떤 작업이 필요한지 알아봅시다.

1. `Symbol.iterator` 대신, `Symbol.asyncIterator`를 사용해야 합니다.
2. `next()`는 프라미스를 반환해야 합니다.
3. 비동기 이터러블 객체를 대상으로 하는 반복 작업은 `for await` (`let item of iterable`) 반복문을 사용해 처리해야 합니다.

- 익숙한 예시인 이터러블 객체 `range`를 토대로, 일초마다 비동기적으로 값을 반환하는 이터러블 객체를 만들어보겠습니다.

```js
let range = {
  from: 1,
  to: 5,

  // for await..of 최초 실행 시, Symbol.asyncIterator가 호출됩니다.
  [Symbol.asyncIterator]() {
    // (1)
    // Symbol.asyncIterator 메서드는 이터레이터 객체를 반환합니다.
    // 이후 for await..of는 반환된 이터레이터 객체만을 대상으로 동작하는데,
    // 다음 값은 next()에서 정해집니다.
    return {
      current: this.from,
      last: this.to,

      // for await..of 반복문에 의해 각 이터레이션마다 next()가 호출됩니다.
      async next() {
        // (2)
        //  next()는 객체 형태의 값, {done:.., value :...}를 반환합니다.
        // (객체는 async에 의해 자동으로 프라미스로 감싸집니다.)

        // 비동기로 무언가를 하기 위해 await를 사용할 수 있습니다.
        await new Promise((resolve) => setTimeout(resolve, 1000)); // (3)

        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      },
    };
  },
};

(async () => {
  for await (let value of range) {
    // (4)
    alert(value); // 1,2,3,4,5
  }
})();
```

- 위 예시에서 볼 수 있듯이, `async` 이터레이터는 일반 이터레이터와 구조가 유사합니다. 하지만 아래와 같은 차이가 있습니다.

1. 객체를 비동기적으로 반복 가능하도록 하려면, `Symbol.asyncIterator`메서드가 반드시 구현되어 있어야 합니다. – (1)
2. `Symbol.asyncIterator`는 프라미스를 반환하는 메서드인 `next()`가 구현된 객체를 반환해야 합니다. – (2)
3. `next()`는 `async` 메서드일 필요는 없습니다. 프라미스를 반환하는 메서드라면 일반 메서드도 괜찮습니다. 다만, `async`를 사용하면 `await`도 사용할 수 있기 때문에, 여기선 편의상 `async`메서드를 사용해 일 초의 딜레이가 생기도록 했습니다. – (3)
4. 반복 작업을 하려면 ‘`for`’ 뒤에 '`await`’를 붙인 `for await`(`let value of range`)를 사용하면 됩니다. `for await`(`let value of range`)가 실행될 때 `range[Symbol.asyncIterator]()`가 일회 호출되는데, 그 이후엔 각 값을 대상으로 `next()`가 호출됩니다. – (4)

<br>

### 전개 문법 ...은 비동기적으로 동작하지 않습니다.

<br>

[출처]
https://ko.javascript.info/async-iterators-generators
