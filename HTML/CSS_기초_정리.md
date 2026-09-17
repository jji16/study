# CSS 기초 정리

## 1. CSS란 무엇인가?

CSS는 **Cascading Style Sheets**의 약자입니다.

HTML이 웹 페이지의 **구조와 내용**을 만든다면, CSS는 그 구조를 **디자인하고 배치**하는 역할을 합니다.

예를 들어 HTML로 버튼을 만들면 다음과 같습니다.

```html
<button>확인</button>
```

이 상태에서는 브라우저의 기본 디자인으로 표시됩니다. CSS를 적용하면 다음과 같이 모양을 변경할 수 있습니다.

```css
button {
    background-color: blue;
    color: white;
    padding: 10px 20px;
}
```

CSS를 이용하면 다음과 같은 것을 변경할 수 있습니다.

- 글자 색상 / 글자 크기
- 배경색
- 요소의 크기, 요소 사이의 간격
- 요소의 위치, 화면 레이아웃
- 애니메이션
- 반응형 디자인

---

## 2. CSS의 기본 문법

CSS는 다음과 같은 형태로 작성합니다.

```css
선택자 {
    속성: 값;
}
```

예시:

```css
p {
    color: red;
}
```

이 코드를 나누어 보면 다음과 같습니다.

```text
p       → 선택자(Selector)
color   → 속성(Property)
red     → 값(Value)
color: red;  → 선언(Declaration)
```

> `p` 태그를 선택해서 글자 색상(`color`)을 빨간색(`red`)으로 설정한다.

---

## 3. 선택자(Selector)

선택자는 CSS를 적용할 HTML 요소를 선택하는 방법입니다.

```html
<h1>제목</h1>
<p>내용입니다.</p>
<p>두 번째 내용입니다.</p>
```

```css
p {
    color: blue;
}
```

### 3-1. 태그 선택자

HTML 태그 이름을 이용하여 선택합니다. 모든 해당 태그에 적용됩니다.

```css
h1 { color: red; }
p  { font-size: 18px; }
```

- **장점**: 같은 종류의 요소에 한꺼번에 스타일을 적용할 수 있습니다.
- **주의점**: 특정 요소 하나에만 스타일을 적용하고 싶을 때는 적합하지 않을 수 있습니다.

### 3-2. 클래스 선택자

HTML의 `class` 속성을 이용하는 선택자입니다. 이름 앞에 `.`을 붙입니다.

```html
<p class="title">제목</p>
<p class="content">내용</p>
```

```css
.title   { color: red; }
.content { color: blue; }
```

여러 요소에 동일한 스타일을 적용할 때 주로 사용합니다.

```html
<p class="text">내용 1</p>
<p class="text">내용 2</p>
<p class="text">내용 3</p>
```

### 3-3. ID 선택자

HTML의 `id` 속성을 이용합니다. 앞에 `#`을 사용합니다.

```html
<h1 id="main-title">메인 제목</h1>
```

```css
#main-title {
    color: blue;
}
```

class와 id의 차이:

```text
class → 여러 요소에서 사용할 수 있음
id    → 하나의 요소를 고유하게 식별하는 용도로 사용
```

### 3-4. 전체 선택자

모든 요소를 선택합니다. `*` 기호를 사용합니다.

```css
* {
    margin: 0;
    padding: 0;
}
```

### 3-5. 그룹 선택자

여러 선택자에 같은 스타일을 적용할 수 있습니다. 쉼표(`,`)로 구분합니다.

```css
h1, h2, h3 {
    color: blue;
}
```

### 3-6. 자손 선택자

특정 요소 내부에 있는 모든 요소를 선택합니다. (몇 단계 아래든 상관없음)

```html
<div>
    <p>내용</p>
</div>
```

```css
div p {
    color: red;
}
```

> div 내부에 있는 모든 p 요소를 선택한다.

### 3-7. 자식 선택자

바로 아래에 있는 자식 요소만 선택합니다. `>` 기호를 사용합니다.

```css
div > p {
    color: blue;
}
```

자손 선택자와 자식 선택자의 차이:

```text
div p   → div 내부의 모든 p (몇 단계든 상관없음)
div > p → div 바로 아래에 있는 p만
```

