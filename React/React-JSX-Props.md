# React 기초 학습 정리

---

## 1. `React.createElement`와 렌더링

React를 사용하는 가장 원초적인 방법입니다.

```js
const greeting = React.createElement('h1', { className: 'greeting' }, '안녕, React.');
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(greeting);
```

- `React.createElement(태그명, 속성객체, 자식내용)` 형태로 호출합니다.
- 이 함수가 반환하는 값은 **실제 DOM 노드가 아니라 "React Element"** 라는 순수한 자바스크립트 객체입니다. (콘솔에 찍어보면 `type`, `props`, `key` 등을 가진 일반 객체임을 확인할 수 있어요.)
- `ReactDOM.createRoot(...).render(엘리먼트)`가 이 React Element를 실제 브라우저 DOM으로 변환해서 화면에 그려줍니다.
- 기존 바닐라 JS 방식(`document.createElement` → `appendChild`)과 비교하면, React는 "요소를 만드는 것"과 "화면에 그리는 것"을 분리해서 관리한다는 차이가 있습니다.

> **보충 개념**: `React.createElement`가 반환하는 객체가 바로 다음에 나올 **가상 DOM(Virtual DOM)의 기본 단위**입니다. JSX는 결국 이 `createElement` 호출을 사람이 읽기 쉽게 감싼 문법일 뿐입니다.

---

## 2. 컴포넌트와 JSX

### 컴포넌트란?
- UI를 재사용 가능한 '부품' 단위로 나눈 것 (헤더, 버튼, 카드 등).
- **React Element를 반환하는 자바스크립트 함수(또는 클래스)** 입니다.
- 이름은 반드시 **대문자로 시작**해야 합니다. (소문자로 시작하면 React가 HTML 태그로 오해함)

### 클래스형 컴포넌트 vs 함수형 컴포넌트

| 구분      | 클래스형                            | 함수형                        |
|----------|------------------------------------|------------------------------|
| 문법      | `class X extends React.Component` | `function X() {}`            |
| 필수 구현 | `render()` 메서드                  | 함수 자체가 JSX를 반환          |
| 현재 추세 | 레거시(과거 방식)                   | **현재 표준** (Hooks 사용 가능) |

```js
class TitleClass extends React.Component {
    render() {
        return React.createElement('h1', null, '클래스형 컴포넌트입니다.');
    }
}
```

> **보충 개념**: 요즘 React 프로젝트는 거의 대부분 **함수형 컴포넌트 + Hooks**(`useState`, `useEffect` 등)로 작성합니다. 클래스형은 옛날 코드나 레거시 프로젝트를 읽을 때를 위해 알아두는 정도면 충분합니다. (아직 Hooks를 안 배우셨다면 다음 학습 순서로 이어질 내용입니다.)

### `createElement` 방식 vs JSX 방식

```js
// createElement 방식 - 중첩될수록 코드가 길고 가독성이 떨어짐
function OldWay() {
    return React.createElement('div', null,
        React.createElement('h1', { className: 'greeting' }, "CreateElement 방식"),
        React.createElement('p', null, '코드가 길고 복잡.')
    )
}

// JSX 방식 - HTML처럼 보여서 직관적
function NewWay() {
    return (
        <div>
            <h1 className='greeting'>JSX 방식</h1>
            <p>HTML처럼 보여서 훨씬 직관적이다.</p>
        </div>
    )
}
```

### 컴포넌트 합성(Composition)과 Fragment

```js
function App() {
    return (
        <>
            <OldWay />
            <hr />
            <NewWay />
        </>
    )
}
```

- JSX는 **반드시 하나의 부모 요소**로 감싸서 반환해야 합니다.
- 불필요한 `<div>`를 추가하고 싶지 않을 때 `<>...</>` (Fragment, 정식 표기는 `<React.Fragment>...</React.Fragment>`)를 사용합니다.
- 여러 컴포넌트를 조합해서 더 큰 컴포넌트를 만드는 것을 **컴포넌트 합성**이라고 합니다.

---

## 3. JSX 규칙 4가지

