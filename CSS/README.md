## Cascading Style Sheet

# 🌈 Syntax

```jsx
selector {
	property: value;
}
```

<br/>
<br/>
<br/>

# 🌈 Inline

- 흐름이라고 생각하면 됩니다.
- cf) Block 은 영역이라고 생각하면 됩니다.
- 그렇기 때문에 Inline은 padding-top,bottom, margin-top,bottom을 사용하게 되면 흐름에 위배 되기 때문에 무시됩니다.
- 다만 흐름에 지장을 안주는 padding-right,left, margint-left,right는 괜찮습니다.
- cf) Block은 영역이기 때문에 padding, margin다 사용 했을 때 자기만의 영역을 가집니다.

<br/>
<br/>
<br/>

# 🌈 Inline-Block

- Inline + Block 특징을 합친 것 입니다.

<br/>
<br/>
<br/>

# 🌈 Float ⭐️

- Block 들의 가로 배치 하기 위해 사용합니다.

1. 집 나간 자식 처리

```jsx
<div class="parants">
  <div class="child red">red</div>
  <div class="child blue">blue</div>
  <div class="child green">green</div>
</div>

// 만약에 자식인 red에 float를 설정하게 되면
// 부모 태그는 red가 집나갔다고 생각하고
// red의 자리를 없애 버립니다.
// 그 대신에 blue와 green이 그 자리를 차지 하게 되며 빈 자리는 없애버립니다.
// 또한 float를 적용받은 red는 떠 있다고 생각하시면 됩니다.
```

2. Block으로 신분 상승

- float를 적용하면 display가 Block으로 바뀝니다.

3. 블록인데 길막을 못해!!

- 원래 Block은 따로 width 선언하지 않을 경우, 부모의 width가 적용됩니다.
- 하지만 float를 적용 받게 되면 안에 있는 Content의 길이 만큼만 width가 적용됩니다.

4. 또한 Block일 때는 옆에 자동으로 margin이 채워 졌지만 float 적용시 margin이 채워지지 않습니다.

- 위에 코드를 보았을 때 만약에 blue도 float를 적용 시키게 되면 기존의 red와 겹치기 때문에
- 어떻게든 red와 겹치지 않도록 하기 위해서 red 옆으로 이동합니다.
- 또한 green도 float를 적용시키면 red, blue와 겹치지 않기 위해 다른 곳으로 배치 하게 됩니다.
- 그런데 이때 모든 child가 float 적용이 되면서 parant는 height가 0이 되어서 Layout의 문제가 생깁니다.

5. 만약에 Inline인 span, text 같은게 오게 되면 float도 Inline이여서 인지를 합니다.

### 😎How to Fix : Float 때문에 엉망이 된 Layout 해결 하기

<br/>

✅ `overflow : hidden;`

- Float 속성 때문에 집 나간 자식을 부모 태그가 자식 태그를 찾게 됩니다.

✅`ClearFix`

- CSS의 속성인 Clear은 Float의 속성 때문에 Layout이 망가진 부분을 해결하기 위해 탄생 했습니다.
- Flaot 적용이 된 태그의 위치를 인지 할 수 있도록 해줍니다. ex) `clear : left`
- Clear는 Block인 요소에만 적용이 됩니다.
- `clear : left , right , both`의 속성을 가질 수 있습니다.

✅`Pseudo-Element`

- 자식 태그가 Float를 적용해 가로 정렬을 할 경우 부모 태그에 높이를 부여해 전체 레이아웃을 잡아야 합니다.
- 그 때 CSS만을 통해 가상의 영역을 만들어 Clear 속성을 부여 하게 되면 부모 태그가 높이를 가집니다.

```html
<body>
  <div class="parent">
    <div class="childe">Child</div>
    <div class="childe">Child</div>
  </div>
</body>

<style>
  .parent::after{  // 가상의 마지막 childe 태그 영역을 만듭니다.
  	content: ''
  	display: block;
  	clear: left;
  }
  .childe{
  	float:left
  }
</style>
```

```html
<body>
  <div class="pseudo">Red</div>
  <div class="pseudo">Yellow</div>
  <div class="pseudo">Blue</div>
</body>

<style>
  <!-가상영역을 설정 할 때 반드시 Content Property를 넣어야 합니다.->
  	.pseudo::before{
  		content: "⭐️"
  	}
  	.pseudo::after{
  		content: ""
  		display: inline-block;
  		width: 10px;
  		height: 20px;
  		backgorund-color: skyblue;
  	}
</style>
```

<br/>
<br/>
<br/>

# 🌈 Float 실습

- CSS : margin을 사용 할 때 bottom, top 혼잡하게 쓰는 게 아니라 하나만 정해서 사용합니다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Float 2</title>
    <link
      href="https://fonts.googleapis.com/css?family=Noto+Sans+KR&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="card clearfix">
      <img src="./assets/user.png" alt="Customer support" class="card-user" />
      <div class="card-content">
        <h1>RE: 안녕하세요 배송 관련 문의드립니다</h1>
        <strong> customer support </strong>
        <p>안녕하세요 우현님. 문의 드린 사항에 대한 답변드립니다. 지난 12...</p>
      </div>
    </div>
  </body>
</html>
```

```css
* {
  box-sizing: border-box; /* box-size의 기준을 box의 테두리를 기준으로 합니다. 이렇게 했을 때 테두리의 두께를 예측 해서 테두리를 포함한 크기를 지정할 수 있습니다. */
  margin: 0; /* Block에 자동적으로 부여된 margin 값을 제거 합니다. */
}

body {
  height: 100vh;
  background-color: black;
  font-family: "Noto Sans KR", sans-serif;
  letter-spacing: -0.02em;
}

h1 {
  font-size: 16px;
  font-weight: 400;
  color: #1f2d3d;
  line-height: 1.25;
}

strong {
  font-size: 14px;
  font-weight: 400;
  color: #afb3b9;
  line-height: 1.4285714286;
}

p {
  font-size: 16px;
  color: #79818b;
  line-height: 1.5;
}

/* ▼ WHERE YOUR CODE BEGINS */

/* float를 적용한 부모 태그에게 공통적으로 적용시키기 위해 만들었습니다.(자식 태그가 float했음을 인지 시켜주는 코드 입니다. 이렇게 되면 부모 태그는 자식태그를 포함한 width,height를 찾게 됩니다. */
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}

.card {
  max-width: 556px; /* 이 숫자보다 적게 했을 때 자식 태그들이 float 적용이 안됩니다. */
  padding: 20px;
  background-color: white;
}

.card-user,
.card-content {
  float: left;
}

.card-user {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  margin-right: 20px;
}

.card-content {
  margin-bottom: 4px;
}

