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

2.

<br>

[출처]
https://ko.javascript.info/instanceof
