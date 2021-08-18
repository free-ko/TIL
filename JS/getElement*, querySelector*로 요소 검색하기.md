# getElement*, querySelector*로 요소 검색하기

- 요소들이 가까이 붙어있다면 앞서 학습한 `DOM` 탐색 프로퍼티를 사용해 목표 요소에 쉽게 접근할 수 있습니다.
- 그런데, 요소들이 가까이 붙어있지 않은 경우도 있기 마련입니다.
- 상대 위치를 이용하지 않으면서 웹 페이지 내에서 원하는 요소 노드에 접근하는 방법은 없는 걸까요?

## document.getElementById 혹은 id를 사용해 요소 검색하기

- 요소에 `id` 속성이 있으면 위치에 상관없이 메서드 `document.getElementById(id)`를 이용해 접근할 수 있습니다.

```js
<div id="elem">
  <div id="elem-content">Element</div>
</div>

<script>
  // 요소 얻기
  let elem = document.getElementById('elem');

  // 배경색 변경하기
  elem.style.background = 'red';
</script>
```

- 이에 더하여 `id` 속성값을 그대로 딴 전역 변수를 이용해 접근할 수도 있습니다.

```js
<div id="elem">
  <div id="elem-content">Element</div>
</div>

<script>
  // 변수 elem은 id가 'elem'인 요소를 참조합니다.
  elem.style.background = 'red';

  // id가 elem-content인 요소는 중간에 하이픈(-)이 있기 때문에 변수 이름으로 쓸 수 없습니다.
  // 이럴 땐 대괄호(`[...]`)를 사용해서 window['elem-content']로 접근하면 됩니다.
</script>
```

- 그런데 이렇게 요소 `id`를 따서 자동으로 선언된 전역변수는 동일한 이름을 가진 변수가 선언되면 무용지물이 됩니다.

```js
<div id="elem"></div>

<script>
  let elem = 5; // elem은 더이상 <div id="elem">를 참조하지 않고 5가 됩니다.

  alert(elem); // 5
</script>
```

### id를 따서 만들어진 전역변수를 요소 접근 시 사용하지 마세요.

<br>

[출처]
https://ko.javascript.info/searching-elements-dom