.card-content strong {
  display: block; /* strong 태그가 inline이기 때문에 margin이 적용이 안됩니다. 그래서 display:block을 적용시켰습니다. (이 부분은 개발 도구를 통해 발견했습니다.)  */
  margin-bottom: 12px;
}
```

<br/>
<br/>
<br/>

# 🌈 Position

- 요소를 자유롭게 이동시키기 위해 사용 합니다.
- 속성 값으로는 `static, relative, absolute, fixed, sticky` 가 있습니다.
- 반드시 생각해야 할 부분
  - 내가 어떤 Position을 사용 하고 있는지
  - 내가 사용 하고자 하는 Position은 무엇을 기준으로 하고 있는지 ( Position Type에 따라 기준점이 달라집니다. )

## 1. Static

- 모든 요소의 기본 값

## 2. Relative

- 이동의 기준 점은 바로 본래 있던 자신의 위치 입니다. 그 위치를 기준으로 해서 위, 아래, 좌, 우를 이동 할 수 있습니다.
- Float와 마찬가지로 부모 태그를 떠나 붕 뜨게 됩니다.
- 단 Float와 다르게 Layout이 붕괴되거나, 다른 태그들에게 영향을 끼치지 않습니다.
- 왜냐하면 Float 처럼 붕 떠도 자신의 위치를 기억하고 있습니다. 뿐만아니라 부모, 형제 태그들도 그 위치를 기억 합니다. ( 형제 태그들이 relative 적용된 태그의 위치로 이동하거나 그러지 않습니다. )

```css
.red {
	position: relative
	top: 20px; /* top을 기준으로 밑으로 20px 아래로 이동됩니다. */
	right: 20px; /* right을 기준으로 right으로 부터 왼쪽으로 20px 떨어 집니다. */
}
```

## 3. Absolute

- Float를 적용했을 때와 마찬가지로 똑같은 현상이 발생합니다.
  - 예를 들어 : 집 나간 내 새끼, 찾을 길 없네 / 블록으로 신분 상승 / 길막을 못해 슬픈 블록아! / 다른 요소 무시하고 그냥 혼자 붕 떠있음
- Float의 속성을 부여 받은 태그는 부모의 태그에 항상 종속적으로 갇혀 있었는데
- Absolute 자신이 기준을 선택 함 ( 그 기준은 position이 static이 아닌 요소를 기준으로 자기 자신을 위치 시킵니다. 왠만하면 relative를 적용 시켜서 기준을 잡는다. )
- 빨강이는 부모가 아닌 조부모의 position이 absolute가 있기 때문에 그것을 기준으로 이동 함

## 4. Fixed

- 기존의 absolut 적용했을 때랑 같습니다.
- 단 "Viewport"(내가 보고 있는 웹 화면)을 기준으로 자신의 위치를 표현 합니다.

## 5. Z-Index

- 수직 레벨을 지정 할 때 사용 합니다.

<br/>
<br/>
<br/>

# 🌈 Position 실습 1

```css
<style.css파일>

// 이미지는 inline 요소임에도 불구하고 width와 height을 줄수 있는 이유는
// 이미지 자체 width와 height가 존재 하기 때문입니다.
// 그래서 width, height을 적용할 수 있는 대상은 block 이기 때문에
// 일부로 display:block 이라고 명시를 해줍니다.
.user-photo img {
	display: block
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

// position을 주게 되면 자동적으로 display가 block이 되어서 width, height을 부여 할 수 있습니다.
// top, bottom & left, right 각 1개씩 사용하는 것을 추천
.user-status {
  position: absolute;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  border: 2px solid #fff;
  background-color: #21df91;
}
```

<br/>
<br/>
<br/>

# 🌈 Position 실습 2

```css
<style.css > .card {
  width: 480px;
}

.card-carousel img {
  disply: block; // img는 inline 이기에 width를 부여 할 수 없지만 자신이 가지는 width가 있기에 조절 가능합니다. 헤깔림을 방지 하기 위해 작성 했습니다.
  width: 100%; // 부모의 width에 무조건 100% 따르겠다는 의미 입니다. 이미지 사이즈 조절 할 때 자주 사용하니 기억하면 좋습니다.
  height: auto;
}

.card-carousel {
  position: relative;
}

#prev,
#next {
  position: absolute;
  top: 50%; // 기준으로 잡은 relative의 top 기준으로 50% 내려옵니다. 그러나 50%의 내려온 지점을 기준으로 내려 오기 때문에 정확한 가운데 정렬이 아닙니다.
  transform: translateY(
    -50%
  ); // 그래서 transform을 통해 자신을 기준으로 50% 만큼 올립니다. 그러면 가운데 정렬히 정확하게 됩니다.
}

#prev {
  left: 0;
}

#next {
  right: 0;
}

