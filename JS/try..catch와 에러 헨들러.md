# 'try..catch'와 에러 핸들링

- 아무리 프로그래밍에 능한 사람이더라도 에러가 있는 스크립트를 작성할 수 있습니다.
- 원인은 아마도 실수, 예상치 못한 사용자 입력, 잘못된 서버 응답 등의 수천만 가지 이유 때문일 겁니다.
- 에러가 발생하면 스크립트는 ‘죽고’(즉시 중단되고), 콘솔에 에러가 출력됩니다.
- 그러나 `try..catch` 문법을 사용하면 스크립트가 죽는 걸 방지하고, 에러를 ‘잡아서(catch)’ 더 합당한 무언가를 할 수 있게 됩니다.

<br>

## ‘try…catch’ 문법

- `‘try…catch’` 문법은 `'try’`와 `'catch’`라는 두 개의 주요 블록으로 구성됩니다.

```js
try {
  // 코드...
} catch (err) {
  // 에러 핸들링
}

// 먼저, try {...} 안의 코드가 실행됩니다.
// 에러가 없다면, try 안의 마지막 줄까지 실행되고, catch 블록은 건너뜁니다.
// 에러가 있다면, try 안 코드의 실행이 중단되고, catch(err) 블록으로 제어 흐름이 넘어갑니다.
// 변수 err(아무 이름이나 사용 가능)는 무슨 일이 일어났는지에 대한 설명이 담긴 에러 객체를 포함합니다.

try {
  alert("try 블록 시작"); // (1) <--

  // ...에러가 없습니다.

  alert("try 블록 끝"); // (2) <--
} catch (err) {
  alert("에러가 없으므로, catch는 무시됩니다."); // (3)
}

try {
  alert("try 블록 시작"); // (1) <--

  lalala; // 에러, 변수가 정의되지 않음!

  alert("try 블록 끝(절대 도달하지 않음)"); // (2)
} catch (err) {
  alert(`에러가 발생했습니다!`); // (3) <--
}
```

<br>

### `try..catch`는 오직 런타임 에러에만 동작합니다.

- `try..catch`는 실행 가능한(runnable) 코드에만 동작합니다.
- 실행 가능한 코드는 유효한 자바스크립트 코드를 의미합니다.
- 중괄호 짝이 안 맞는 것처럼 코드가 문법적으로 잘못된 경우엔 `try..catch`가 동작하지 않습니다

```js
try {
  {{{{{{{{{{{{
} catch(e) {
  alert("유효하지 않은 코드이기 때문에, 자바스크립트 엔진은 이 코드를 이해할 수 없습니다.");
}
```

- 자바스크립트 엔진은 코드를 읽고 난 후 코드를 실행합니다.
- 코드를 읽는 중에 발생하는 에러는 `'parse-time 에러’`라고 부르는데, 엔진은 이 코드를 이해할 수 없기 때문에 `parse-time` 에러는 코드 안에서 복구가 불가능합니다.
- `try..catch`는 유효한 코드에서 발생하는 에러만 처리할 수 있습니다.
- 이런 에러를 `‘런타임 에러(runtime error)’` 혹은 `'예외(exception)'`라고 부릅니다.

<br>

### `try..catch`는 동기적으로 동작합니다.

- `setTimeout`처럼 `‘스케줄 된(scheduled)’` 코드에서 발생한 예외는 `try..catch`에서 잡아낼 수 없습니다.

```js
try {
  setTimeout(function () {
    noSuchVariable; // 스크립트는 여기서 죽습니다.
  }, 1000);
} catch (e) {
  alert("작동 멈춤");
}
```

- `setTimeout`에 넘겨진 익명 함수는 엔진이 `try..catch`를 떠난 다음에서야 실행되기 때문입니다.
- 스케줄 된 함수 내부의 예외를 잡으려면, `try..catch`를 반드시 함수 내부에 구현해야 합니다.

```js
setTimeout(function () {
  try {
    noSuchVariable; // 이제 try..catch에서 에러를 핸들링 할 수 있습니다!
  } catch {
    alert("에러를 잡았습니다!");
  }
}, 1000);
```

