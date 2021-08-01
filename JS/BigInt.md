# BigInt

- `BigInt`는 길이의 제약 없이 정수를 다룰 수 있게 해주는 숫자형입니다.
- 정수 리터럴 끝에 `n`을 붙이거나 함수 `BigInt`를 호출하면 문자열이나 숫자를 가지고 `BigInt` 타입의 값을 만들 수 있습니다.

```js
const bigint = 1234567890123456789012345678901234567890n;

const sameBigint = BigInt("1234567890123456789012345678901234567890");

const bigintFromNumber = BigInt(10); // 10n과 동일합니다.
```

<br>

## 수학 연산자

- `BigInt`는 대개 일반 숫자와 큰 차이 없이 사용할 수 있습니다.

```js
alert(1n + 2n); // 3

alert(5n / 2n); // 2
```

- 위 예시에서 나눗셈 연산 `5/2`의 결과엔 소수부가 없다는 점에 주의하시기 바랍니다.
- `BigInt`형 값을 대상으로 한 연산은 `BigInt`형 값을 반환합니다.
- `BigInt`형 값과 일반 숫자를 섞어서 사용할 순 없습니다.

```js
alert(1n + 2); // Error: Cannot mix BigInt and other types
```

- 일반 숫자와 섞어서 써야 하는 상황이라면 `BigInt()`나 `Number()`를 사용해 명시적으로 형 변환을 해주면 됩니다.

```js
let bigint = 1n;
let number = 2;

// 숫자를 bigint로
alert(bigint + BigInt(number)); // 3

// bigint를 숫자로
alert(Number(bigint) + number); // 3
```

- 형 변환과 관련된 연산은 항상 조용히 동작합니다.
- 절대 에러를 발생시키지 않죠.
- 그런데 `bigint`가 너무 커서 숫자형에서 허용하는 자릿수를 넘으면 나머지 비트는 자동으로 잘려 나갑니다.
- 이런 점을 염두하고 형 변환을 해야 합니다.

### 단항 덧셈 연산자는 bigint에 사용할 수 없습니다.

- 단항 덧셈 연산자 `+value`를 사용하면 `value`를 손쉽게 숫자형으로 바꿀 수 있습니다.
- 그런데 혼란을 방지하기 위해 `bigint`를 대상으로 하는 연산에선 단항 덧셈 연산자를 지원하지 않습니다.

```js
let bigint = 1n;

alert(+bigint); // 에러
```

- `bigint`를 숫자형으로 바꿀 때는 `Number()`를 사용해야 합니다.

## 비교 연산자

<br>

[출처]
https://ko.javascript.info/bigint
