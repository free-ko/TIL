# getElement*, querySelector*로 요소 검색하기

- 요소들이 가까이 붙어있다면 앞서 학습한 `DOM` 탐색 프로퍼티를 사용해 목표 요소에 쉽게 접근할 수 있습니다.
- 그런데, 요소들이 가까이 붙어있지 않은 경우도 있기 마련입니다.
- 상대 위치를 이용하지 않으면서 웹 페이지 내에서 원하는 요소 노드에 접근하는 방법은 없는 걸까요?

## document.getElementById 혹은 id를 사용해 요소 검색하기

- 요소에 `id` 속성이 있으면 위치에 상관없이 메서드 `document.getElementById(id)`를 이용해 접근할 수 있습니다.

```html
<div id="elem">
  <div id="elem-content">Element</div>
</div>

<script>
  // 요소 얻기
  let elem = document.getElementById("elem");

  // 배경색 변경하기
  elem.style.background = "red";
</script>
```

- 이에 더하여 `id` 속성값을 그대로 딴 전역 변수를 이용해 접근할 수도 있습니다.

```html
<div id="elem">
  <div id="elem-content">Element</div>
</div>

<script>
  // 변수 elem은 id가 'elem'인 요소를 참조합니다.
  elem.style.background = "red";

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

- `id`에 대응하는 전역변수는 명세서의 내용을 구현해 만들어진 것으로 표준이긴 하지만 하위 호환성을 위해 남겨둔 동작입니다.
- 브라우저는 스크립트의 네임스페이스와 `DOM`의 네임스페이스를 함께 사용할 수 있도록 해서 개발자의 편의를 도모합니다.
- 그런데 이런 방식은 스크립트가 간단할 땐 괜찮지만, 이름이 충돌할 가능성이 있기 때문에 추천하는 방식은 아닙니다.
- `HTML`을 보지 않은 상황에서 코드만 보고 변수의 출처를 알기 힘들다는 단점도 있습니다.
- 간결성을 위해 요소의 출처가 명확한 경우, `id`를 사용해 요소에 직접 접근하는 방법을 사용할 예정입니다.
- 실무에선 `document.getElementById`를 사용하시길 바랍니다.

### id는 중복되면 안 됩니다.

- `id`는 유일무이해야 합니다. 문서 내 요소의 `id` 속성값은 중복되어선 안 됩니다.
- 같은 `id`를 가진 요소가 여러 개 있으면 `document.getElementById`같이 `id`를 이용해 요소를 검색하는 메서드의 동작이 예측 불가능해집니다.
- 검색된 여러 요소 중 어떤 요소를 반환할지 판단하지 못해 임의의 요소가 반환되죠. 문서 내 동일 `id`가 없도록 해 이런 일을 방지하도록 합시다.

### `anyNode.getElementById`가 아닌 `document.getElementById`

- `getElementById`는 `document` 객체를 대상으로 해당 `id`를 가진 요소 노드를 찾아 줍니다. 문서 노드가 아닌 다른 노드엔 호출할 수 없습니다.

<br>

## `querySelectorAll`

- `elem.querySelectorAll(css)`은 다재다능한 요소 검색 메서드입니다.
- 이 메서드는 `elem`의 자식 요소 중 주어진 `CSS` 선택자에 대응하는 요소 모두를 반환합니다.
- 아래 예시는 마지막 `<li>`요소 모두를 반환합니다.

```html
<ul>
  <li>1-1</li>
  <li>1-2</li>
</ul>
<ul>
  <li>2-1</li>
  <li>2-2</li>
</ul>
<script>
  let elements = document.querySelectorAll("ul > li:last-child");

  for (let elem of elements) {
    alert(elem.innerHTML); // "1-2", "2-2"
  }
