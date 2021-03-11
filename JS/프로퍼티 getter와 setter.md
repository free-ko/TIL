# 프로퍼티 getter와 setter

- 객체의 프로퍼티는 두 종류로 나뉩니다.

1. 첫 번째 종류는 `데이터 프로퍼티(data property)` 입니다.

- 지금까지 사용한 모든 프로퍼티는 데이터 프로퍼티입니다.
- 데이터 프로퍼티 조작 방법에 대해선 모두 알고 계실 것이라 생각합니다.

2. 두 번째는 `접근자 프로퍼티(accessor property)` 라 불리는 새로운 종류의 프로퍼티입니다.

- 접근자 프로퍼티의 본질은 함수인데, 이 함수는 값을 획득(`get`)하고 설정(`set`)하는 역할을 담당합니다.
- 그런데 외부 코드에서는 함수가 아닌 일반적인 프로퍼티처럼 보입니다.

<br>

## getter와 setter

- 접근자 프로퍼티는 `'getter(획득자)'`와 `‘setter(설정자)’` 메서드로 표현됩니다.
- 객체 리터럴 안에서 `getter`와 `setter` 메서드는 `get`과 `set`으로 나타낼 수 있습니다.

```js
let obj = {
  get propName() {
    // getter, obj.propName을 실행할 때 실행되는 코드
  },

  set propName(value) {
    // setter, obj.propName = value를 실행할 때 실행되는 코드
  },
};
```

- `getter` 메서드는 `obj.propName`을 사용해 프로퍼티를 읽으려고 할 때 실행되고, `setter` 메서드는 `obj.propName = value`으로 프로퍼티에 값을 할당하려 할 때 실행됩니다.
- 프로퍼티 `name`과 `surname`이 있는 객체 `user`를 만들어봅시다.

```js
let user = {
  name: "John",
  surname: "Smith",
};
```

- 이 객체에 `fullName` 이라는 프로퍼티를 추가해 `fullName`이 `'John Smith'`가 되도록 해봅시다.
- 기존 값을 복사-붙여넣기 하지 않고 `fullName`이 `'John Smith'`가 되도록 하려면 접근자 프로퍼티를 구현하면 됩니다.

```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },
};

alert(user.fullName); // John Smith
```

- 바깥 코드에선 접근자 프로퍼티를 일반 프로퍼티처럼 사용할 수 있습니다.
- 접근자 프로퍼티는 이런 아이디어에서 출발했습니다.
- 접근자 프로퍼티를 사용하면 함수처럼 호출 하지 않고, 일반 프로퍼티에서 값에 접근하는 것처럼 평범하게 `user.fullName`을 사용해 프로퍼티 값을 얻을 수 있습니다.
- 나머지 작업은 `getter` 메서드가 뒷단에서 처리해줍니다.
- 한편, 위 예시의 `fullName`은 `getter` 메서드만 가지고 있기 때문에 `user.fullName=`을 사용해 값을 할당하려고 하면 에러가 발생합니다.

```js
let user = {
  get fullName() {
    return `...`;
  },
};

user.fullName = "Test"; // Error (프로퍼티에 getter 메서드만 있어서 에러가 발생합니다.)
```

- `user.fullName`에 `setter` 메서드를 추가해 에러가 발생하지 않도록 고쳐봅시다.

```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  },
};

// 주어진 값을 사용해 set fullName이 실행됩니다.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper

// 이렇게 getter와 setter 메서드를 구현하면 객체엔 fullName이라는 '가상’의 프로퍼티가 생깁니다.
// 가상의 프로퍼티는 읽고 쓸 순 있지만 실제로는 존재하지 않습니다.
```

<br>

## 접근자 프로퍼티 설명자

[출처]
https://ko.javascript.info/property-accessors