```js
function JsxRules() {
    return (
        <>
            <h2>1. {} 안에 자바스크립트 표현식을 넣을 수 있다.</h2>
            <h3>이름: {user.name}</h3>
            <p>소개: {introduce(user)}</p>

            <h2>2. HTML 속성과 약간 다른 규칙을 따른다.</h2>
            <button className='my-button' onClick={handleClick}>클릭해보세요.</button>

            <h2>3. 인라인 스타일은 객체 형태로 전달해야 한다.</h2>
            <p style={customStyle}>커스텀 스타일 적용</p>

            <h2>4. 모든 태그는 반드시 닫혀야 한다.</h2>
            <br /><hr /><input />
        </>
    )
}
```

1. **`{ }` 안에는 "표현식"만 가능** — 하나의 값으로 평가되는 코드(변수, 함수 호출, 삼항 연산자 등). `if`문 같은 "문장(statement)"은 넣을 수 없습니다.
2. **속성명이 HTML과 다름** — `class` → `className`, `onclick` → `onClick`, `for` → `htmlFor` 처럼 camelCase를 따릅니다. (DOM 프로퍼티 이름 규칙을 따르기 때문)
3. **인라인 스타일은 객체로 전달** — CSS 속성명도 camelCase로 씁니다. (`background-color` → `backgroundColor`)
4. **모든 태그는 닫혀야 함** — `<br />`, `<input />` 처럼 self-closing 태그도 예외 없이 슬래시로 닫아야 합니다.

> **보충 개념 (빠진 규칙 하나 더)**: JSX에서 주석은 `{/* 이렇게 */}` 형태로 작성해야 합니다 (`02_JSX.html`의 `{/* JSX 내부 주석*/}` 부분). HTML의 `<!-- -->`는 JSX 안에서는 동작하지 않습니다.

> **보충 개념**: JSX는 문법일 뿐 브라우저가 직접 이해하지 못합니다. `Babel`(예제에서 `babel.min.js`를 불러온 이유)이 JSX를 `React.createElement(...)` 호출로 **트랜스파일(변환)** 해줘야 브라우저에서 실행됩니다.

---

## 4. 렌더링과 가상 DOM (Virtual DOM) 

예제 코드(`03_rendering-and-vdom.html`)는 아래처럼 1초마다 시계를 다시 그리는 예제였는데, **왜 이게 "VDOM"의 예시인지에 대한 설명이 빠져있어서** 개념을 채워 넣었습니다.

```js
function Clock() {
    return (
        <>
            <h1>실시간 시계</h1>
            <h2>현재 시간: {new Date().toLocaleTimeString()}</h2>
        </>
    );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
function tick() {
    root.render(<Clock />); // 1초마다 재실행됨
}
setInterval(tick, 1000);
```

### 가상 DOM(Virtual DOM)이란?
- 실제 브라우저 DOM은 조작(수정)이 상대적으로 **느리고 비용이 큰 작업**입니다.
- React는 실제 DOM을 직접 건드리는 대신, **메모리 상에 가벼운 자바스크립트 객체로 UI 구조를 표현한 "가상 DOM"** 을 먼저 만듭니다. (이 객체가 바로 1번에서 본 `React.createElement`의 반환값입니다.)

### 렌더링이 일어나는 과정 (실제로 벌어지는 일)
1. `setInterval`이 1초마다 `tick()`을 실행 → `root.render(<Clock />)` 호출.
2. React는 `<Clock />`을 실행해서 **새로운 가상 DOM 트리**를 생성합니다.
3. React는 이 **새 가상 DOM**과 **이전 가상 DOM**을 비교(diffing)합니다. 이 과정을 **재조정(Reconciliation)** 이라고 합니다.
4. 비교 결과, **실제로 바뀐 부분만** 찾아서 실제 브라우저 DOM에 반영합니다.

이 예제에서는 `<h1>실시간 시계</h1>` 텍스트는 매초 똑같지만, `<h2>` 안의 시간 텍스트는 매초 바뀝니다. React는 diffing을 통해 **`<h2>`의 텍스트 노드만 실제로 갱신**하고, 안 바뀐 `<h1>`은 실제 DOM을 건드리지 않습니다.

### 왜 이렇게 하는가?
- 매번 전체 DOM을 새로 그리는 것보다, **바뀐 부분만 최소한으로 실제 DOM에 반영**하는 것이 훨씬 빠르고 효율적이기 때문입니다.
- 이것이 React가 빠른 이유 중 하나이며, 개발자는 "상태가 바뀌면 그냥 다시 `render`를 호출한다"는 **선언적(Declarative)** 방식으로만 코드를 짜면 되고, "어떤 DOM을 직접 어떻게 바꿀지"는 React가 알아서 계산해줍니다 (명령형 방식과의 차이).