<br>

## 에러 객체

- 에러가 발생하면 자바스크립트는 에러 상세내용이 담긴 객체를 생성합니다. 그 후, `catch` 블록에 이 객체를 인수로 전달합니다.

```js
try {
  // ...
} catch (err) {
  // <-- '에러 객체', err 대신 다른 이름으로도 쓸 수 있음
  // ...
}
```

- 내장 에러 전체와 에러 객체는 두 가지 주요 프로퍼티를 가집니다.
- `name` : 에러 이름. 정의되지 않은 변수 때문에 발생한 에러라면 `"ReferenceError"`가 이름이 됩니다.
- `message` : 에러 상세 내용을 담고 있는 문자 메시지
- 표준은 아니지만, `name`과 `message` 이외에 대부분의 호스트 환경에서 지원하는 프로퍼티도 있습니다. `stack`은 가장 널리 사용되는 비표준 프로퍼티 중 하나입니다.
- `stack` : 현재 호출 스택. 에러를 유발한 중첩 호출들의 순서 정보를 가진 문자열로 디버깅 목적으로 사용됩니다.

```js
try {
  lalala; // 에러, 변수가 정의되지 않음!
} catch (err) {
  alert(err.name); // ReferenceError
  alert(err.message); // lalala is not defined
  alert(err.stack); // ReferenceError: lalala is not defined at ... (호출 스택)

  // 에러 전체를 보여줄 수도 있습니다.
  // 이때, 에러 객체는 "name: message" 형태의 문자열로 변환됩니다.
  alert(err); // ReferenceError: lalala is not defined
}
```

<br>

## 선택적 ‘catch’ 바인딩

- 에러에 대한 자세한 정보가 필요하지 않으면, `catch`에서 이를 생략할 수 있습니다.

```js
try {
  // ...
} catch {
  // <-- (err) 없이 쓸 수 있음
  // ...
}
```

<br>

## ‘try…catch’ 사용하기

- `try..catch`가 실무에서 어떻게 사용되는지 알아봅시다.
- 앞서 `JSON`으로 인코딩된 값을 읽을 수 있도록 해주는 `JSON.parse(str)` 메서드에 대해 배운 바 있습니다.
- 이 메서드는 주로 서버 등에서 네트워크를 통해 전달받은 데이터를 디코딩하는 데 사용합니다.
- 전달받은 데이터에 `JSON.parse`를 호출하는 식으로 사용되죠.

```js
let json = '{"name":"John", "age": 30}'; // 서버로부터 전달받은 데이터

let user = JSON.parse(json); // 전달받은 문자열을 자바스크립트 객체로 변환

// 문자열 형태로 전달받은 user가 프로퍼티를 가진 객체가 됨
alert(user.name); // John
alert(user.age); // 30
```

- 잘못된 형식의 `json`이 들어온 경우, `JSON.parse`는 에러를 만들기 때문에 스크립트가 ‘죽습니다’.
- 스크립트가 죽는 것에 만족해야 할까요? 당연히 아니죠!
- 서버에서 전달받은 데이터가 잘못되어 스크립트가 죽는 경우, 사용자는 개발자 콘솔을 열지 않는 이상 절대 원인을 알 수 없습니다.
- 그런데 사람들은 메시지 등을 통해 에러의 원인을 알지 못한 채 무언가가 '그냥 죽는 것’을 정말 싫어합니다.
- `try..catch`를 사용해 이를 처리해 봅시다.

```js
let json = "{ bad json }";

try {
  let user = JSON.parse(json); // <-- 여기서 에러가 발생하므로
  alert(user.name); // 이 코드는 동작하지 않습니다.
} catch (e) {
  // 에러가 발생하면 제어 흐름이 catch 문으로 넘어옵니다.
  alert("데이터에 에러가 있어 재요청을 시도합니다.");
  alert(e.name);
  alert(e.message);
}
```

