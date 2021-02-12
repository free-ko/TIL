# 오래된 `var`

- 변수 선언 방법 3가지 `let`, `const`, `var`가 있습니다.
- `var`로 선언한 변수는 `let`으로 선언한 변수와 유사합니다.
- 대부분의 경우에 `let`을 `var`로, `var`를 `let`으로 바꿔도 큰 문제 없이 동작합니다.
- 하지만 `var`는 초기 JS 구현 방식 때문에 `let`과 `const`로 선언한 변수와는 다른 방식으로 동작합니다.
- 근래엔 `var`를 쓰지 않아서 이를 만나는 건 흔치 않은 일이지만, `var`는 오래된 JS에서 괴물 같은 존재 입니다.

<br>

## `var`는 블록 스코프가 없습니다.

- `var`로 선언한 변수의 스코프는 함수 스코프이거나 전역 스코프 입니다.
- 블록 기준으로 스코프가 생기지 않기 때문에 블록 밖에서 접근 가능합니다.

```js
if (true) {
  var test = true; // 'let' 대신 'var'를 사용 했습니다.
}

alert(test); // true(if 문이 끝났어도 변수에 여전히 접근할 수 있음)

// var는 코드 블록을 무시하기 때문에 test는 전역 변수가 됩니다. 전역 스코프에서 이 변수에 접근할 수 있죠.
// 두 번째 행에서 var test가 아닌 let test를 사용했다면, 변수 test는 if문 안에서만 접근할 수 있습니다.
```

<br>

- 반복문에서도 유사한 일이 일어납니다. `var`는 블록이나 루프 수준의 스코프를 형성하지 않기 때문이죠.

```js
for (var i = 0; i < 10; i++) {
  // ...
}

alert(i); // 10, 반복문이 종료되었지만 'i'는 전역 변수이므로 여전히 접근 가능합니다.
```

<br>

- 코드 블록이 함수 안에 있다면, `var`는 함수 레벨 변수가 됩니다.

```js
function sayHi() {
  if (true) {
    var phrase = "Hello";
  }

  alert(phrase); // 제대로 출력됩니다.
}

sayHi();
alert(phrase); // Error: phrase is not defined
```

- 위에서 살펴본 바와 같이, `var`는 `if, for` 등의 코드 블록을 관통합니다.
- 아주 오래전의 자바스크립트에선 블록 수준 렉시컬 환경이 만들어 지지 않았기 때문입니다.
- `var`는 구식 자바스크립트의 잔재이죠.

<br>

## `var`는 재선언이 허용

- 만약에 전역 스코프에 동일한 변수를 `let`으로 선언한 하면 에러가 발생합니다.

```js
let user;
let user; // SyntaxError: 'user' has already been declared
```

<br>

- 만약 `var`로 위에 처럼 선언 할 경우 기존에 선언한 것을 무시하고 덮어 씁니다.

```js
var user = "Pete";

var user = "John"; // this "var" does nothing (already declared)
// ...it doesn't trigger an error

alert(user); // John
```

<br>

## 선언 하기 전에 사용 할 수 있는 `var`

- `var` 선언은 함수가 시작될 때 처리됩니다.
- 전역에서 선언한 변수라면 스크립트가 시작될 때 처리되죠.
- 함수 본문 내에서 `var`로 선언한 변수는 선언 위치와 상관없이 함수 본문이 시작되는 지점에서 정의됩니다(단, 변수가 중첩 함수 내에서 정의되지 않아야 이 규칙이 적용됩니다).

```js
function sayHi() {
  phrase = "Hello";

  alert(phrase);

  var phrase; //
}
sayHi();
```

```js
// var phrase가 위로 이동한 것처럼 말이죠.

function sayHi() {
  var phrase;

  phrase = "Hello";

  alert(phrase);
}
sayHi();
```

```js
// 코드 블록은 무시되기 때문에, 아래 코드 역시 동일하게 동작합니다.

function sayHi() {
  phrase = "Hello"; // (*)

  if (false) {
    var phrase;
  }

  alert(phrase);
}
sayHi();
```

- 이렇게 변수가 끌어올려 지는 현상을 `'호이스팅(hoisting)'`이라고 부릅니다.
- `var`로 선언한 모든 변수는 함수의 최상위로 ‘끌어 올려지기(hoisted)’ 때문입니다.
- 바로 위 예제에서 `if (false)` 블록 안 코드는 절대 실행되지 않지만, 이는 호이스팅에 전혀 영향을 주지 않습니다.
- `if` 내부의 `var` 는 함수 `sayHi`의 시작 부분에서 처리되므로 (\*)로 표시한 줄에서 `phrase`는 이미 정의가 된 상태인 것이죠.
- 선언은 호이스팅 되지만 할당은 호이스팅 되지 않습니다.

<br>

```js
// var phrase = "Hello"행에선 두 가지 일이 일어납니다.
// 1. 변수 선언(var)
// 2. 변수에 값을 할당(=)

function sayHi() {
  alert(phrase);

  var phrase = "Hello";
}

sayHi();

// 변수 선언은 함수 실행이 시작될 때 처리되지만(호이스팅) 할당은 호이스팅 되지 않기 때문에 할당 관련 코드에서 처리됩니다.
// 따라서 위 예제는 아래 코드처럼 동작하게 됩니다.

function sayHi() {
  var phrase; // 선언은 함수 시작 시 처리됩니다.

  alert(phrase); // undefined

  phrase = "Hello"; // 할당은 실행 흐름이 해당 코드에 도달했을 때 처리됩니다.
}

sayHi();
```

- 이처럼 모든 `var 선언`은 함수 시작 시 처리되기 때문에, `var`로 선언한 변수는 어디서든 참조할 수 있습니다.
- 하지만 변수에 무언가를 할당하기 전까진 값이 `undefined`이죠.
- 바로 위의 두 예시에서 `alert`를 호출하기 전에 변수 phrase는 선언이 끝난 상태이기 때문에 에러 없이 얼럿 창이 뜹니다.
- 그러나 값이 할당되기 전이기 때문에 얼럿 창엔 undefined가 출력되죠.

<br>

### 즉시 실행 함수 표현식