</script>
```

- `querySelectorAll`은 `CSS` 선택자를 활용할 수 있다는 점에서 아주 유용합니다.

### 가상 클래스도 사용할 수 있습니다.

- `querySelectorAll`에는 `:hover`나 `:active` 같은 `CSS` 선택자의 가상 클래스(pseudo-class)도 사용할 수 있습니다.
- `document.querySelectorAll(':hover')`을 사용하면 마우스 포인터가 위에 있는(hover 상태인) 요소 모두를 담은 컬렉션이 반환됩니다.
- 이때 컬렉션은 `DOM` 트리 최상단에 위치한 `<html>`부터 가장 하단의 요소 순으로 채워집니다.

<br>

## querySelector

- `elem.querySelector(css)`는 주어진 CSS 선택자에 대응하는 요소 중 첫 번째 요소를 반환합니다.
- `elem.querySelectorAll(css)[0]`과 동일하죠. `elem.querySelectorAll(css)[0]`은 선택자에 해당하는 모든 요소를 검색해 첫 번째 요소만을 반환하고, `elem.querySelector`는 해당하는 요소를 찾으면 검색을 멈춘다는 점에서 차이가 있습니다.
- `elem.querySelector`가 더 빠른 이유이죠. `querySelector`는 `querySelectorAll`에 비해 코드의 길이가 짧다는 장점도 있습니다.

<br>

## matches

- 지금까지 소개한 모든 메서드는 DOM 검색에 쓰입니다.
- `elem.matches(css)`는 `DOM`을 검색하는 일이 아닌 조금 다른 일을 합니다.
- 이 메서드는 요소 `elem`이 주어진 CSS 선택자와 일치하는지 여부를 판단해줍니다. 일치한다면 `true`, 아니라면 `false`를 반환하죠.
- 요소가 담겨있는 배열 등을 순회해 원하는 요소만 걸러내고자 할 때 유용합니다.

```html
<a href="http://example.com/file.zip">...</a>
<a href="http://ya.ru">...</a>

<script>
  // document.body.children가 아니더라도 컬렉션이라면 이 메서드를 적용할 수 있습니다.
  for (let elem of document.body.children) {
    if (elem.matches('a[href$="zip"]')) {
      alert("주어진 CSS 선택자와 일치하는 요소: " + elem.href);
    }
  }
</script>
```

<br>

## closest

- 부모 요소, 부모 요소의 부모 요소 등 DOM 트리에서 특정 요소의 상위에 있는 요소들은 조상(ancestor) 요소라고 합니다.
- 메서드 `elem.closest(css)`는 `elem` 자기 자신을 포함하여 CSS 선택자와 일치하는 가장 가까운 조상 요소를 찾을 수 있게 도와줍니다.
- `closest`메서드는 해당 요소부터 시작해 DOM 트리를 한 단계씩 거슬러 올라가면서 원하는 요소를 찾습니다.
- CSS 선택자와 일치하는 요소를 찾으면, 검색을 중단하고 해당 요소를 반환합니다.

```html
<h1>목차</h1>

<div class="contents">
  <ul class="book">
    <li class="chapter">1장</li>
    <li class="chapter">2장</li>
  </ul>
</div>

<script>
  let chapter = document.querySelector(".chapter"); // LI

  alert(chapter.closest(".book")); // UL
  alert(chapter.closest(".contents")); // DIV

  alert(chapter.closest("h1")); // null(h1은 li의 조상 요소가 아님)
</script>
```

<br>

## getElementsBy\*

- 태그나 클래스 등을 이용해 원하는 노드를 찾아주는 메서드도 있습니다.
- `querySelector`를 이용하는 게 더 편리하고 문법도 짧아서, 요즘은 이런 메서드들을 잘 쓰진 않습니다.
- `elem.getElementsByTagName(tag)` – 주어진 태그에 해당하는 요소를 찾고, 대응하는 요소를 담은 컬렉션을 반환합니다. 매개변수 `tag`에 `"\*"`이 들어가면, '모든 태그’가 검색됩니다.
- `elem.getElementsByClassName(className)` – `class` 속성값을 기준으로 요소를 찾고, 대응하는 요소를 담은 컬렉션을 반환합니다.
- `document.getElementsByName(name)` – 아주 드물게 쓰이는 메서드로, 문서 전체를 대상으로 검색을 수행합니다. 검색 기준은 `name` 속성값이고, 이 메서드 역시 검색 결과를 담은 컬렉션을 반환합니다.

```js
// 문서 내 모든 div를 얻습니다.
let divs = document.getElementsByTagName("div");
```

- 다음 예시는 표 안의 모든 `input` 태그를 찾습니다.

```html
<table id="table">
  <tr>
    <td>나이:</td>

    <td>
      <label>
        <input type="radio" name="age" value="young" checked /> 18세 미만
      </label>
      <label>
        <input type="radio" name="age" value="mature" /> 18세 이상, 60세 미만
      </label>
      <label>
        <input type="radio" name="age" value="senior" /> 60세 이상
      </label>
    </td>
  </tr>
</table>

<script>
  let inputs = table.getElementsByTagName("input");

  for (let input of inputs) {
    alert(input.value + ": " + input.checked);
  }
</script>
```

### 's'를 절대 빠트리지 마세요!

<br>

[출처]
https://ko.javascript.info/searching-elements-dom
