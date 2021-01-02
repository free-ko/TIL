# '??' Operator

- a ?? b의 의미는
- a가 null도 아니고 undefined도 아니면 a
- 그 외의 경우는 b

```js
let firsName = null;
let lastName = null;
let nickName = "freeko";

// 첫 번째 인자가 null or undefined이면 두 번째 인자 값을 첫 번째 인자에 넣음
alert(firstName ?? lastName ?? nickname ?? "Supercoder");
```

<br>

### '??'와 '||'의 차이

- || 는 첫 번째 truthy 값을 반환합니다.
- ?? 는 첫 번째 정의된 값을 반환 합니다.
- 안정성 이유 때문에 ??는 &&나 ||를 함께 사용하지 못합니다.

```js
let height = 0;

// height에 0을 할당 했지만 0을 falsy 한 값으로 취급 했기 때문에
// null 이나 undefined를 할당한 것과 동일하게 처리 합니다.
// 그래서 결과 값은 100입니다.
alert(height || 100);

// height의 값이 정확하게 null 이나 undefined일 경우에만 100이 됩니다.
alert(hegith ?? 100); // 0

// ??와 &&, || 함께 사용하기 위해서는 괄호를 사용합니다.
let x = (1 && 2) ?? 3;
alert(x); // 2
```