### 3-8. 형제 선택자

같은 부모 아래 있는 형제 요소를 선택합니다.

```css
h1 + p {
    color: green;
}

h1 ~ p {
    color: purple;
}
```

```text
h1 + p → h1 바로 다음에 오는 p 하나만 (인접 형제)
h1 ~ p → h1 뒤에 오는 모든 형제 p (일반 형제)
```

### 3-9. 속성 선택자

특정 속성이나 속성값을 가진 요소를 선택합니다.

```css
input[type="text"] {
    border: 1px solid gray;
}

a[target="_blank"] {
    color: orange;
}
```

### 3-10. 가상 클래스로 위치 선택하기

요소의 순서에 따라 선택할 수 있습니다.

```css
li:first-child { color: red; }   /* 첫 번째 자식 */
li:last-child  { color: blue; }  /* 마지막 자식 */
li:nth-child(2) { color: green; } /* 두 번째 자식 */
p:not(.highlight) { color: gray; } /* highlight 클래스가 아닌 요소 */
```

---

## 4. CSS 적용 방법

CSS를 HTML에 적용하는 방법은 크게 세 가지입니다.

### 4-1. 인라인 CSS

HTML 태그에 직접 작성합니다. 작은 테스트에는 편리하지만 많은 스타일을 작성하면 관리하기 어렵습니다.

```html
<p style="color: red;">안녕하세요</p>
```

### 4-2. 내부 CSS

HTML의 `<head>` 내부에 `<style>` 태그를 사용합니다.

```html
<head>
    <style>
        p {
            color: red;
        }
    </style>
</head>
```

### 4-3. 외부 CSS

별도의 CSS 파일을 만들어 연결하는 방식입니다. 실제 웹 개발에서는 외부 CSS 파일을 사용하는 경우가 많습니다.

```html
<link rel="stylesheet" href="style.css">
```

```css
/* style.css */
p {
    color: red;
}
```

- `rel="stylesheet"`: 현재 HTML 문서와 연결된 파일의 관계(스타일시트 파일)를 나타냅니다.
- `href="style.css"`: 연결할 파일의 위치를 지정합니다.

---

## 5. 색상(Color)

CSS에서 색상을 설정하는 방법은 여러 가지가 있습니다.

```css
color: red;                    /* 색상 이름 */
color: #ff0000;                /* HEX 코드 */
color: rgb(255, 0, 0);         /* RGB */
color: rgba(255, 0, 0, 0.5);   /* RGB + 투명도 */
```

### RGBA

RGB에 투명도를 추가한 방식입니다. 마지막 값은 투명도입니다.

```text
0   → 완전히 투명
0.5 → 반투명
1   → 완전히 불투명
```

---

## 6. 글자 관련 CSS

### font-size

글자 크기를 설정합니다.

### font-weight

글자의 굵기를 설정합니다.

```css
font-weight: bold;
font-weight: 400; /* 일반 */
font-weight: 700; /* 굵게 */
```

### font-family

글꼴을 설정합니다.

```css
font-family: Arial, sans-serif;
```

여러 글꼴을 작성하는 이유는 첫 번째 글꼴을 사용할 수 없을 경우 다음 글꼴을 사용하기 위해서입니다.

### text-align

글자를 정렬합니다.

```css
text-align: center; /* left, center, right */
```

### line-height

줄 간격을 설정합니다.

```css
line-height: 1.5;
```

### text-decoration

밑줄, 취소선 등을 설정합니다.

```css
text-decoration: none;      /* 밑줄 제거 (링크에 자주 사용) */
text-decoration: underline; /* 밑줄 */
text-decoration: line-through; /* 취소선 */
```

---

## 7. 크기와 단위

### px

**Pixel(픽셀)**의 약자입니다. 화면에서 가장 기본적인 크기 단위입니다.

```css
width: 300px;
```

### %

부모 요소를 기준으로 비율을 계산합니다.

```css
width: 50%;
```

부모 요소의 너비가 1000px이라면 50%는 500px 정도가 됩니다.

### vw / vh

**Viewport Width / Height**의 약자입니다. 브라우저 화면의 너비/높이를 기준으로 합니다.

