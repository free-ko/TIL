# 'instanceof'로 클래스 확인하기

- `instanceof` 연산자를 사용하면 객체가 특정 클래스에 속하는지 아닌지를 확인할 수 있습니다.
- `instanceof`는 상속 관계도 확인해줍니다.
- 확인 기능은 다양한 곳에서 쓰이는데, `instanceof`를 사용해 인수의 타입에 따라 이를 다르게 처리하는 `다형적인(polymorphic)` 함수를 만드는데 사용해보겠습니다.

<br>

## instanceof 연산자

```js
obj instanceof Class;

// obj가 Class에 속하거나 Class를 상속받는 클래스에 속하면 true가 반환됩니다.

class Rabbit {}
let rabbit = new Rabbit();

// rabbit이 클래스 Rabbit의 객체인가요?
alert(rabbit instanceof Rabbit); // true
```

- `instanceof`는 생성자 함수에서도 사용할 수 있습니다.

```js
// 클래스가 아닌 생성자 함수
function Rabbit() {}

alert(new Rabbit() instanceof Rabbit); // true
```

- `Array` 같은 내장 클래스에도 사용할 수 있습니다.

```js
let arr = [1, 2, 3];
alert(arr instanceof Array); // true
alert(arr instanceof Object); // true
```

- 위 예시에서 `arr`은 클래스 `Object`에도 속한다는 점에 주목해주시기 바랍니다.
- `Array`는 프로토타입 기반으로 `Object`를 상속받습니다.
- `instanceof` 연산자는 보통, 프로토타입 체인을 거슬러 올라가며 인스턴스 여부나 상속 여부를 확인합니다.
- 그런데 정적 메서드 `Symbol.hasInstance`을 사용하면 직접 확인 로직을 설정할 수도 있습니다.
- `obj instanceof Class`은 대략 아래와 같은 알고리즘으로 동작합니다.

1. 클래스에 정적 메서드 `Symbol.hasInstance`가 구현되어 있으면, `obj instanceof Class`문이 실행될 때, `Class[Symbol.hasInstance](obj)`가 호출됩니다. 호출 결과는 `true`나 `false`이어야 합니다. 이런 규칙을 기반으로 `instanceof`의 동작을 커스터마이징 할 수 있습니다.

```js
// canEat 프로퍼티가 있으면 animal이라고 판단할 수 있도록
// instanceOf의 로직을 직접 설정합니다.
class Animal {
  static [Symbol.hasInstance](obj) {
    if (obj.canEat) return true;
  }
}

let obj = { canEat: true };

alert(obj instanceof Animal); // true, Animal[Symbol.hasInstance](obj)가 호출됨
```

2. 그런데, 대부분의 클래스엔 `Symbol.hasInstance`가 구현되어있지 않습니다. 이럴 땐 일반적인 로직이 사용됩니다. `obj instanceOf Class`는 `Class.prototype`이 `obj` 프로토타입 체인 상의 프로토타입 중 하나와 일치하는지 확인합니다.

```js
obj.__proto__ === Class.prototype?
obj.__proto__.__proto__ === Class.prototype?
obj.__proto__.__proto__.__proto__ === Class.prototype?
...
// 이 중 하나라도 true라면 true를 반환합니다.
// 그렇지 않고 체인의 끝에 도달하면 false를 반환합니다.
```

- 위 예시에서 `rabbit.__proto__ === Rabbit.prototype`가 `true`이기 때문에 `instanceof`는 `true`를 반환합니다.
- 상속받은 클래스를 사용하는 경우엔 두 번째 단계에서 일치 여부가 확인됩니다.

```js
class Animal {}
class Rabbit extends Animal {}

let rabbit = new Rabbit();
alert(rabbit instanceof Animal); // true

// rabbit.__proto__ === Rabbit.prototype
// rabbit.__proto__.__proto__ === Animal.prototype (일치!)
```

- 한편, `objA`가 `objB`의 프로토타입 체인 상 어딘가에 있으면 `true`를 반환해주는 메서드, `objA.isPrototypeOf(objB)`도 있습니다.
- `obj instanceof Class`는 `Class.prototype.isPrototypeOf(obj)`와 동일합니다.
- `isPrototypeOf`는 `Class` 생성자를 제외하고 포함 여부를 검사하는 점이 조금 특이합니다.
- 검사 시, 프로토타입 체인과 `Class.prototype`만 고려합니다.
- `isPrototypeOf`의 이런 특징은 객체 생성 후 `prototype` 프로퍼티가 변경되는 경우 특이한 결과를 초래하기도 합니다. 아래와 같이 말이죠.

```js
function Rabbit() {}
let rabbit = new Rabbit();

// 프로토타입이 변경됨
Rabbit.prototype = {};

// 더 이상 Rabbit이 아닙니다!
alert(rabbit instanceof Rabbit); // false
```

<br>

## 타입 확인을 위한 Object.prototype.toString

- 일반 객체를 문자열로 변화하면 `[object Object]`가 된다는 것을 알고 계실 겁니다.

```js
let obj = {};

alert(obj); // [object Object]
alert(obj.toString()); // 같은 결과가 출력됨
```

- 이렇게 `[object Object]`가 되는 이유는 `toString`의 구현방식 때문입니다.
- 그런데 `toString`엔 `toString`을 더 강력하게 만들어주는 기능이 숨겨져 있습니다.
- `toString`의 숨겨진 기능을 사용하면 확장 `typeof`, `instanceof`의 대안을 만들 수 있습니다.
- 아직 감이 안 잡히시겠지만, 구체적으로 설명하겠습니다.
- 명세서에 따르면, 객체에서 내장 `toString`을 추출하는 게 가능합니다.
  = 이렇게 추출한 메서드는 모든 값을 대상으로 실행할 수 있습니다. 호출 결과는 값에 따라 달라집니다.
- 숫자형 – `[object Number]`
- 불린형 – `[object Boolean]`
- `null` – `[object Null]`
- `undefined` – `[object Undefined]`
- 배열 – `[object Array]`
- 그외 – 커스터마이징 가능

- 예시:

```js
// 편의를 위해 toString 메서드를 변수에 복사함
let objectToString = Object.prototype.toString;

// 아래 변수의 타입은 무엇일까요?
let arr = [];

alert(objectToString.call(arr)); // [object Array]
```

- `call`을 사용해 컨텍스트를 `this=arr`로 설정하고 함수 `objectToString`를 실행하였습니다.
- `toString` 알고리즘은 내부적으로 `this`를 검사하고 상응하는 결과를 반환합니다. 예시를 더 살펴봅시다.

```js
let s = Object.prototype.toString;

alert(s.call(123)); // [object Number]
alert(s.call(null)); // [object Null]
alert(s.call(alert)); // [object Function]
```

<br>

### Symbol.toStringTag

<br>

[출처]
https://ko.javascript.info/instanceof