.card-content strong {
  display: block;
  margin-top: 8px;
  text-align: right; // inline을 위치 이동 시킬 때 사용합니다.
}
```

<br/>
<br/>
<br/>

# 🌈 Position 실습 3

```css
<style.css > .modal {
  margin-bottom: 4px;
  padding: 32px 40px;

  // position:fixed을 적용 하게 되면 일단 modal이 width 값이 선언되어 있지 않기 때문에
  // 가지고 있는 content 내용만큼 줄어 듭니다.
  // 또한 밑에 처럼 50% 씩 이동 할 시 50% 이동된 꼭지점 기준으로 배치 되어지기 때문에
  // 자신의 영역만큼 다시 이동해 주어야 합니다.
  // 즉 자신의 영역의 50% 만큼 식 top,left을 이동시킵니다.
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.modal-title,
.modal-desc {
  // modal-title만 가운데 정렬이 됩니다. 그 이유는 modal-desc는 p태그 즉 display:block, 이기때문에
  // 심지어 밑에 width를 설정함으로 옆에 margin이 자동적으로 생성되어 있습니다.
  // 그래서 text-align:center 해도 가운데 정렬이 안됩니다.
  // 이럴 때에는 밑에 처럼 margin:0 auto;를 하게 되면 양 옆 margin 값을 균등하게 부여 함으로 가운데 정렬이 됩니다.
  text-align: center;
}

.modal-desc {
  width: 590px;
  margin: 0 auto;
}

// input-group 안에 있는 태그들이 inline이기 때문에
// text-align: center;을 적용하면 가운데 정렬이 적용 됩니다.
.input-group {
  text-align: center;
}

// input이 자연스럽게 display: inline-block으로 지정되어 있습니다.
// inline-block을 사용할 경우 알 수 없는 오차가 발생하기때문에 주의 해야 합니다.
.input-group input {
  width: 200px;
  height: 36px;
  padding: 0 16px;
  border-radius: 4px;
  border: none;
  background-color: #f6f8fa;
}

.input-group button {
  height: 36px;
  padding: 0 14px;
  border: none;
  border-radius: 4px;
  background-color: #2860e1;
  color: #fff;
}

// 우리는 close-button을 .modal을 기준으로 움직일려고 합니다.
// 그 기준 설정을 위해서 그 기준이 position이 static일 경우 position:relative를 적용 시켜야 합니다.
// 그러나 현재 .modal position:fixed이기 때문에 굳이 position:relative할 필요가 없습니다.
.close-button {
  position: absolute;
  top: 8px;
  right: 8px;
}
```

<br/>
<br/>
<br/>

# 🌈 FlexBox

### ⭐️정렬의 끝판 왕⭐️

### ❗️FlexBox 사용 할 때에는 4가지만 생각 해주면 됩니다.

- 나, FlexBox 사용 쓸 거임(단호)
- 가로 정렬? 세로 정렬?
- 무조건 한 줄 안에 다 정렬?
- 신나는 FlexBox 파티 타임!!

1. 나, FlexBox 사용 쓸 거임(단호)

```css
.flexbox {
  display: flex; /* flex, inline-flex */
}
```

- 누구에게 FlexBox를 적용 해야 하나??

2. 가로 정렬? 세로 정렬?

```css
.flexbox {
  display: flex;
  flex-direction: row; /* row | row-reverse | column | column-reverse */
}

/* flex-direction을 설정할 경우 Axis(축)이 변경 된다는 사실을 알아야 합니다. */
```

3. 무조건 한 줄에 다 정렬?

```css
.flexbox {
  display: flex;
  flex-direction: row;

  /* 모든 요소들을 한 줄에 다 정렬 할 것인지, 아니면 상황에 따라 여러 줄을 만들어서 정렬한 것인지 설정하는 것 입니다. */
  flex-wrap: nowrap; /* nowrap | wrap */
}
```

- 부모의 width: 600px 일때 자식들의 flex-direction:row 할 경우
- `flex-wrap: nowrap`을 하게 되면 부모의 width 600px에 맞게 자식 태그 width 고려하지 않고 한 줄로 무조건 정렬 되기 때문에 자식의 width가 줄어 듭니다.

- 그러나 `flex-wrap: wrap`을 하게 되면 자식의 width을 고려하고 한 줄로 정렬이 됩니다.

<br/>
<br/>
<br/>

# 🌈 FlexBox - 실습1

- Float 실습 - 1 의 코드를 Flex 적용하기

```css
/* Flex를 적용하고 싶은 태그의 부모태그에게 Flex에게 적용을 합니다. */
/* 위에서 아래로 과정을 따라서 코드를 작성했습니다. */
/* 가로 정렬, 세로 정렬 할 것인지 정해줍니다. */
/* 한 줄에 다 표현 할 것인지 정합니다. */
/* 각 아이템 들의 간격을 정합니다. */

.tab-menu {
  display: flex;
  flex-direction: row; /* 기본 값이여서 안 적어도 무방합니다. */
  flex-wrap: nowrap; /* 기본 값이여서 안 적어도 무방합니다. */
  justify-content: flex-start; /* 기본 값이여서 안 적어도 무방합니다. */
  align-items: center;
  border-bottom: 1px solid #e5eaef;
  background-color: #f2f2f2;
  max-width: 540px;
}
```

- Float 실습 - 2 의 코드를 Flex 적용하기

```css
.card {
  display: flex;
  justify-content: space-between;
  max-width: 556px;
  padding: 20px;
  background-color: white;
}
```

<br/>
<br/>
<br/>

# 🌈 FlexBox - 실습2

```css
body {
  background-color: black;
  width: 100%;
  height: 100vh;

  /* 안에 있는 content를 가운데 정렬 하기 위해 설정했습니다. */
  /* justify-content는 main-axis(flex-direction에 따라 달라집니다. 현재 row입니다.)에 따라 달라집니다. */
  /* 현재 row 방향에서 justify-content: ceneter이기 때문에 가운데 일단 옵니다. */
  /* 하지만 height을 지정해서 그 속에 있는 item들의 값들을 가운데로 정렬해주는 것이 align-items: center 입니다. */
  /* 그리고 밑에 profile의 flex-direction: column이기 때문에 align-itmes 기준이 column이 됩니다.*/
  display: flex;
  justify-content: center;
  align-items: center;
}

.profile {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 368px;
  padding: 32px 40px;
  background-color: #f2f2f2;
  border-radius: 16px;
}

.profile-image {
  display: flex;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-bottom: 16px;
}

.profile-name {
  margin-bottom: 4px;
}

.profile-location {
  margin-bottom: 32px;
}

.profile-detail {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%; /* 이 코드를 적용하지 않을 경우 서로 겹치는 현상이 일어 납니다. */
  text-align: center;
}
```

- align-itmes, justify-content 참고 사이트

[A Complete Guide to Flexbox | CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
<br/>
<br/>
<br/>

# 🌈 Media Query

- 반응형 웹을 만들기 위해서 반드시 알아야 하는 CSS
- 또한 HTML에서는 `viewport meta` / CSS에서는 `media query`가 존재해야 반응형 웹을 구현할 수 있습니다.
- 브라우저의 화면 크기를 viewport 라고 합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width" />
  </head>
</html>
```

```css
@media screen add (min-width: 768px) {
  /* 768px 이상일 때 밑에 코드 적용합니다. */
  /* Where all the magic happens... */
}
```

- 실습

```css
/*
    * RED #ff4949
    * ORANGE #ff5216
    * YELLOW #ffc82c
    * GREEN #13ce66
    * BLUE #1fb6ff
    * PURPLE #7e5bef
    * PINK #ff49db
*/

* {
  box-sizing: border-box;
  margin: 0;
}

body {
  font-family: "Popins", sans-serif;
  color: #212529;
}

/* Viewport height
	ex) 1vh : Viewport 전체 높이 중 1%
*

.box {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  background-color: #ff4949;
  font-size: 30px;
  font-weight: 700;
  color: #fff;
}

.box::after {
  content: "Mobile";
}

@media screen and (min-width: 576px) {
  /* CSS 선언 */
  .box {
    background-color: #7e5bef;
  }

  .box::after {
    content: "Landscape Phone";
  }
}

@media screen and (min-width: 768px) {
  .box {
    background-color: #ffc82c;
  }

  .box::after {
    content: "Tablet";
  }
}

@media screen and (min-width: 768px) and (max-width: 991px) {
  .box {
    background-color: #ff5216;
  }

  .box::after {
    content: "Max";
  }
}

@media screen and (min-width: 992px) {
  .box {
    background-color: #13ce66;
  }

  .box::after {
    content: "Desktop";
  }
}

@media screen and (min-width: 1200px) {
  .box {
    background-color: #1fb6ff;
  }

  .box::after {
    content: "Large Desktop";
  }
}

@media screen and (min-width: 1366px) {
  .box {
    background-color: #ff49db;
  }

  .box::after {
    content: "Super Large Desktop";
  }
}
```

<br/>
<br/>
<br/>

# 🌈 Media Query - 실습

- Media Query 작업 할 때 작은 것 부터 작업 하는게 좋음 ( 모바일 부터 )
- iPhone5 Width, Height가 가장 작은 크기를 가지고 있기 때문에 모바일 작업에 기준으로 삼습니다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Media Query</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@700;900&family=Poppins:wght@700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <aside class="banner">
      <h1 class="banner-title">
        <a href="#"> 🚗 모집 임박! 8월 게스트 신청하기 </a>
      </h1>
    </aside>

    <section class="landing">
      <h1 class="landing-title">
        <strong lang="en">Eat, pray, work</strong>
        FREEKO<br />
        디지털노마드 민박<br />
        #치앙마이<br />
        #8월예약오픈
      </h1>
      <a href="#" class="landing-link"> 민박 둘러보기 </a>
    </section>
  </body>
</html>
```

```css
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  font-family: "Noto Sans KR", sans-serif;
  letter-spacing: -0.01em;
}

a {
  text-decoration: none;
}

.landing {
  background-image: url("https://raw.githubusercontent.com/rohjs/bugless-101/master/css-basic/media-query/templates/assets/img-bg.jpg");
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
}

.landing-title {
  line-height: 1.25;
  letter-spacing: -0.03em;
  color: #fff;
}

.landing-title strong {
  display: block;
  font-family: "Poppins", sans-serif;
  letter-spacing: -0.01em;
}

.landing-link {
  line-height: 1;
  color: #fff;
}

