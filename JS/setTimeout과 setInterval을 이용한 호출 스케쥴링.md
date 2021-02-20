# setTimeout과 setInterval을 이용한 호출 스케줄링

- 일정 시간이 지난 후에 원하는 함수를 예약 실행(호출)할 수 있게 하는 것을 `'호출 스케줄링(scheduling a call)'`이라고 합니다.
- 호출 스케줄링을 구현하는 방법은 두 가지가 있습니다.
  1. `setTimeout`을 이용해 일정 시간이 지난 후에 함수를 실행하는 방법
  2. `setInterval`을 이용해 일정 시간 간격을 두고 함수를 실행하는 방법
- 자바스크립트 명세서엔 `setTimeout`과 `setInterval`가 명시되어있지 않습니다.
- 하지만 시중에 나와 있는 모든 브라우저, `Node.js`를 포함한 자바스크립트 호스트 환경 대부분이 이와 유사한 메서드와 내부 스케줄러를 지원합니다.

<br>

## setTimeout

```js
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)

```

`매개 변수`

- `func|code` : 실행하고자 하는 코드로, 함수 또는 문자열 형태입니다. 대개는 이 자리에 함수가 들어갑니다. 하위 호환성을 위해 문자열도 받을 수 있게 해놓았지만 추천하진 않습니다.
- `delay` : 실행 전 대기 시간으로, 단위는 밀리초(millisecond, 1000밀리초 = 1초)이며 기본값은 0입니다.
- `arg1, arg2…` : 함수에 전달할 인수들로, IE9 이하에선 지원하지 않습니다.

```js
function sayHi() {
  alert("안녕하세요.");
}

setTimeout(sayHi, 1000);
```

```js
function sayHi(who, phrase) {
  alert(who + " 님, " + phrase);
}

setTimeout(sayHi, 1000, "홍길동", "안녕하세요."); // 홍길동 님, 안녕하세요.
```

- `setTimeout`의 첫 번째 인수가 문자열이면 자바스크립트는 이 문자열을 이용해 함수를 만듭니다.
- 아래 예시가 정상적으로 동작하는 이유이죠.

```js
setTimeout("alert('안녕하세요.')", 1000);
```

- 그런데 이렇게 문자열을 사용하는 방법은 추천하지 않습니다. 되도록 다음 예시와 같이 익명 화살표 함수를 사용하세요.

```js
setTimeout(() => alert("안녕하세요."), 1000);
```

<br>

### 함수를 실행하지 말고 넘기세요.

- 초보 개발자는 `setTimeout`에 함수를 넘길 때, 함수 뒤에 `()`를 붙이는 실수를 하곤 합니다.

```js
// 잘못된 코드
setTimeout(sayHi(), 1000);

// setTimeout은 함수의 참조 값을 받도록 정의되어 있는데 sayHi()를 인수로 전달하면 함수 실행 결과가 전달되어 버립니다.
// 그런데 sayHi()엔 반환문이 없습니다.
// 호출 결과는 undefined가 되겠죠. 따라서 setTimeout은 스케줄링할 대상을 찾지 못해, 원하는 대로 코드가 동작하지 않습니다.
```

<br>

## clearTimeout으로 스케줄링 취소하기

- `setTimeout`을 호출하면 `'타이머 식별자(timer identifier)'`가 반환됩니다.
- 스케줄링을 취소하고 싶을 땐 이 식별자(아래 예시에서 timerId)를 사용하면 됩니다.
- 스케줄링 취소하기

```js
let timerId = setTimeout(...);
clearTimeout(timerId);
```

- 아래 예시는 함수 실행을 계획해 놓았다가 중간에 마음이 바뀌어 계획해 놓았던 것을 취소한 상황을 코드로 표현하고 있습니다.
- 예시를 실행해도 스케줄링이 취소되었기 때문에 아무런 변화가 없는 것을 확인할 수 있습니다.

```js
let timerId = setTimeout(() => alert("아무런 일도 일어나지 않습니다."), 1000);
alert(timerId); // 타이머 식별자

clearTimeout(timerId);
alert(timerId); // 위 타이머 식별자와 동일함 (취소 후에도 식별자의 값은 null이 되지 않습니다.)
```

- 예시를 실행하면 alert 창이 2개가 뜨는데, 이 얼럿 창을 통해 브라우저 환경에선 타이머 식별자가 숫자라는 걸 알 수 있습니다.
- 다른 호스트 환경에선 타이머 식별자가 숫자형 이외의 자료형일 수 있습니다.
- 참고로 `Node.js`에서 `setTimeout`을 실행하면 타이머 `객체`가 반환합니다.
- 다시 한번 말씀드리자면, 스케줄링에 관한 명세는 따로 존재하지 않습니다.
- 명세가 없기 때문에 호스트 환경마다 약간의 차이가 있을 수밖에 없습니다.
- 참고로 브라우저는 `HTML5`의 `timers section`을 준수하고 있습니다.

<br>

[출처]
https://ko.javascript.info/settimeout-setinterval
  