```css
width: 50vw;   /* 화면 너비의 50% */
height: 100vh; /* 화면 전체 높이 */
```

### rem

HTML 루트 요소(`html`)의 글자 크기를 기준으로 합니다.

```css
font-size: 2rem;
```

브라우저 기본 글자 크기가 16px이라면:

```text
1rem = 16px
2rem = 32px
```

### em

현재 요소 또는 부모 요소의 글자 크기와 관련된 상대 단위입니다.

```css
font-size: 1.5em;
```

중첩 구조에 따라 크기 계산이 달라질 수 있으므로 `rem`과의 차이를 이해하는 것이 중요합니다.

---

## 8. Box Model

CSS에서 매우 중요한 개념입니다. HTML 요소는 하나의 사각형 상자로 생각할 수 있습니다.

```text
Margin
┌───────────────────────┐
│       Border          │
│   ┌───────────────┐   │
│   │    Padding    │   │
│   │  ┌─────────┐  │   │
│   │  │ Content │  │   │
│   │  └─────────┘  │   │
│   └───────────────┘   │
└───────────────────────┘
```

### Content

실제 내용이 표시되는 영역입니다. (글자, 이미지, 버튼 등)

### Padding

Content와 Border 사이의 안쪽 여백입니다.

```css
padding: 20px;
```

### Border

요소의 테두리입니다.

```css
border: 1px solid black;
```

### Margin

요소와 다른 요소 사이의 바깥 여백입니다.

```css
margin: 20px;
```

### Margin과 Padding의 차이

```text
Margin  → 요소 밖의 공간
Padding → 요소 내부의 공간
```

---

## 9. box-sizing

기본적인 CSS Box Model에서는 `width`와 `height`를 설정한 후 padding과 border가 추가될 수 있습니다.

```css
.box {
    width: 300px;
    padding: 20px;
}
```

원하는 크기를 계산하기 복잡해질 수 있어서, 다음 설정을 자주 사용합니다.

```css
* {
    box-sizing: border-box;
}
```

`border-box`를 사용하면 설정한 width 안에 padding과 border를 포함하여 크기를 계산합니다.

---

## 10. Display

요소가 화면에 표시되는 방식을 결정합니다.

### block

```css
display: block;
```

블록 요소처럼 표시합니다. 일반적으로 새로운 줄에서 시작합니다.

### inline

```css
display: inline;
```

문장 흐름 안에서 표시됩니다. 일반적으로 내용만큼 공간을 차지합니다.

### inline-block

```css
display: inline-block;
```

인라인처럼 나란히 배치되면서 width와 height 설정이 가능합니다.

### none

```css
display: none;
```

요소를 화면에서 표시하지 않습니다. 해당 요소가 화면의 공간도 차지하지 않습니다.

### display: none과 visibility: hidden의 차이

```text
display: none      → 요소가 사라지고 공간도 차지하지 않음
visibility: hidden  → 요소는 보이지 않지만 공간은 그대로 차지함
```

---

## 11. Position

요소의 위치를 설정하는 방식입니다.

### static

```css
position: static;
```

기본 위치 방식입니다. HTML 문서의 일반적인 흐름에 따라 배치됩니다.

### relative

```css
.box {
    position: relative;
    top: 10px;
    left: 20px;
}
```

원래 위치를 기준으로 이동합니다.

### absolute

```css
.box {
    position: absolute;
    top: 0;
    left: 0;
}
```

특정 기준 요소를 기준으로 위치를 지정합니다. 일반적으로 부모 요소에 다음을 설정합니다.

```css
.parent {
    position: relative;
}
```

그러면 자식 요소의 absolute 위치를 부모를 기준으로 설정할 수 있습니다.

### fixed

```css
.box {
    position: fixed;
    bottom: 20px;
    right: 20px;
}
```

브라우저 화면을 기준으로 고정합니다. 스크롤해도 위치가 유지됩니다.

### sticky

```css
.menu {
    position: sticky;
    top: 0;
}
```

일반적인 위치에 있다가 특정 스크롤 위치에서 고정됩니다.

---

## 12. Flexbox