.banner-title a {
  color: #1f2d3d;
}

/* ▼ WHERE YOUR CODE BEGINS */

.banner {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #ffc82c;
}

.banner-title a {
  font-size: 18px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 64px;
}

.landing {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-end;
  width: 100%;
  height: 100vh;
  padding: 0 20px;
}

.landing-title {
  margin-bottom: 24px;
  text-align: right;
}

.landing-link {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 160px;
  height: 52px;
  font-size: 15px;
  border-radius: 16px;
  border: 2px solid #fff;
  background-color: rgba(0, 0, 0, 0.5);
}

@media screen and (min-width: 768px) {
  .banner {
    top: 0;
    bottom: auto; /* 기존에 사용했던 bottom 값을 리셋하기 위해 auto를 사용 합니다. */
  }

  .banner-title a {
    height: 80px;
  }

  .landing {
    align-items: center;
  }

  .landing-title {
    margin-bottom: 32px;
    font-size: 50px;
    text-align: center;
  }

  .landing-link {
    width: 180px;
    height: 56px;
    font-size: 18px;
  }
}
```

<br/>
<br/>
<br/>

# 🌈 Typography

- Typography를 설정 할 때 필수적으로 부여해야 하는 Essentials 한 것들을 다루겠습니다.
- 자주 사용 하지 않지만 알아두면 좋은 속성들에 대해 알아 보겠습니다.

- Font-size : px(절대 단위), em & rem(상대 단위)
- em : 실제로 적용된 폰트 사이즈

```css
p {
  font-size: 24px;
  width: 1em; /* 1em = 24px 기존에 부여된 크기를 기준으로 삼습니다. */
}
```

- rem : root(HTML) + em
- HTML에 적용된 Font-size를 1rem으로 봅니다.

```css
html {
  font-size: 24px;
}

p {
  font-size: 2rem; /* 24px X 2 = 48px 입니다. */
}
```

- line-height : 줄 간격을 뜻합니다. ( 이때에도 px, em, rem을 사용 할 수 있습니다. )
- line-height를 얼마나 주던지 간에 Text는 줄 간격에 항상 가운데 정렬이 됩니다

  ‼️수직으로 무언가를 정렬 할 때 Line-height를 사용 하는 방법이 있습니다.

```css
p {
  font-size: 1rem;
  line-height: 1.5; /* em을 생략 할 수 있습니다. */
}
```

- Letter-spacing : 글자 글자 사이의 간격을 가리킵니다. ( 사용 할 수 있는 단위는 px, em이 있습니다. )

```css
p {
  letter-spacing: 2em; /* 여기서는 em을 생략하면 안됩니다. */
}
```

- Font-family

```css
.text {
  font-family: "Poppins";
  font-family: "Poppins", sans-serif; /* Poppins 서체가 없을 경우 sans-srif 계열의 모든 서체를 사용 하겠다는 의미 입니다.*/
  font-family: "Poppins", "Roboto", sans-serif;
}
```

- Font-weight : Font의 굵기 입니다.

  400, 700을 주로 사용 합니다.

- Color
- hex, rgb, rgba를 통해 Font에 색상을 적용 할 수 있습니다.

```css
p {
  color: #0066ff; /* rgb(0,102,255)로도 표현 가능 합니다. 또한 rgba(0, 102, 255, 1)로도 표현 가능합니다. a는 투명도를 의미 합니다. */
}
```

<br/>
<br/>

# 🌈 Typography - 2

- 중요하지는 않지만 알아두면 좋을 것들을 정리하겠습니다.
- Text-align : left, right, center
- Text-indent(들여쓰기) : +- 100px 등
- Text-transform : none, capitalize(문자의 앞 부분을 대문자로 변경합니다.), uppercase, lowercase
- Text-decoration(줄을 끊는 것과 관련된 속성 입니다.) : none, underline, line-through, overline ex) a 택의 underline을 제거할 때 많이 사용합니다.
- Font-style : normal, italic, oblique
- `<em>` 이 태그 안에 글자를 작성하면 italic 이 적용되어서 보여 줍니다.

<br/>
<br/>
<br/>

# 🌈 WebFont

- 우리가 프로젝트를 작업 할 당시 특별한 Font를 사용 할 경우 사용자에게도 그 Font를 제공해야 합니다.
- 첫 번째 `갖다 쓴다`
- 두 번째 `직접 제공 한다`

```css
<styles.css파일>

@import url("./font.css");

body {
  font-family: "Freeko", sans-serif;
  line-height: 1.65;
  color: #212529;
}

.box {
  width: 100%;
  max-width: 540px;
  padding: 120px 0;
  margin: 0 auto;
}

.box h1 {
  margin-bottom: 1.25em;
  color: #1f2d3d;
  line-height: 1.4;
}

.box p {
  color: #3c4858;
}
```

```css
<font.css파일>

/* 나만의 폰트를 만드는 방법 입니다. */
@font-face {
  font-family: "Freeko";
  font-style: normal;
  font-weight: 400;
  src: url("./assets/fonts/NanumSquareR.eot");
  src: url("./assets/fonts/NanumSquareR.eot?#iefix") format("embedded-opentype"),
    url("./assets/fonts/NanumSquareR.woff2") format("woff2"),
    url("./assets/fonts/NanumSquareR.woff") format("woff"), url("./assets/fonts/NanumSquareR.ttf")
      format("truetype");
}
```

<br/>
<br/>
<br/>

# 🌈 Typography Library 제작

<br/>

- 클래스 명도 이해하기 쉽게 작성합니다.
- 글자 크기 뿐만 아니라 자간 및 글자 간의 간격까지 함께 작성 합니다.

```css
/* ▼ WHERE YOUR CODE BEGINS */

body {
  font-family: "Noto Sans KR", sans-serif;
  letter-spacing: -0.015em;
}

/* Font Size */

.fs-tiny {
  font-size: 12px;
  line-height: 1.3333333333;
}

.fs-small {
  font-size: 14px;
  line-height: 1.4285714286;
}

.fs-base {
  font-size: 16px;
  line-height: 1.5;
}

.fs-medium {
  font-size: 18px;
  line-height: 1.5555555556;
}

.fs-large {
  font-size: 20px;
  line-height: 1.6;
}

.fs-h2 {
  font-size: 28px;
  line-height: 1.4285714286;
}

.fs-h1 {
  font-size: 34px;
  line-height: 1.4117647059;
}

/* Font Weight */

.fw-light,
.fw-300 {
  font-weight: 300;
}

.fw-regular,
.fw-400 {
  font-weight: 400;
}

.fw-medium,
.fw-500 {
  font-weight: 500;
}

.fw-bold,
.fw-700 {
  font-weight: 700;
}

/* Color */

.text-dark {
  color: #1f2d3d;
}

.text-primary {
  color: #3c4858;
}

.text-secondary {
  color: #8492a6;
}

.text-tertiary {
  color: #c0ccda;
}

.text-white {
  color: #fff;
}

.text-success {
  color: #13ce66;
}

.text-error {
  color: #ff5216;
}

