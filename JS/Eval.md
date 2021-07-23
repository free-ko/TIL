# Eval: 문자열 코드 실행하기

- 내장 함수 `eval`을 사용하면 문자열 형태의 코드를 실행할 수 있습니다.

```js
let result = eval(code);
```

```js
let code = 'alert("Hello")';
eval(code); // Hello
```

- 길이가 긴 문자열이 코드가 될 수 있는데, 여기엔 줄 바꿈, 함수 선언, 변수 등이 포함될 수도 있습니다.
- 마지막 구문의 결과가 `eval`의 결과가 됩니다.

```js
let value = eval("1+1");
alert(value); // 2
```

```js
let value = eval("let i = 0; ++i");
alert(value); // 1
```

- `eval`로 둘러싼 코드는 현재 렉시컬 환경에서 실행되므로 외부 변수에 접근할 수 있습니다.

```js
let a = 1;

function f() {
  let a = 2;

  eval("alert(a)"); // 2
}

f();
```

- 외부 변수를 변경하는 것도 가능하죠.

```js
let x = 5;
eval("x = 10");
alert(x); // 10, 변경된 값
```

- 엄격 모드에서 `eval`은 자체 렉시컬 환경을 갖고 있습니다.
- 따라서 `eval` 내부에 선언된 함수와 변수는 외부에서 읽을 수 없습니다.

```js
// 참고: 실행 가능한 모든 예시에 'use strict'가 적용되어있습니다.

eval("let x = 5; function f() {}");

alert(typeof x); // undefined (없는 변수)
// 함수 f도 읽을 수 없음
```

- `use strict`가 적용되어있지 않은 경우엔 `eval`은 자체 렉시컬 환경을 갖지 않기 때문에 외부에 있는 `x`와 `f`를 읽을 수 있습니다.

<br>

## ‘eval’ 사용하기

<br>

[출처]
https://ko.javascript.info/eval