> 참고: 실제 프로젝트에서는 `setInterval` + `root.render()`를 직접 호출하는 방식 대신, 다음 단계에서 배우게 될 **`useState`(상태) + `useEffect`(부수효과)** 를 사용해서 "상태가 바뀌면 React가 알아서 재렌더링"하게 만드는 것이 표준적인 방법입니다.

---

## 5. Props (속성)

```js
function Greeting({ name = '기본이름', favoriteNumber = 1, children }) {
    return (
        <>
            <h1>안녕하세요, {name}님</h1>
            <h2>제가 좋아하는 숫자는 {favoriteNumber} 입니다.</h2>
            <strong>전달된 자식 요소(children) : {children}</strong>
        </>
    );
}

function App() {
    return (
        <>
            <Greeting name='홍길동' favoriteNumber={5}>props 공부 중.</Greeting>
            <Greeting name='유관순' favoriteNumber={8}>
                <div>여러 줄도 가능</div>
                <div>React 최고</div>
            </Greeting>
            <Greeting>props를 넘기지 않으면 기본값이 적용됩니다.</Greeting>
        </>
    )
}
```

### Props 핵심 특징
- **부모 → 자식**으로 데이터를 전달하는 통로입니다.
- **읽기 전용(read-only)** 입니다 — 자식 컴포넌트는 전달받은 props를 직접 수정할 수 없습니다.
- 데이터는 항상 **위에서 아래로(단방향)** 흐릅니다. (자식이 부모의 데이터를 직접 바꾸는 것은 불가능 — 이건 이후에 배울 "상태 끌어올리기(lifting state up)"와 연결되는 개념입니다.)
- 함수의 매개변수(parameter)처럼 생각하면 이해하기 쉽습니다.

### 구조 분해 할당(Destructuring) + 기본값
```js
function Greeting({ name = '기본이름', favoriteNumber = 1, children }) { ... }
```
- 매개변수 자리에서 바로 `{ }`로 구조 분해하며, `=`로 **기본값**을 지정할 수 있습니다.
- props가 전달되지 않으면 기본값이 사용됩니다. (예: 세 번째 `<Greeting>`)

### `children`이란?
- 컴포넌트 태그 **사이에 작성한 내용**은 자동으로 `props.children`이라는 특별한 prop으로 전달됩니다.
```
<Greeting name="홍길동">props 공부 중.</Greeting>
// 내부적으로는:
// props = { name: "홍길동", children: "props 공부 중." }
```
- `children`에는 텍스트뿐 아니라 **여러 개의 JSX 엘리먼트도 그대로 전달**할 수 있습니다. (두 번째 `Greeting` 예시처럼 `<div>` 두 개를 감싸서 전달 가능)

> **보충 개념**: props로 전달할 수 있는 값은 문자열뿐 아니라 숫자, 배열, 객체, 함수, 심지어 JSX 엘리먼트까지 **모든 자바스크립트 값**이 가능합니다. 문자열이 아닌 값을 전달할 때만 `{ }`를 사용한다는 점이 `02_JSX.html`의 규칙 1번과 연결됩니다.

---

## 전체 학습 흐름
```
createElement (React의 최소 단위)
    ↓
JSX (createElement를 편하게 쓰기 위한 문법, Babel이 변환)
    ↓
컴포넌트 (JSX를 반환하는 함수/클래스, 재사용 가능한 UI 단위)
    ↓
Props (컴포넌트 간 데이터 전달 통로, 부모 → 자식)
    ↓
가상 DOM & 재조정 (state/props가 바뀔 때마다 효율적으로 재렌더링)
    ↓
(다음 단계) useState, useEffect 등 Hooks로 상태 관리
```

---

### 다음에 보면 좋을 개념 
- **`useState`**: 컴포넌트 내부에서 변하는 값을 관리 (시계 예제를 hook으로 개선하는 방법)
- **`useEffect`**: 렌더링 이후 실행되는 부수 효과(타이머, API 호출 등) 관리
- **이벤트 핸들링 심화**: `onChange`, 폼(form) 다루기
- **조건부 렌더링**: `{condition && <X />}`, 삼항 연산자 활용
- **리스트 렌더링과 `key`**: 배열을 `map()`으로 돌려서 렌더링할 때 필요한 개념