Flexbox는 여러 요소를 **한 방향으로 정렬하고 배치하기 위한 CSS 레이아웃 방식**입니다. 부모 요소에 적용합니다.

```css
.container {
    display: flex;
}
```

```html
<div class="container">
    <div>1</div>
    <div>2</div>
    <div>3</div>
</div>
```

```text
.container      → Flex Container
내부의 자식 요소  → Flex Item
```

### flex-direction

Flex Item의 배치 방향을 결정합니다.

```css
flex-direction: row; /* row: 가로, column: 세로 */
```

### Main Axis와 Cross Axis

Flexbox에서 가장 중요한 개념입니다. 기본값(`flex-direction: row`)일 경우:

```text
Main Axis  → 가로
Cross Axis → 세로
```

```text
justify-content → Main Axis 기준 정렬
align-items     → Cross Axis 기준 정렬
```

### justify-content

주축 방향으로 요소를 정렬합니다.

```css
justify-content: center;
```

```text
flex-start     → 시작 부분
center         → 가운데
flex-end       → 끝 부분
space-between  → 요소 사이의 공간을 동일하게 배분
```

### align-items

교차축 방향으로 요소를 정렬합니다.

```css
align-items: center;
```

### flex-wrap

Flex Item이 한 줄에 다 들어가지 않을 때 줄바꿈 여부를 결정합니다.

```css
flex-wrap: wrap; /* nowrap(기본값), wrap */
```

### flex-grow / flex-shrink / flex-basis

각 Flex Item이 얼마나 늘어나고 줄어들지를 결정합니다.

```css
.item {
    flex-grow: 1;   /* 남는 공간을 채우는 비율 */
    flex-shrink: 1; /* 공간이 부족할 때 줄어드는 비율 */
    flex-basis: 100px; /* 기본 크기 */
}
```

### gap

Flex Item 사이의 간격을 설정합니다.

```css
gap: 20px;
```

---

## 13. CSS Grid

Grid는 화면을 **행(Row)과 열(Column)**로 나누어 요소를 배치하는 레이아웃 시스템입니다.

```css
.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 100px 100px;
    gap: 10px;
}
```

- `grid-template-columns`: 열의 개수와 크기를 지정합니다. (예: 3개의 열)
- `grid-template-rows`: 행의 개수와 크기를 지정합니다.
- `gap`: 행과 열 사이의 간격을 지정합니다. (Flexbox의 gap과 동일한 속성)

### fr

**Fraction**의 약자로 비율을 나타냅니다.

```css
grid-template-columns: 1fr 2fr;
```

전체 공간을 1:2 비율로 나눕니다.

### grid-template-areas

영역에 이름을 붙여 레이아웃 구조를 시각적으로 표현할 수 있습니다.

```css
.container {
    display: grid;
    grid-template-areas:
        "header header"
        "sidebar content";
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.content { grid-area: content; }
```

### Grid와 Flexbox 차이

```text
Flexbox → 한 방향의 정렬에 적합
Grid    → 행과 열을 동시에 사용하는 레이아웃에 적합
```

```text
메뉴를 가로로 정렬     → Flexbox
복잡한 카드 레이아웃    → Grid
```

---

## 14. 가상 클래스(Pseudo-class)와 가상 요소(Pseudo-element)

### 가상 클래스 (Pseudo-class)

요소의 **특정 상태**를 선택할 때 사용합니다. 콜론(`:`) 하나를 사용합니다.

```css
button:hover  { background-color: blue; }    /* 마우스를 올렸을 때 */
button:active { transform: scale(0.95); }    /* 클릭하는 순간 */
input:focus   { border-color: blue; }        /* 입력 요소가 선택되었을 때 */
```

### 가상 요소 (Pseudo-element)

요소의 **특정 부분**을 선택하거나, 존재하지 않는 콘텐츠를 새로 만들어 넣을 때 사용합니다. 콜론 두 개(`::`)를 사용합니다.

```css
p::first-line {
    font-weight: bold;
}

.box::before {
    content: "★ ";
}

.box::after {
    content: " ✓";
}
```

`::before`, `::after`는 아이콘 추가, 장식 요소 등에 자주 사용됩니다.

---

## 15. Overflow

