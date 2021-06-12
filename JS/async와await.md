# async와 await

- `async`와 `await`라는 특별한 문법을 사용하면 프라미스를 좀 더 편하게 사용할 수 있습니다.
- `async/await`는 놀라울 정도로 이해하기 쉽고, 사용법도 어렵지 않습니다.

<br>

## async

```js
async function f() {
  return 1;
}
```

- `function` 앞에 `async`를 붙이면 해당 함수는 항상 프라미스를 반환합니다.
- 프라미스가 아닌 값을 반환하더라도 이행 상태의 프라미스(`resolved promise`)로 값을 감싸 이행된 프라미스가 반환되도록 합니다.
- 아래 예시의 함수를 호출하면 `result`가 `1`인 이행 프라미스가 반환됩니다.

```js
async function f() {
  return 1;
}

f().then(alert); // 1
```

- 명시적으로 프라미스를 반환하는 것도 가능한데, 결과는 동일합니다.

```js
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

- `async`가 붙은 함수는 반드시 프라미스를 반환하고, 프라미스가 아닌 것은 프라미스로 감싸 반환합니다.
- 굉장히 간단하죠? 그런데 `async`가 제공하는 기능은 이뿐만이 아닙니다.
- 또 다른 키워드 `await`는 `async` 함수 안에서만 동작합니다. `await`는 아주 멋진 녀석이죠.

<br>

## await

```js
// await는 async 함수 안에서만 동작합니다.
let value = await promise;
```

- 자바스크립트는 `await` 키워드를 만나면 프라미스가 처리(`settled`)될 때까지 기다립니다. 결과는 그 이후 반환됩니다.
- 1초 후 이행되는 프라미스를 예시로 사용하여 `await`가 어떻게 동작하는지 살펴봅시다.

```js
async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000);
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

  alert(result); // "완료!"
}

f();
```

- 함수를 호출하고, 함수 본문이 실행되는 도중에 `(*)`로 표시한 줄에서 실행이 잠시 `'중단’`되었다가 프라미스가 처리되면 실행이 재개됩니다.
- 이때 프라미스 객체의 `result` 값이 변수 `result`에 할당됩니다. 따라서 위 예시를 실행하면 1초 뒤에 '완료!'가 출력됩니다.
- `await('기다리다’라는 뜻을 가진 영단어)`는 말 그대로 프라미스가 처리될 때까지 함수 실행을 기다리게 만듭니다.
- 프라미스가 처리되면 그 결과와 함께 실행이 재개되죠.
- 프라미스가 처리되길 기다리는 동안엔 엔진이 다른 일(다른 스크립트를 실행, 이벤트 처리 등)을 할 수 있기 때문에, CPU 리소스가 낭비되지 않습니다.
- `await`는 `promise.then`보다 좀 더 세련되게 프라미스의 `result` 값을 얻을 수 있도록 해주는 문법입니다.
- `promise.then`보다 가독성 좋고 쓰기도 쉽습니다.

<br>

### 일반 함수엔 await을 사용할 수 없습니다.

- `async` 함수가 아닌데 `await`을 사용하면 문법 에러가 발생합니다.

```js
function f() {
  let promise = Promise.resolve(1);
  let result = await promise; // Syntax error
}
```

- `function` 앞에 `async`를 붙이지 않으면 이런 에러가 발생할 수 있습니다.
- 앞서 설명해 드린 바와 같이 `await`는 `async` 함수 안에서만 동작합니다.

```js
async function showAvatar() {
  // JSON 읽기
  let response = await fetch("/article/promise-chaining/user.json");
  let user = await response.json();

  // github 사용자 정보 읽기
  let githubResponse = await fetch(`https://api.github.com/users/${user.name}`);
  let githubUser = await githubResponse.json();

  // 아바타 보여주기
  let img = document.createElement("img");
  img.src = githubUser.avatar_url;
  img.className = "promise-avatar-example";
  document.body.append(img);

  // 3초 대기
  await new Promise((resolve, reject) => setTimeout(resolve, 3000));

  img.remove();

  return githubUser;
}

showAvatar();
```

<br>

### await는 최상위 레벨 코드에서 작동하지 않습니다.

<br>

[출처]
https://ko.javascript.info/async-await