- 위 예시에선 에러가 발생했다는 걸 보여주기 위해 간단히 예외처리했지만, `catch` 블록 안에서 새로운 네트워크 요청 보내기, 사용자에게 대안 제안하기, 로깅 장치에 에러 정보 보내기 등과 같은 구체적인 일을 할 수 있습니다.
- 스크립트가 죽도록 놔두는 것보다 훨씬 나은 대응이죠.

<br>

## 직접 에러를 만들어서 던지기

- `json`이 문법적으로 잘못되진 않았지만, 스크립트 내에서 사용 중인 필수 프로퍼티 `name`을 가지고 있지 않다면 무슨 일이 생길까요?

```js
let json = '{ "age": 30 }'; // 불완전한 데이터

try {
  let user = JSON.parse(json); // <-- 에러 없음
  alert(user.name); // 이름이 없습니다!
} catch (e) {
  alert("실행되지 않습니다.");
}
```

- 위 예시에서 `JSON.parse`는 정상적으로 실행되었지만 `name`이 없는 건 에러를 유발하는 상황입니다.
- 이제 `throw` 연산자를 사용해 에러 처리를 통합해 보도록 하겠습니다.

<br>

### ‘throw’ 연산자

- `throw` 연산자는 에러를 생성합니다.

```js
throw <error object>
```

- 이론적으로는 숫자, 문자열 같은 원시형 자료를 포함한 어떤 것이든 에러 객체(error object)로 사용할 수 있습니다.
- 하지만 내장 에러와의 호환을 위해 되도록 에러 객체에 `name`과 `message` 프로퍼티를 넣어주는 것을 권장합니다.
- 자바스크립트는 `Error`, `SyntaxError`, `ReferenceError`, `TypeError`등의 표준 에러 객체 관련 생성자를 지원합니다.
- 이 생성자들을 이용해 에러 객체를 만들 수도 있습니다.

```js
let error = new Error(message);
// or
let error = new SyntaxError(message);
let error = new ReferenceError(message);
// ...
```

- 일반 객체가 아닌 내장 생성자를 사용해 만든 내장 에러 객체의 `name` 프로퍼티는 생성자 이름과 동일한 값을 갖습니다.
- 프로퍼티 `message`의 값은 인수에서 가져옵니다.

```js
let error = new Error("이상한 일이 발생했습니다. o_O");

alert(error.name); // Error
alert(error.message); // 이상한 일이 발생했습니다. o_O
```

- 잘못된 데이터를 받았을 때, `JSON.parse`가 어떤 종류의 에러를 만들어내는지 아래 코드를 통해 살펴봅시다.

```js
try {
  JSON.parse("{ 잘못된 형식의 json o_O }");
} catch (e) {
  alert(e.name); // SyntaxError
  alert(e.message); // Unexpected token b in JSON at position 2
}
```

- `SyntaxError`가 발생하네요.
- 사용자를 나타내는 객체에 `name` 프로퍼티는 반드시 있어야 하므로, 이제 `name`이 없으면 에러가 발생한 것으로 간주하고 예외 처리해봅시다.
- `throw` 연산자를 사용해 에러를 던져보겠습니다.

```js
let json = '{ "age": 30 }'; // 불완전한 데이터

try {
  let user = JSON.parse(json); // <-- 에러 없음

  if (!user.name) {
    throw new SyntaxError("불완전한 데이터: 이름 없음"); // (*)
  }

  alert(user.name);
} catch (e) {
  alert("JSON Error: " + e.message); // JSON Error: 불완전한 데이터: 이름 없음
}
```

- `(*)`로 표시한 줄에서 `throw` 연산자는 `message`를 이용해 `SyntaxError`를 생성합니다.
- 에러 생성 방식은 자바스크립트가 자체적으로 에러를 생성하는 방식과 동일합니다.
- 에러가 발생했으므로 `try`의 실행은 즉시 중단되고 제어 흐름이 `catch`로 넘어간 것을 얼럿 창을 통해 확인할 수 있네요.
- 이제 `JSON.parse`에서 에러가 발생한 경우를 포함해서 모든 에러를 `catch` 블록 안에서 처리할 수 있게 되었습니다.

<br>

## 에러 다시 던지기

<br>

[출처]
https://ko.javascript.info/try-catch