내용이 요소의 크기를 넘어갔을 때 처리하는 방법입니다.

```css
overflow: hidden;
```

```text
visible → 넘친 내용 표시
hidden  → 넘친 내용 숨김
scroll  → 스크롤 표시
auto    → 필요한 경우 스크롤 표시
```

---

## 16. z-index

요소가 서로 겹칠 때 앞뒤 순서를 결정합니다.

```css
z-index: 10;
```

일반적으로 숫자가 클수록 앞쪽에 표시됩니다. (단, `position`이 `static`이 아닌 요소에만 적용됩니다.)

---

## 17. Opacity

요소의 투명도를 설정합니다.

```css
opacity: 0.5;
```

```text
0 → 완전히 투명
1 → 완전히 불투명
```

---

## 18. Transition

CSS 속성이 변경될 때 부드럽게 변화하도록 합니다.

```css
button {
    background-color: gray;
    transition: 0.3s;
}

button:hover {
    background-color: blue;
}
```

마우스를 올리면 배경색이 즉시 변경되는 것이 아니라 부드럽게 변경됩니다.

---

## 19. Transform

요소의 위치나 크기, 회전 등을 변경합니다.

```css
transform: scale(1.2);       /* 확대 */
transform: translateX(20px); /* 이동 */
transform: rotate(45deg);    /* 회전 */
```

---

## 20. Animation

CSS만으로 애니메이션을 만들 수 있습니다. 먼저 움직임의 규칙을 만듭니다.

```css
@keyframes move {
    from { transform: translateX(0); }
    to   { transform: translateX(100px); }
}
```

그다음 요소에 적용합니다.

```css
.box {
    animation: move 2s;
}
```

---

## 21. 반응형 웹 디자인

반응형 웹 디자인은 화면 크기에 따라 웹 페이지의 레이아웃이 변경되는 방식입니다.

```text
컴퓨터   → 넓은 화면
태블릿   → 중간 화면
스마트폰 → 작은 화면
```

이를 위해 Media Query를 사용합니다.

### Media Query

```css
@media (max-width: 768px) {
    p {
        font-size: 14px;
    }
}
```

> 화면의 너비가 768px 이하라면 p 태그의 글자 크기를 14px로 변경한다.

### 모바일 우선(Mobile First) 접근

기본 스타일을 작은 화면 기준으로 작성하고, 화면이 커질수록 `min-width` 미디어 쿼리로 스타일을 추가하는 방식입니다. 최근 반응형 개발에서 많이 사용됩니다.

```css
/* 기본: 모바일 스타일 */
p { font-size: 14px; }

/* 화면이 768px 이상일 때 */
@media (min-width: 768px) {
    p { font-size: 18px; }
}
```

---

## 22. CSS Cascade

CSS의 C는 **Cascading**입니다. 여러 스타일 규칙이 하나의 요소에 적용될 경우 어떤 스타일을 최종적으로 적용할지 결정하는 과정과 관련 있습니다.

```css
p { color: red; }
p { color: blue; }
```

같은 우선순위라면 일반적으로 나중에 작성된 스타일이 적용됩니다. 결과: `blue`

---

## 23. Specificity(선택자 우선순위)

여러 CSS 규칙이 같은 요소에 적용될 때 선택자의 우선순위가 중요합니다.

기본적인 우선순위는 다음과 같습니다.

```text
인라인 스타일
    ↓
ID 선택자
    ↓
클래스 선택자 (속성 선택자, 가상 클래스 포함)
    ↓
태그 선택자
```

```html
<p id="title" class="text">내용</p>
```

```css
p       { color: blue; }
.text   { color: green; }
#title  { color: red; }
```

ID 선택자가 더 높은 우선순위를 가지므로 빨간색이 적용됩니다.

### 우선순위 점수로 이해하기

선택자마다 점수를 매겨 합산해서 비교한다고 생각하면 이해하기 쉽습니다.

| 선택자 종류                                         | 점수    |
|--------------------------------------------------- |--------|
| 태그 선택자 (`p`)                                   | 1점     |
| 클래스/속성/가상 클래스 (`.text`, `[type]`, `:hover`) | 10점    |
| ID 선택자 (`#title`)                                | 100점   | 
| 인라인 스타일 (`style="..."`)                        | 1000점  |