.text-info {
  color: #009eeb;
}
```

<br/>

```html
<index.html파일>
  <!DOCTYPE html>
  <html lang="ko">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Typography</title>
      <link
        href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;regular;500;700&display=swap"
        rel="stylesheet"
      />
      <link rel="stylesheet" href="./typography.css" />
      <link rel="stylesheet" href="./style.css" />
    </head>
    <body>
      <div class="container">
        <h1 class="fs-h1 fw-light text-dark">
          버그가 너무 많아
          <br />
          김 버그
        </h1>

        <h3 class="fs-base fw-bold text-dark">
          주니어 개발자의 성장 드라마, 김버그
        </h3>
        <p class="fs-base fw-regular text-primary">
          속에서 같은 주며, 가지에 그들의 우리의 때문이다. 이 길지 가는 있는
          같이, 있다. 그들에게 그들은 길지 보내는 얼마나 동력은 칼이다. 청춘
          위하여 같은 새 노래하며 풀이 청춘을 천자만홍이 풍부하게 칼이다. 목숨이
          사는가 그들에게 안고, 위하여서, 타오르고 말이다. 많이 자신과 얼마나
          없으면 몸이 이것은
          <span class="text-error">품고 있으랴?</span>
        </p>

        <h3 class="fs-base fw-bold text-dark">앞으로의 계획</h3>

        <p class="fs-base fw-regular text-primary">
          끓는 봄바람을 끝까지 심장은 사는가 끓는 철환하였는가? 찾아다녀도,
          우리는 청춘에서만 능히 행복스럽고 바로 위하여서. 반짝이는 광야에서
          가지에 커다란 것이 청춘은 그들은 봄바람이다. 날카로우나 피가 얼마나
          얼마나 기쁘며, 밥을 끓는 자신과 귀는 말이다. 그림자는 황금시대의
          현저하게 곳으로 소리다.이것은 쓸쓸하랴? 가진 인간의 옷을 생명을 창공에
          그들의 얼마나 살 따뜻한 힘있다. 현저하게 투명하되 웅대한 대한 것이다.
        </p>

        <h2 class="fs-h2 fw-regular text-dark">
          프론트엔드 개발, 첫 단추를 꿰다
        </h2>

        <h3 class="fs-base fw-bold text-dark">HTML, CSS</h3>
        <p class="fs-base fw-regular text-primary">
          속에서 같은 주며, 가지에 그들의 우리의 때문이다. 이 길지 가는 있는
          같이, 있다. 그들에게 그들은 길지 보내는 얼마나 동력은 칼이다. 청춘
          위하여
          <strong class="fw-bold">같은 새 노래하며</strong> 풀이 청춘을
          천자만홍이 풍부하게 칼이다. 목숨이 사는가 그들에게 안고, 위하여서,
          타오르고 말이다. 많이 자신과 얼마나 없으면 몸이 이것은 품고 있으랴?
        </p>
      </div>
    </body>
  </html></index.html파일
>
```

<br/>

```css
<style.css파일>

/* ▼ WHERE YOUR CODE BEGINS */

* {
  box-sizing: border-box;
  margin: 0;
}

.container {
  width: 100%;
  max-width: 740px;
  padding: 48px;
  margin: 0 auto;
}

h1 {
  margin-bottom: 48px;
}

h2 {
  margin-bottom: 24px;
}

h3 {
  margin-bottom: 8px;
}

p {
  margin-bottom: 32px;
}
```

</br>
</br>
</br>

# 🌈 Background

## 1. Background-Color : hex , rgb, rgba

## 2. Background-image : url(' ')

## 3. Background-repeat : repeat(기본 값), no-repeat

## 4. Background-size : contain, cover, custom(width, height)

```css
/* Background-size : Custom */

.box {
  background-size: 100px 200px;
}
```

## 5. Background-position : x축, y축

<br/>
<br/>
<br/>

# 🌈 Background - 실습

- `img` 을 사용하지 않고 `div` 안에 `Background-image` 를 사용 하는 이유는 사용자들이 다양한 이미지를 올릴 경우 이미지 마다 가로와 세로 길이가 다 다릅니다.
- 그렇기 때문에 우리가 이미지를 사용자가 올릴 경우 틀을 제공해서 동일한 이미지가 올라 올 수 있도록 해줘야 합니다.
- CSS를 사용 할 때에는 사용해야 하는 논리가 있어야 합니다.

```html
<index.html파일>
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Background</title>
      <link
        href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500&display=swap"
        rel="stylesheet"
      />
      <link rel="stylesheet" href="./style.css" />
    </head>
    <body>
      <article class="card">
        <div class="card-image">
          <button
            type="button"
            class="like-button"
            aria-label="Like this property"
          ></button>
          <!-- <img src="./assets/img-house.jpg" alt="Seoul AirBnB, hosted by Woohyeon Roh" /> -->
        </div>

        <div class="card-content">
          <header class="card-header">
            <div class="property-type">
              <strong class="plus-badge">Plus</strong>
              <span>Entire apartment</span>
            </div>

            <div class="property-rate">
              <strong aria-label="Review: 4.97"> 4.97 </strong>
              <span aria-label="Total 203 reviews">(203)</span>
            </div>
          </header>

          <h1 class="card-title">
            Unwind in a Bright Space with Rustic Accents
          </h1>

          <div class="card-detail">
            <dl class="property-detail">
              <div>
                <dt class="sr-only">Rooms and beds</dt>
                <dd>
                  <span>2 guests</span>
                  <span>1 bedroom</span>
                  <span>1 bed</span>
                  <span>1 bath</span>
                </dd>
              </div>

              <div>
                <dt class="sr-only">Amenities</dt>
                <dd>
                  <span>Wifi</span>
                  <span>Kitchen</span>
                </dd>
              </div>
            </dl>
          </div>
        </div>
      </article>
    </body>
  </html></index.html파일
>
```

```css
<sytle.css파일 > .like-button {
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}

/* ▼ WHERE YOUR CODE BEGINS */

