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
