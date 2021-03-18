# 함수의 prototype 프로퍼티

- `new F()`와 같은 `생성자 함수`를 이용하면 새로운 객체를 만들 수 있다는 걸 앞서 배운 바 있습니다.
- 그런데 `F.prototype`이 객체면 `new` 연산자는 `F.prototype`을 사용해 새롭게 생성된 객체의 `[[Prototype]]`을 설정합니다.
- `F.prototype`에서 `"prototype"`은 `F`에 정의된 일반 프로퍼티라는 점에 주의해 주시기 바랍니다.
- 앞서 배웠던 ‘프로토타입’ 객체와 같아 보이지만 `F.prototype`에서 `"prototype"`은 이름만 같은 일반 프로퍼티입니다.

<br>

## 주의

- 자바스크립트가 만들어졌을 때는 `프로토타입 기반 상속`이 주요 기능 중 하나였습니다.
- 그런데 과거엔 프로토타입에 직접 접근할 방법이 없었습니다.
- 그나마 믿고 사용할 수 있었던 방법은 이번 챕터에서 설명할 생성자 함수의 `"prototype"` 프로퍼티를 이용하는 방법뿐이었죠.
- 많은 스크립트가 아직 이 방법을 사용하는 이유가 여기에 있습니다

<br>

```js
let animal = {
  eats: true,
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal

alert(rabbit.eats); // true

// Rabbit.prototype = animal은 "new Rabbit을 호출해 만든 새로운 객체의 [[Prototype]]을 animal로 설정하라."라는 것을 의미합니다.
```

<br>

### `F.prototype`은 `new F`를 호출할 때만 사용됩니다.

- `F.prototype` 프로퍼티는 `new F`가 호출될 때만 사용됩니다.
- `new F`를 호출해 새롭게 만든 객체의 `[[Prototype]]`을 할당해 주죠.
- 새로운 객체가 만들어진 후에 `F.prototype` 프로퍼티가 바뀌면`(F.prototype = <another object>) new F`로 만들어지는 새로운 객체는 또 다른 객체()를 `[[Prototype]]`으로 갖게 됩니다.
- 다만, 기존에 있던 객체의 `[[Prototype]]`은 그대로 유지됩니다.

<br>
[출처]
https://ko.javascript.info/function-prototype
