# Method와 This

- 객체는 사용자(user), 주문(order) 등과 같이 실제 존재하는 개체(entity)를 표현하고자 할 때 생성됩니다.
- 사용자는 현실에서 장바구니에서 물건 선택하기, 로그인하기, 로그아웃 하기 등의 행동을 합니다.
- 이와 마찬가지로 사용자를 나타내는 객체 user도 특정한 행동을 할 수 있습니다.

<br>

## 메서드 만들기

- 객체 프로퍼티에 할당된 함수를 Method라고 부릅니다.

```js
let user = {
  name: "Free",
  age: 30,
};

user.sayHi = function () {
  alert("Hi");
};

// 선언된 함수를 메서드로 등록
user.sayHi = sayHi;

user.sayHi(); // Hi
```

<br>

### 객체 지향 프로그래밍

- 객체를 사용하여 개체를 표현하는 방식을 `객체 지향 프로그래밍(Object-Oriented Programming, OOP)`이라고 부릅니다.
- 올바른 개체를 선택하는 방법, 개체 사이의 상호작용을 나타내는 방법 등에 관한 의사결정은 객체 지향 설계를 기반으로 이뤄집니다.

<br>

## 메서드와 This

- 메서드는 객체에 저장된 정보에 접근할 수 있어야 제 역할을 할 수 있습니다.
- 모든 메서드가 그런 건 아니지만, 대부분의 메서드가 객체 프로퍼티의 값을 활용합니다.
- `user.sayHi()`의 내부 코드에 객체 `user`에 저장된 이름(nmae)을 이용해 인사말을 만드는 경우가 이런 경우에 속합니다.
- <b>메서드 내부에서 `this` 키워드를 사용하면 객체에 접근 할 수 있습니다.</b>

```js
let user = {
  name: "free",
  age: 30,

  sayHi() {
    // this는 '현재 객체'를 나타냅니다.
    // this 대신 'user'를 이용 가능합니다.
    alert(this.name);
  },
};

user.sayHi(); // 실행 되는 동안 this는 user를 나타냅니다.
```

<b>

- 그런데 이렇게 외부 변수를 사용해 객체를 참조하면 예상치 못한 에러가 발생 할 수 있습니다.
- `user`를 복사해 다른 변수에 할당(admin = user)하고, `user`는 전혀 다른 값으로 덮어 썼다고 가정해 봅시다.
- sayHi()는 원치 않는 값(null)을 참조 할 겁니다.

```js
let user = {
  name: "free",
  age: 30,

  sayHi() {
    // Error: Cannot read property 'name'of null
    alert(user.naem);

    // 대신 alert함수가 user.name 대신 this.name을 인수로 받았다면 에러가 발생하지 않았을 겁니다.
    alert(this.name);
  },
};

let admin = user;
user = null; // user를 Null로 덮어 씁니다.
admin.sayHi(); // sayHi()가 엉뚱한 객체를 참고하면서 에러가 발생했습니다.
```

<br>

## 자유로운 This

- JS의 `this`는 다른 프로그래밍 언어의 `this`와 동작 방식이 다릅니다.
- JS에선 모든 함수의 `this`를 사용 할 수 있습니다.
- `this` 값은 런타임에 결정됩니다.

```js
// 문법 에러가 발생하지 않습니다.
function sayHi() {
  alert(this.name);
}
```

- 컨텍스트에 따라 달라집니다.
- 동일한 함수라도 다른 객체에서 호출 했다면 `this`가 참조하는 값이 달라집니다.

```js
let user = { name: "Free" };
let admin = (name: "Admin");

function sayHi() {
  alert(this.name);
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // Free  (this == user)
admin.f(); // Admin (this == admin)

admin["f"](); // Admin (점과 대괄호는 동일하게 동작함)

// obj.f()를 호출했다면 this는 f를 호출하는 동안의 obj입니다.
// 위 예시에선 obj가 user나 admin을 참조합니다.
```

<br>

### 객체 없이 호출 하기 : this == undefined

- 객체가 없어도 함수를 호출 할 수 있습니다.
- 밑에 코드를 엄격모드(use strict)에서 실행하면, `this`엔 `undefined`가 할당됩니다.
- `this.name`으로 name에 접근 하려고 하면 에러가 발생합니다.
- 그런데 엄격모드가 아닐 때는 `this`가 전역 객체를 참조 합니다.
- 브라우저 환경에선 `window`라는 전역 객체를 참조합니다.
- 이런 동작 차이는 `use strict`가 도입된 배경이기도 합니다.
- 밑의 코드는 대개 실수로 작성된 경우가 많습니다.
- 함수 본문에 `this`가 사용되었다면, 객체 컨텍스트 내에서 함수를 호출할 것이라고 예상하시면 됩니다.

```js
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

### 자유로운 `this`가 만드는 결과

- 다른 언어를 사용하다 JS로 넘어온 개발자는 `this`를 혼동하기 쉽습니다.
- `this`는 항상 메서드가 정의된 객체를 참조할 것이라고 착각합니다.
- 이런 개념을 bound `this`라고 합니다.
- JS에서 `this`는 런타임에 결정됩니다.
- 메서드가 어디서 정의되었는지에 상관없이 `this`는 '점 앞의' 객체가 무엇인가에 따라 자유롭게 결정됩니다.
- 이렇게 `this`가 런타임에 결정되면 좋은 점도 있고 나쁜 점도 있습니다.
- 함수(메서드)를 하나만 만들어 여러 객체에서 재사용할 수 있다는 것은 장점이지만, 이런 유연함이 실수로 이어질 수 있다는 것은 단점입니다.
- JS가 `this`를 다루는 방식이 좋은지, 나쁜지는 우리가 판단할 문제가 아닙니다.
- 개발자는 `this`의 동작 방식을 충분히 이해하고 장점을 취하면서 실수를 피하는 데만 집중하면 됩니다.

<br>

## this가 없는 화살표 함수

- 화살표 함수는 일반 함수와 달리 '고유한' `this`를 가지지 않습니다.
- 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 평범한 외부 함수에서 `this`값을 가져 옵니다.
- 밑에 코드의 `arrow()`의 `this`는 외부 함수 `user.sayHi()`의 `this`가 됩니다.

```js
let user = {
  firstName: "ko",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  },
};

user.sayHi(); // ko
```

- 별개의 `this`가 만들어지는 건 원하지 않고, 외부 컨텍스트에 있는 `this`를 이용하고 싶은 경우 화살표 함수가 유용합니다.

<br>

# 요약

- 객체 프로퍼티에 저장된 함수를 `메서드`라고 부릅니다.
- `object.doSomthing()`은 객체를 '행동'할 수 있게 해줍니다.
- 메서드는 `this`로 객체를 참조합니다.
- `this`값은 런타임에 결정됩니다.
- 함수를 선언 할 때 `this`를 사용할 수 있습니다. 다만, 함수가 호출되기 전까지 `this`엔 값이 할당되지 않습니다.
- 함수를 복사해 객체 간 전달 할 수 있습니다.
- 함수를 객체 프로퍼티에 저장해 `object.method()`같이 메서드 형태로 호출하면 `this`는 `object`를 참조 합니다.
- 화살표 함수는 자신만의 `this`를 가지지 않는 다른 점에서 독특합니다.
- 화살표 함수 안에서 `this`를 사용하면, 외부에서 `this` 값을 가져 옵니다.

<br>

[출처]
https://ko.javascript.info/object-methods