* {
  box-sizing: border-box;
  margin: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

button {
  border: none;
}

/* Global 하게 사용 하는 Class Name 입니다. */
/* 눈이 불편하신 분들을 위해 html에 정보를 넣고 웹 상에서는 보이지 않도록 하기 위함입니다.*/
/* display: none;을 할 경우 html에서도 무시 하기 때문에 사용은 하지 않습니다. */
.sr-only {
  position: absolute;
  z-index: -1;
  width: 1px;
  height: 1px;
  overflow: hidden;
  visibility: hidden;
}

.card {
  display: flex;
  width: 840px;
  padding: 24px;
  background-color: #fff;
}

.card-image {
  position: relative;
  width: 300px;
  height: 200px;
  border-radius: 6px;
  margin-right: 24px;
  background-image: url("https://raw.githubusercontent.com/rohjs/bugless-101/master/css-basic/background/assets/img-house.jpg");
  background-position: center center;
  background-repeat: no-repeat;
  background-size: cover;
}

.like-button {
  position: absolute;
  top: 12px;
  left: 12px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: #fff;
  background-image: url("https://raw.githubusercontent.com/rohjs/bugless-101/master/css-basic/background/assets/icon-favorite.svg");
  background-size: 24px 24px;
  background-repeat: no-repeat;
  background-position: center center;
  cursor: pointer;
}

/* 
flex-grow CSS property 는 flex-item 요소가, flex-container 요소 내부에서 
할당 가능한 공간의 정도를 선언합니다. 
만약 형제 요소로 렌더링 된 모든 flex-item 요소들이 동일한 flex-grow 값을 갖는다면, 
flex-container 내부에서 동일한 공간을 할당받습니다. 
하지만 flex-grow 값으로 다른 소수값을 지정한다면, 그에 따라 다른 공간값을 나누어 할당받게 됩니다.
*/
.card-content {
  flex-grow: 1;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.plus-badge {
  display: inline-block;
  padding: 1px 8px;
  border-radius: 4px;
  margin-right: 8px;
  font-size: 14px;
  line-height: 1.4285714286;
  color: #fff;
  text-transform: uppercase;
  background-color: #92174d;
}

.property-type span {
  font-size: 16px;
  line-height: 1.25;
  color: #7d858f;
}

.property-rate {
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

/* 정보가 아니기 때문에 CSS로 처리 합니다. */
.property-rate::before {
  content: "";
  display: block;
  width: 16px;
  height: 16px;
  margin-right: 4px;
  background-image: url("https://raw.githubusercontent.com/rohjs/bugless-101/master/css-basic/background/assets/icon-star.svg");
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center center;
}

.property-rate {
  font-size: 16px;
  line-height: 1.25;
  color: #7d858f;
}

.property-rate strong {
  margin-right: 2px;
  font-weight: 400;
  color: #151b26;
}

.card-title {
  margin-bottom: 16px;
  font-size: 20px;
  font-weight: 400;
  line-height: 1.6;
  color: #151b26;
}

.property-detail {
  font-size: 14px;
  line-height: 1.1428571429;
  color: #7d858f;
}

.property-detail div:first-child {
  margin-bottom: 8px;
}

/* 가상요소는 일반적으로 inline입니다. */
.property-detail dd span::after {
  content: "·";
  margin: 0 6px;
}

.property-detail dd span:last-child::after {
  content: "";
  margin: 0;
}
```

<br/>
<br/>
<br/>

# 🌈 Transition

- 속성 : property, duration, [timing-function], [delay]
- 제일 먼저 선언 되어야 하는 요소는 property 입니다.
- duration : 지속 시간 입니다. (ms, s, 1000ms == 1s)
- timing-function : transition 변화의 속도를 지정합니다.
- ease-in(처음에만 천천히), ease-out(나중에 천천히), ease-in-out, cubic-bezier()
- delay :

```css
.box {
  transition: all 2s ease-in;
  transition: font-size 1000ms ease-out, background-color 2000ms cubic-bezier(
        0.08,
        0.57,
        0.97,
        -0.78
      ) 1000ms;
}
```

<br/>
<br/>
<br/>

# 🌈 Animation

- Animation vs Transition ( 속성이 전환 )
- Animation의 예시

```css
.animation {
  animation: animation-name 1000ms ease-in-out 500ms infinite;
}
```

- Animation Name : @keyframes를 통해 Animation을 정의 합니다.

```css
@keyframes name {
  from {
    /* Rules */
  }
  to {
    /* Rules */
  }
}

@keyframes name {
  0% {
    /* Rules */
  }
  50% {
    /* Rules */
  }
  100% {
    /* Rules */
  }
}
```

- Animation Duration : 지속시간 입니다.
- Timing-Function : ease-in, ease-out, ease-in-out, cubic-bezier()
- Delay : Delay후에 작동 합니다.
- Iteration-count : 반복 횟수를 의미 합니다.
- Direction : Animation의 진행 방향을 의미 합니다. ( From - To )

```css
.box {
  position: relative;
  width: 300px;
  height: 300px;
  background: #0066ff; /* 애니메이션이 끝나고 기본값이 있어야 됩니다. */
  animation-name: move-box;
  animation-duration: 1000ms;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite; /* Animation이 무한대로 반복 됩니다. */
  animation-direction: alternat; /* 자연스럽게 애니메이션이 끝나고 되돌아 갑니다. */
}

@keyframes move-box;
 {
  from {
    top: 0;
    background-color: #0066ff;
  }
  to {
    top: 200px;
    background-color: #ff4949;
  }
}
```

<br/>
<br/>
<br/>

# 🌈 Transition 훈련

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Transition</title>
    <link rel="stylesheet" href="style.css" />
    <link
      href="https://fonts.googleapis.com/css2?family=Lato&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <button type="button" class="line-button">Button</button>
  </body>
</html>
```

```css
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  font-family: "Lato", sans-serif;
}

button,
input,
textarea {
  font-family: "Lato", sans-serif;
}

button {
  outline: none;
  border: none;
  background-color: #fff;
}

.line-button {
  position: relative;
  padding: 18px 30px;
  font-size: 16px;
  line-height: 1.25;
  color: #151b26;
  cursor: pointer;
}

.line-button::after {
  content: "";
  position: absolute; /* position을 주게 되면 block이 됩니다. */
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #0066ff;
  transition: width 250ms ease-in, background-color 250ms ease-in;
}

.line-button:hover::after {
  width: 100%;
  background-color: orange;
}
```

<br/>
<br/>
<br/>

# 🌈 Animation 훈련

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Animation</title>
    <link rel="stylesheet" href="./style.css" />
    <link
      href="https://fonts.googleapis.com/css2?family=Mulish:wght@500&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <section class="loading">
      <h1 class="loading-title">Loading...</h1>
      <div class="progress-bar" aria-hidden="true">
        <span class="progress-bar-gauge"></span>
      </div>
    </section>
  </body>
</html>
```

```css
* {
  box-sizing: border-box;
}

body {
  font-family: "Mulish", sans-serif;
}

.loading {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.loading-title {
  margin-bottom: 20px;
  font-size: 18px;
  font-weight: 500;
  line-height: 1.3333333;
  color: #151826;
  animation-name: flicker;
  animation-duration: 1600ms;
  animation-iteration-count: infinite; /* 애니메이션의 효과 횟수를 무한으로 나타 냅니다. */
  animation-direction: alternate; /* 애니메이션이 자연스럽게 교차적으로 진행되었다가 돌아옵니다. */
}

.progress-bar {
  position: relative;
  width: 300px;
  height: 12px;
  border-radius: 100px; /* px로 주는 이유는 50%를 주게되면 모양이 찌그러지게 됩니다. */
  background-color: #e5eaef;
  overflow: hidden; /* gauge의 width 값이 증가하여도 bar width의 값 이상으로 증가 할 경우 보여주지 않습니다. */
}

.progress-bar-gauge {
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  height: 12px;
  border-radius: 100px;
  background-color: #13ce65;
  animation-name: loading-bar;
  animation-duration: 3600ms;
  animation-iteration-count: infinite;
  animation-timing-function: ease-out;
}

@keyframes flicker {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

@keyframes loading-bar {
  0% {
    width: 0;
    opacity: 1;
  }
  80% {
    width: 100%;
    opacity: 1;
  }
  100% {
    width: 100%;
    opacity: 0;
  }
}
```

<br/>
<br/>
<br/>

# 🌈 Box Shadow

- Neomorphism 생성 사이트

[Neumorphism/Soft UI CSS shadow generator](https://neumorphism.io/#55b9f3)

- Box-Shadow의 속성 ( 순서를 지켜서 작성해야 합니다. )
- Property : h-offset(x축 이동), v-offset(y축 이동), blur(흐린 정도), spread(그림자 크기), color(색상)
- 속성을 다 적을 필요는 없습니다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Box-Shadow</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <button type="button" class="cancel-button">Cancel</button>
    <button type="button" class="confirm-button">Confirm</button>
  </body>
</html>
```

```css
body {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}

button {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  height: 56px;
  padding: 0 36px;
  border: none;
  border-radius: 50px;
  margin: 0 8px;
  font-size: 20px;
  font-weight: 600;
  color: #fff;
  cursor: pointer;
}

button:focus,
button:active {
  outline: none;
}

.cancel-button {
  background-color: #ff4949;
  transition: box-shadow 250ms ease-in;
}

.cancel-button:hover {
  box-shadow: 0 10px 16px 0 rgba(255, 73, 73, 0.35);
}

.confirm-button {
  background-color: #13ce66;
  box-shadow: 0 10px 16px 0 rgba(19, 206, 102, 0.35);
}
```

<br/>
<br/>
<br/>

# 🌈 Overflow

- 틀 안에 Content가 넘쳤을 때 효과를 주는 속성입니다.
- 속성 : visible, auto, scroll, hidden

<br/>
<br/>
<br/>

# 🌈 Transform

- 2,3차원에서 변형 시킵니다.
- 진정한 CSS 장인들이 자주 사용 합니다.
- 가장 많이 사용 하는 함수는 : translate() / scale() / rotate()
- 가운데 정렬 할 때 자주 사용 합니다. ( 특히 position )

- translate(x,y) : 옮길 때 사용 합니다.
- 기존의 위치를 기억하기 때문에 다른 태그들에게 영향을 주지 않습니다.

- 이동 단위를 %로 할 경우

```css
.box {
  width: 100px;
  height: 100px;
  transform: translate(
    100%,
    100%
  ); /* %로 작성할 경우 자신의 width, height를 기준으로 이동 합니다. */
}
```

- scale(N) : N배 만큼 커집니다.
- 자신의 크기를 기억합니다.
- 마찬가지로 다른 요소들 에게 영향을 주지 않습니다.

- rotate(Ndeg) : N deg 만큼 회전 합니다.
- 마찬가지로 다른 요소에 영향을 주지 않습니다.

<br/>
<br/>
<br/>

# 🌈 Visibility

- Visible ( 기본 값 입니다. )
- Hidden : 그냥 보이지만 않는 것 입니다.

```css
/* 밑에 코드는 아에 태그가 사라집니다. */
.box {
  display: none;
}
```

<br/>
<br/>
<br/>

# 🌈 Selector

- 원하는 요소를 고르는 방법을 배웁니다.
- Type (Tag를 직접 접근 합니다.) & Class & ID Selector
- Class는 중요합니다

```css
<div
  id="free"
  class="box"
  > </div
  > <div
  class="box"
  > </div
  > <div
  class="box"
  > </div
  > .box {
  color: red;
}
#free {
  font-size: 16px;
}

/* 2개를 하나로 표현해 보겠습니다.*/
/* 붙어서 표현하면 ID and Class를 의미 합니다. */
#free.box {
  color: red;
  font-size: 16px;
}
```

- Child, Descendant & Sibling Combinators
- 자식 선택자 & 자손 선택자 & 형제 선택자
- Childe Combinator

```css
/* 자식을 선택 할 때 사용 합니다. */
parent > child
```

- 자손 선택자

```css
/* 공백을 해줍니다. */
parent descendants
```

- Sibling Combinators ( 여러 형제를 선택 )

```css
parent + sibling
```

- 예를 들어

```css
<ul>
	<li></li>
	<li></li>
	<li class="active"></li>
	<li></li>
	<li></li>
</ul>

/* Class active 뒤의 모든 Li를 선택합니다. */
.active ~ li {
  color: blue;
}
/* Class active 바로 뒤의 Li(1개)를 선택합니다.*/
.active + li {
  color: green;
}
```

- Structural Pseudo-classes
- Element : first-child & last-child & :nth-child(n)

```css
<ul>
	<li></li>
	<li></li>
	<li class="active"></li>
	<li></li>
	<li></li>
</ul>

/* Li 중에 첫번째 요소를 선택합니다. */
li:first-child {
  color: red;
}

/* Li 중에 3번째 요소를 선택합니다. */
li:nth-child(3) {
  color: blue;
}
```

- hidden

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e5ba7b2-67cd-44ce-affc-ac0959b52e23/_2020-09-20__3.24.15.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e5ba7b2-67cd-44ce-affc-ac0959b52e23/_2020-09-20__3.24.15.png)

  # 🌈 Transform

  ***

  - 2,3차원에서 변형 시킵니다.
  - 진정한 CSS 장인들이 자주 사용 합니다.
  - 가장 많이 사용 하는 함수는 : translate() / scale() / rotate()
  - 가운데 정렬 할 때 자주 사용 합니다. ( 특히 position )

  - translate(x,y) : 옮길 때 사용 합니다.
  - 기존의 위치를 기억하기 때문에 다른 태그들에게 영향을 주지 않습니다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf5212d3-3dfe-42a0-8a2b-f6d8bfac6145/_2020-09-20__3.35.13.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf5212d3-3dfe-42a0-8a2b-f6d8bfac6145/_2020-09-20__3.35.13.png)

  - 이동 단위를 %로 할 경우

  ```css
  .box {
    width: 100px;
    height: 100px;
    transform: translate(
      100%,
      100%
    ); /* %로 작성할 경우 자신의 width, height를 기준으로 이동 합니다. */
  }
  ```

  - scale(N) : N배 만큼 커집니다.
  - 자신의 크기를 기억합니다.
  - 마찬가지로 다른 요소들 에게 영향을 주지 않습니다.

  - rotate(Ndeg) : N deg 만큼 회전 합니다.
  - 마찬가지로 다른 요소에 영향을 주지 않습니다.

  # 🌈 Visibility

  ***

  - Visible ( 기본 값 입니다. )
  - Hidden : 그냥 보이지만 않는 것 입니다.

  ```css
  /* 밑에 코드는 아에 태그가 사라집니다. */
  .box {
    display: none;
  }
  ```

  # 🌈 Selector

  ***

  - 원하는 요소를 고르는 방법을 배웁니다.
  - Type (Tag를 직접 접근 합니다.) & Class & ID Selector
  - Class는 중요합니다

  ```css
  <div
    id="free"
    class="box"
    > </div
    > <div
    class="box"
    > </div
    > <div
    class="box"
    > </div
    > .box {
    color: red;
  }
  #free {
    font-size: 16px;
  }

  /* 2개를 하나로 표현해 보겠습니다.*/
  /* 붙어서 표현하면 ID and Class를 의미 합니다. */
  #free.box {
    color: red;
    font-size: 16px;
  }
  ```

  - Child, Descendant & Sibling Combinators
  - 자식 선택자 & 자손 선택자 & 형제 선택자
  - Childe Combinator

  ```css
  /* 자식을 선택 할 때 사용 합니다. */
  parent > child
  ```

  - 자손 선택자

  ```css
  /* 공백을 해줍니다. */
  parent descendants
  ```

  - Sibling Combinators ( 여러 형제를 선택 )

  ```css
  parent + sibling
  ```

  - 예를 들어

  ```css
  <ul>
  	<li></li>
  	<li></li>
  	<li class="active"></li>
  	<li></li>
  	<li></li>
  </ul>
  
  /* Class active 뒤의 모든 Li를 선택합니다. */
  .active ~ li {
    color: blue;
  }
  /* Class active 바로 뒤의 Li(1개)를 선택합니다.*/
  .active + li {
    color: green;
  }
  ```

  - Structural Pseudo-classes
  - Element : first-child & last-child & :nth-child(n)

  ```css
  <ul>
  	<li></li>
  	<li></li>
  	<li class="active"></li>
  	<li></li>
  	<li></li>
  </ul>
  
  /* Li 중에 첫번째 요소를 선택합니다. */
  li:first-child {
    color: red;
  }

  /* Li 중에 3번째 요소를 선택합니다. */
  li:nth-child(3) {
    color: blue;
  }
  ```

<br/>

    - User Action Pseudo-classes
    - Element : hover & focus & active

    ```css
    /*
    PURPLE 1 #a389f5
    PURPLE 2 #7e5bef
    PURPlE 3 #592dea
    BLUE 1 #85d7ff
    BLUE 2 #1fb6ff
    BLUE 3 #009eeb
    */

    a:active {
    	background-color: #009eeb; /* 변화하는 찰나의 속성을 부여 합니다. */
    }

    input {
    	outline: none;
    	box-shadow: none;
    }

    /* 특정 행위를 했을 경우 스타일이 적용됩니다.*/
    input:focus {
    	border: 1px solid #592dea
    }
    ```

    # 🌈 CSS 선택자 올림픽

    ---

    ```css
    p {
    	color: blue;
    }

    p {
    	color: green;  /* green이 적용이 됩니다. */
    }
    ```

⁉️선택자 우선순위를 무시하는 경우

- Inline Style

```html
<p style="font-size: 32px;"></p>
```

- import : 가장 우선순위 입니다.

```css
p {
  color: red !important;
}
```

<br/>
<br/>
<br/>

# 🌈 Grid System

- Front-end 개발자로 입사 하게 되면 디자이너 분들의 시안을 받아서 코드로 구현을 합니다.
- 그 때 어떠한 방식으로 디자이너분들이 작업을 했는지 생각을 해야 합니다.
- 그 기준이 되는 것이 Grid System입니다.
- 우리가 알아야 되는 3가지 개념이 있습니다.
- Container & Column & Gutter
- 12 Column을 웹 디자인에서 많이 사용 합니다.
- Gutter는 Column의 양 옆의 간격을 의미 합니다.

<br/>
<br/>
<br/>

# 🌈 [실습] 작업 환경 세팅

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>FREEKO | WEB</title>
    <link
      href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="./grid.min.css" />
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="container">
      <div class="row">
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
        <div class="col-1">col-1</div>
      </div>
      <p>
        Lorem ipsum dolor sit, amet consectetur adipisicing elit. Magnam, error
        nesciunt pariatur dolorem tenetur culpa. Quod voluptatum ratione magnam
        iusto vitae adipisci reiciendis non dolore, magni ipsam. Assumenda,
        adipisci tempora.
      </p>
    </div>
  </body>
</html>
```

```css
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  font-family: "DM Sans", sans-serif;
}

/* Reset CSS */
@import "./reset.css";
a {
  color: inherit;
  text-decoration: none;
}

button,
input,
textarea {
  font-family: "DM Sans", sans-serif;
  font-size: 16px;
}

ul,
ol,
li {
  list-style: none;
  padding-left: 0;
  margin-left: 0;
}

button:focus,
button:active,
input:focus,
input:active,
textarea:focus,
textarea:active {
  outline: none;
  box-shadow: none;
}

p {
  font-size: 16px;
  line-height: 1.5;
  letter-spacing: -0.01em;
  color: #2b292d;
}

/* >= 768px (Desktop) */
@media screen and (min-width: 768px) {
  /* Reset CSS */
  p {
    font-size: 22px;
    line-height: 1.3636363636;
  }
}

/* Custom Grid System - Fix container width */
@media screen and (min-width: 1200px) {
  .container {
    max-width: 960px !important;
  }
}
```

# 🌈 [실습] Landing

---

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>FREEKO | WEB</title>
    <link
      href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="./grid.min.css" />
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <!-
부트스트랩 때문에 container, row, col-12 클래스에 스타일을 적용하는 것은 비 효율적입니다.
이미 부트스트랩에서 스타일을 적용되어져 있기 때문입니다.
그렇기 때문에 landing-content를 만들어서 스타일을 적용합니다.
->
    <section class="landing">
      <div class="container">
        <div class="row">
          <div class="col-12">
            <div class="landing-content">
              <h1 class="landing-title">
                Change your career,
                <br />
                Change your life
              </h1>
              <p class="landing-desc">
                Get ahead with expert-led training in
                <br />
                coding & design
              </p>
              <div class="button-group">
                <a href="#" class="fill-button">Apply now</a>
                <a href="#" class="fill-button inverted">Learn more</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </body>
</html>
```

```css
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  font-family: "DM Sans", sans-serif;
}

/* Reset CSS */
@import "./reset.css";
a {
  color: inherit;
  text-decoration: none;
}

button,
input,
textarea {
  font-family: "DM Sans", sans-serif;
  font-size: 16px;
}

ul,
ol,
li {
  list-style: none;
  padding-left: 0;
  margin-left: 0;
}

button:focus,
button:active,
input:focus,
input:active,
textarea:focus,
textarea:active {
  outline: none;
  box-shadow: none;
}

p {
  font-size: 16px;
  line-height: 1.5;
  letter-spacing: -0.01em;
  color: #2b292d;
}

/*
공통적으로 사용 하는 것들을 따로 작성합니다.
*/
.fill-button {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  width: 140px;
  height:48px;
  font-size 15px;
  font-weight: 700;
  line-height: 1.6;
  letter-spacing: 1.6;
  color:white;
  background-color: #3040C4;
  border-radius: 2px;
  transition: opacity .3s ease-in-out;
}

.fill-button.inverted {
  background-color:white;
  color: #3040C4;
}

.fill-button:hover {
  opacity: 0.5;
}

/* Landing */
.landing {
  background-color: #fdded8;
  text-align: center;
}

.landing-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  height: 100vh;
}

.landing-title {
  margin-bottom: 24px;
  font-size: 40px;
  line-height: 1;
  letter-spacing: -0.05em;
  color: #2b292d;
}

.landing-desc {
  margin-bottom: 20px;
}

.landing .button-group {
  display: flex;
  justify-content: center;
  align-items: center;
}

.landing .button-group .fill-button:first-child {
  margin-right: 8px;
}

/* >= 768px (Desktop) */
@media screen and (min-width: 768px) {
  /* Reset CSS */
  p {
    font-size: 22px;
    line-height: 1.3636363636;
  }

  .fill-button {
    width: 160px;
    height: 56px;
    font-size: 18px;
    line-height: 1.5555555556;
  }

  /* Landing */
  .landing-content {
    height: auto;
    padding: 120px 0;
  }

  .landing-title {
    font-size: 70px;
    line-height: 1.0285714286;
    margin-bottom: 32px;
  }

  .landing-desc {
    margin-bottom: 32px;
  }

  .landing .button-group .fill-button:first-child {
    margin-right: 16px;
  }
}

/* Custom Grid System - Fix container width */
@media screen and (min-width: 1200px) {
  .container {
    max-width: 960px !important;
  }
}
```
