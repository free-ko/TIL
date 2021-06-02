# 프라미스 API

- `Promise` 클래스에는 5가지 정적 메서드가 있습니다.

<br>

## Promise.all

- 여러 개의 프라미스를 동시에 실행시키고 모든 프라미스가 준비될 때까지 기다린다고 해봅시다.
- 복수의 URL에 동시에 요청을 보내고, 다운로드가 모두 완료된 후에 콘텐츠를 처리할 때 이런 상황이 발생합니다.
- `Promise.all`은 이럴 때 사용할 수 있습니다.

```js
let promise = Promise.all([...promises...]);

```

- `Promise.all`은 요소 전체가 프라미스인 배열(엄밀히 따지면 이터러블 객체이지만, 대개는 배열임)을 받고 새로운 프라미스를 반환합니다.
- 배열 안 프라미스가 모두 처리되면 새로운 프라미스가 이행되는데, 배열 안 프라미스의 결괏값을 담은 배열이 새로운 프라미스의 `result`가 됩니다.
- 아래 `Promise.all`은 `3초` 후에 처리되고, 반환되는 프라미스의 `result`는 배열 `[1, 2, 3]`이 됩니다.

```js
Promise.all([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
]).then(alert); // 프라미스 전체가 처리되면 1, 2, 3이 반환됩니다. 각 프라미스는 배열을 구성하는 요소가 됩니다.
```

- 배열 `result`의 요소 순서는 `Promise.all`에 전달되는 프라미스 순서와 상응한다는 점에 주목해 주시기 바랍니다.
- `Promise.all`의 첫 번째 프라미스는 가장 늦게 이행되더라도 처리 결과는 배열의 첫 번째 요소에 저장됩니다.
- 작업해야 할 데이터가 담긴 배열을 프라미스 배열로 매핑하고, 이 배열을 `Promise.all`로 감싸는 트릭은 자주 사용됩니다.
- `URL`이 담긴 배열을 `fetch`를 써서 처리하는 예시를 살펴봅시다.

```js
let urls = [
  "https://api.github.com/users/iliakan",
  "https://api.github.com/users/remy",
  "https://api.github.com/users/jeresig",
];

// fetch를 사용해 url을 프라미스로 매핑합니다.
let requests = urls.map((url) => fetch(url));

// Promise.all은 모든 작업이 이행될 때까지 기다립니다.
Promise.all(requests).then((responses) =>
  responses.forEach((response) => alert(`${response.url}: ${response.status}`))
);
```

- `GitHub` 유저네임이 담긴 배열을 사용해 사용자 정보를 가져오는 예시를 살펴봅시다(id를 기준으로 장바구니 목록을 불러올 때도 같은 로직을 사용할 수 있습니다).

```js
let names = ["iliakan", "remy", "jeresig"];

let requests = names.map((name) =>
  fetch(`https://api.github.com/users/${name}`)
);

Promise.all(requests)
  .then((responses) => {
    // 모든 응답이 성공적으로 이행되었습니다.
    for (let response of responses) {
      alert(`${response.url}: ${response.status}`); // 모든 url의 응답코드가 200입니다.
    }

    return responses;
  })
  // 응답 메시지가 담긴 배열을 response.json()로 매핑해, 내용을 읽습니다.
  .then((responses) => Promise.all(responses.map((r) => r.json())))
  // JSON 형태의 응답 메시지는 파싱 되어 배열 'users'에 저장됩니다.
  .then((users) => users.forEach((user) => alert(user.name)));
```

<br>

### Promise.all에 전달되는 프라미스 중 하나라도 거부되면, Promise.all이 반환하는 프라미스는 에러와 함께 바로 거부됩니다.

```js
Promise.all([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error("에러 발생!")), 2000)
  ),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).catch(alert); // Error: 에러 발생!
```

- 2초 후 두 번째 프라미스가 거부되면서 `Promise.all` 전체가 거부되고, `.catch`가 실행됩니다. 거부 에러는 `Promise.all` 전체의 결과가 됩니다.

<br>

### 에러가 발생하면 다른 프라미스는 무시됩니다.

<br>

[출처]
https://ko.javascript.info/promise-api
