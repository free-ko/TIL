# Proxy와 Reflect

- `Proxy`는 특정 객체를 감싸 프로퍼티 읽기, 쓰기와 같은 객체에 가해지는 작업을 중간에서 가로채는 객체로, 가로채진 작업은 `Proxy` 자체에서 처리되기도 하고, 원래 객체가 처리하도록 그대로 전달되기도 합니다.
- 프락시는 다양한 라이브러리와 몇몇 브라우저 프레임워크에서 사용되고 있습니다.

<br>

## Proxy

```js
let proxy = new Proxy(target, handler);
```

- `target` : 감싸게 될 객체로, 함수를 포함한 모든 객체가 가능합니다.
- `handler` : 동작을 가로채는 메서드인 `'트랩(trap)'`이 담긴 객체로, 여기서 프락시를 설정합니다(예시: `get` 트랩은 `target`의 프로퍼티를 읽을 때, `set` 트랩은 `target`의 프로퍼티를 쓸 때 활성화됨).
- `proxy`에 작업이 가해지고, `handler`에 작업과 상응하는 트랩이 있으면 트랩이 실행되어 프락시가 이 작업을 처리할 기회를 얻게 됩니다.
- 트랩이 없으면 `target`에 작업이 직접 수행됩니다.
- 먼저, 트랩이 없는 프락시를 사용한 예시를 살펴봅시다.

```js
let target = {};
let proxy = new Proxy(target, {}); // 빈 핸들러

proxy.test = 5; // 프락시에 값을 씁니다. -- (1)
alert(target.test); // 5, target에 새로운 프로퍼티가 생겼네요!

alert(proxy.test); // 5, 프락시를 사용해 값을 읽을 수도 있습니다. -- (2)

for (let key in proxy) alert(key); // test, 반복도 잘 동작합니다. -- (3)
```

<br>

[출처]
https://ko.javascript.info/proxy
