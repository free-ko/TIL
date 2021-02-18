# `new Function` 문법

- 함수 표현식과 함수 선언문 이외에 함수를 만들 수도 있는 방법이 하나 더 있습니다. 잘 사용하는 방법은 아니지만, 이 방법 외에는 대안이 없을 때 사용합니다

## 문법

- `new Function` 문법을 사용하면 함수를 만들 수 있습니다.

```js
let func = new Function([arg1, arg2, ...argN], functionBody);

// 새로 만들어지는 함수는 인수 arg1...argN과 함수 본문 functionBody로 구성됩니다.
```

- 인수 두 개가 있는 함수를 직접 만들어 보면서 new Function 문법에 대해 이해해보도록 합시다.

```js
let sum = new Function("a", "b", "return a + b");

alert(sum(1, 2)); // 3
```

- 인수가 없고 함수 본문만 있는 함수를 만들어보겠습니다.

```js
let sayHi = new Function('alert("Hello")');

sayHi(); // Hello
```

<br>

- 기존에 사용하던 방법과 `new Function`을 사용해 함수를 만드는 방법의 가장 큰 차이는 런타임에 받은 문자열을 사용해 함수를 만들 수 있다는 점입니다.
- 함수 표현식과 함수 선언문은 개발자가 직접 스크립트를 작성해야만 함수를 만들 수 있었죠.
- 그러나 `new Function`이라는 문법을 사용하면 어떤 문자열도 함수로 바꿀 수 있습니다.
- 서버에서 전달받은 문자열을 이용해 새로운 함수를 만들고 이를 실행하는 것도 가능합니다.

```js
let str = ... 서버에서 동적으로 전달받은 문자열(코드 형태) ...

let func = new Function(str);
func();

// 서버에서 코드를 받거나 템플릿을 사용해 함수를 동적으로 컴파일해야 하는 경우, 복잡한 웹 애플리케이션을 구현할 때와 같이 아주 특별한 경우에 new Function을 사용할 수 있습니다.
```

<br>

## 클로저

[출처]
https://ko.javascript.info/new-function