점수가 높은 규칙이 우선 적용되며, 점수가 같다면 나중에 작성된 규칙이 적용됩니다.

### `!important`

우선순위와 관계없이 가장 강제로 적용하고 싶을 때 사용합니다. 다만 유지보수가 어려워지므로 남용하지 않는 것이 좋습니다.

```css
p {
    color: red !important;
}
```

---

## 24. Inheritance(상속)

부모 요소의 일부 스타일이 자식 요소에게 전달되는 현상입니다.

```css
body {
    color: blue;
}
```

body 내부의 글자 요소들이 영향을 받을 수 있습니다. 하지만 모든 CSS 속성이 상속되는 것은 아닙니다. (`color`, `font-family` 등은 상속되지만 `border`, `margin`, `padding` 등은 상속되지 않습니다.)

---

## 25. CSS 변수 (Custom Property)

반복해서 사용하는 값을 변수로 저장해두고 재사용할 수 있습니다.

```css
:root {
    --main-color: #3498db;
    --spacing: 20px;
}

.box {
    color: var(--main-color);
    margin: var(--spacing);
}
```

- `:root`: 문서 전체에서 사용할 변수를 선언하는 대표적인 위치
- `var(--변수이름)`: 변수를 사용하는 방법
- 색상 테마, 간격 등을 한 곳에서 관리할 때 유용합니다.

---

## 26. calc()

CSS 안에서 사칙연산을 사용할 수 있는 함수입니다. 서로 다른 단위끼리도 계산할 수 있습니다.

```css
.box {
    width: calc(100% - 40px);
}
```

전체 너비에서 40px을 뺀 값을 너비로 사용합니다. 연산자 앞뒤에는 반드시 공백을 넣어야 합니다.

---

## 27. CSS 주석

CSS 코드에 설명을 작성할 때 사용합니다. 브라우저 화면에는 표시되지 않습니다.

```css
/* 이것은 CSS 주석입니다 */
```

---

## 28. 자주 사용하는 CSS 속성

| 속성                  | 뜻                 |
|----------------------|--------------------|
| `color`              | 글자 색상           |
| `background-color`   | 배경색              |
| `font-size`          | 글자 크기           |
| `font-weight`        | 글자 굵기           |
| `font-family`        | 글꼴                |
| `width` / `height`   | 너비 / 높이         |
| `margin`             | 바깥 여백           |
| `padding`            | 안쪽 여백           |
| `border`             | 테두리              |
| `border-radius`      | 테두리 모서리 둥글기  |
| `box-shadow`         | 그림자 효과          |
| `display`            | 표시 방식            |
| `position`           | 위치 방식            |
| `gap`                | 요소 사이 간격        |
| `overflow`           | 넘치는 내용 처리      |
| `opacity`            | 투명도               |
| `z-index`            | 앞뒤 순서            |
| `cursor`             | 마우스 커서 모양      |

### 자주 쓰는 시각 효과 예시

```css
.card {
    border-radius: 8px;                 /* 모서리를 둥글게 */
    box-shadow: 0 2px 8px rgba(0,0,0,0.2); /* 은은한 그림자 */
    cursor: pointer;                    /* 마우스를 손가락 모양으로 */
}
```

---

## 정리

CSS에서 반드시 이해해야 할 핵심은 다음과 같습니다.

```text
선택자        → 어떤 요소를 선택할 것인가?
속성          → 무엇을 변경할 것인가?
값            → 어떻게 변경할 것인가?

Box Model     → 요소의 실제 크기와 여백은 어떻게 구성되는가?
Display       → 요소는 화면에 어떤 방식으로 배치되는가?
Position      → 요소의 위치는 무엇을 기준으로 하는가?
Flexbox       → 한 방향으로 요소를 어떻게 정렬하는가?
Grid          → 행과 열을 이용하여 어떻게 배치하는가?
Media Query   → 화면 크기에 따라 어떻게 스타일을 변경하는가?
CSS 변수/calc → 값을 어떻게 재사용하고 계산할 것인가?
Specificity   → 여러 규칙이 충돌할 때 무엇이 우선하는가?
```