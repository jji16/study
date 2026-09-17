# React useState & 이벤트 처리 정리

> 정리 

---

## 1. useState Hook

### 개념
- **Hook**: 함수형 컴포넌트가 React의 기능(상태, 생명주기 등)을 사용할 수 있게 연결해주는 함수
- **useState**: 함수형 컴포넌트에서 상태(state)를 사용할 수 있게 해주는 Hook

### props vs state
| 구분   | 설명                                        |
|-------|---------------------------------------------|
| props | 부모 컴포넌트가 전달, 자식이 직접 수정 불가       |
| state | 컴포넌트 내부에서 관리, setter 함수로만 변경 가능 |

### 기본 문법
```js
const { useState } = React;

// [ 상태 값, 상태 변경 함수 ] = useState(초기값)
const [message, setMessage] = useState('초기 상태 메세지입니다.');
```

### 리렌더링 원리
- setter 함수를 호출하면 컴포넌트가 **리렌더링**된다 (새로고침 아님)
- 컴포넌트 함수가 다시 실행되면서 변경된 state 값으로 JSX가 새로 만들어짐

### 예제 1 — MessageManager (텍스트 & 색상 변경)
```jsx
function MessageManager() {
    const [message, setMessage] = useState('초기 상태 메세지입니다.');
    const [textColor, setTextColor] = useState('black');

    const handleEnter = () => {
        setMessage('안녕하세요, 환영합니다.');
        setTextColor('blue');
    };

    const handleLeave = () => {
        setMessage('안녕히가세요, 다음에 또 만나요.');
        setTextColor('red');
    };

    return (
        <>
            <h2 style={{ color: textColor }}>{message}</h2>
            <button onClick={handleEnter}>입장</button>
            <button onClick={handleLeave}>퇴장</button>
        </>
    );
}
```

### 예제 2 — Counter (일반 업데이트 vs 함수형 업데이트)

**문제 상황**: 같은 이벤트 핸들러 안에서 state를 두 번 연속 변경해도 의도대로 동작하지 않음
```jsx
// ❌ +2가 아니라 +1로 동작
onClick={() => {
    setNumber(number + 1);
    setNumber(number + 1);
}}
```
→ React는 상태 변경을 **모아서(batch) 처리**하기 때문에, 두 번 모두 렌더링 시점의 동일한 `number` 값을 기준으로 계산됨

**해결 — 함수형 업데이트**
```jsx
// ✅ 의도대로 +2 동작
const increaseByTwo = () => {
    setNumber((prev) => prev + 1);
    setNumber((prev) => prev + 1);
};
```
→ setter의 매개변수(`prev`)로 React가 관리하는 **최신 이전 상태값**이 전달됨
→ 같은 이벤트 안에서 이전 state를 기준으로 연속 계산해야 할 때는 이 방식을 사용

### 핵심 요약
| 방식                           | 특징                        | 사용 시점                        |
|-------------------------------|-----------------------------|--------------------------------|
| `setNumber(number + 1)`       | 렌더링 시점의 값을 직접 사용    | 단순 1회 변경                   |
| `setNumber(prev => prev + 1)` | React가 관리하는 최신 값을 사용 | 같은 이벤트에서 연속 변경 필요 시 |

---

## 2. 이벤트 처리 (Event Handling)

### 기본 원칙
- `value`와 `onChange`를 함께 연결하면 input 값이 **React state에 의해 제어**됨 (제어 컴포넌트)
- `onChange`: input의 내용에 변화가 생길 때마다 실행
- `e.target.value`: 이벤트를 일으킨 대상(`e.target`)이 가지고 있는 최신 값

### 방식 1 — 개별 state로 관리 (SignupFormBasic)
```jsx
function SignupFormBasic() {
    const [username, setUsername] = useState('');
    const [email, setEmail] = useState('');

    const handleSignup = () => {
        alert(`아이디: ${username} \n 이메일: ${email} \n 회원가입 완료.`);
        setUsername('');
        setEmail('');
    };

    return (
        <>
            <input
                type="text"
                placeholder="아이디"
                value={username}
                onChange={e => setUsername(e.target.value)} />
            <input
                type="email"
                placeholder="이메일"
                value={email}
                onChange={e => setEmail(e.target.value)} />
            <button onClick={handleSignup}>가입하기</button>
        </>
    );
}
```
- input이 많아질수록 state와 핸들러가 계속 늘어나는 단점

### 방식 2 — 하나의 객체 state로 관리 (SignupFormAdvanced)
```jsx
function SignupFormAdvanced() {
    const [form, setForm] = useState({
        username: '',
        email: ''
    });
    const { username, email } = form;

    const handleChange = (e) => {
        const { name, value } = e.target; // name: input 구분자, value: 최신 입력값
        setForm({
            ...form,        // 기존 form 객체 복사
            [name]: value   // 변경된 input의 name을 key로 삼아 값 덮어쓰기
        });
    };

    const handleSignup = () => {
        alert(`아이디: ${username}, 이메일: ${email}`);
        setForm({ username: '', email: '' });
    };

    return (
        <>
            <input
                type="text"
                name="username"  // state 객체의 key와 동일하게 지정
                placeholder="아이디"
                value={username}
                onChange={handleChange} />
            <input
                type="email"
                name="email"
                placeholder="이메일"
                value={email}
                onChange={handleChange} />
            <button onClick={handleSignup}>가입하기</button>
        </>
    );
}
```
- **핵심 포인트**: `input`의 `name` 속성을 state 객체의 key와 동일하게 지정 → 하나의 `handleChange` 함수로 여러 input을 동시에 처리 가능
- 스프레드 연산자(`...form`)로 기존 값 복사 후, 계산된 속성명(`[name]`)으로 해당 필드만 갱신

### 두 방식 비교
| 구분       | 개별 state (Basic)            | 객체 state (Advanced)                |
|-----------|-------------------------------|-------------------------------------|
| state 개수 | input마다 useState 1개         | useState 1개 (객체)                  |
| 핸들러     | input마다 별도 함수             | 공통 handleChange 하나                |
| 확장성     | input 늘어날수록 코드 증가       | name만 맞추면 재사용 가능              |
| 코드 예    | `setUsername(e.target.value)` | `setForm({...form, [name]: value})` |

---

## 3. 전체 요약

- **useState**: 배열 구조분해할당으로 `[값, setter]`를 받아 상태 관리
- **리렌더링**: setter 호출 → 컴포넌트 함수 재실행 → 새 JSX 생성
- **함수형 업데이트**(`prev => ...`): 같은 이벤트 내 연속 상태 변경 시 필수
- **제어 컴포넌트**: `value` + `onChange`로 input을 state와 동기화
- **공통 handleChange 패턴**: `name` 속성 + 객체 state + 스프레드 연산자로 여러 input을 하나의 함수로 처리

---

## 4. 코드 해석 — 01_useState.html

### 상단 세팅
```html
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```
- CDN으로 `react`, `react-dom`, `babel`을 불러옴 → 빌드 도구(webpack 등) 없이 브라우저에서 바로 JSX 사용 가능
- `<script type="text/babel">`: 이 안의 코드는 Babel이 실행 시점에 JSX → 순수 JS로 변환

### MessageManager 함수 — 한 줄씩 해석
```jsx
const { useState } = React;
```
- `React` 객체 안의 `useState` 함수를 구조분해할당으로 꺼냄. 이후 `React.useState()` 대신 `useState()`로 바로 호출 가능

```jsx
const result = useState('초기값');
console.log(result);
```
- `useState('초기값')`을 호출하면 **배열**이 반환됨: `[현재 상태값, 상태를 바꾸는 setter 함수]`
- `result`는 `['초기값', function setter(){...}]` 형태 → 콘솔에 찍어서 구조를 직접 확인해보는 실습 코드

```jsx
const [message, setMessage] = useState('초기 상태 메세지입니다.');
const [textColor, setTextColor] = useState('black');
```
- 위 `result` 예시를 실전 문법으로 사용: 배열 구조분해할당으로 `message`(현재값)와 `setMessage`(변경 함수)를 동시에 선언
- `textColor`도 동일한 방식으로 별도의 state로 관리 → **하나의 컴포넌트에 여러 개의 useState를 쓸 수 있음**

```jsx
console.log('MessageManager 컴포넌트가 렌더링 되었습니다.')
```
- 컴포넌트 함수 본문에 있는 코드이므로, **렌더링될 때마다(최초 + 리렌더링 시) 매번 실행**됨 → 버튼 클릭 → setter 호출 → 리렌더링이 실제로 일어나는지 콘솔로 확인하기 위한 코드

```jsx
const handleEnter = () => {
    setMessage('안녕하세요, 환영합니다.');
    setTextColor('blue');
};
```
- 버튼 클릭 시 실행될 이벤트 핸들러. `setMessage`, `setTextColor`를 호출 → 두 state가 바뀌면서 컴포넌트가 리렌더링 → 화면의 문구와 색상이 동시에 바뀜

```jsx
return (
    <>
        <h1>함수형 컴포넌트(useState)</h1>
        <h2 style={{ color: textColor }}>{message}</h2>
        <button onClick={handleEnter}>입장</button>
        <button onClick={handleLeave}>퇴장</button>
    </>
)
```
- `<>...</>`: Fragment. 실제 DOM에 불필요한 wrapper 태그를 추가하지 않고 여러 요소를 묶어서 반환하기 위함
- `style={{ color: textColor }}`: JSX의 인라인 스타일은 **객체** 형태로 작성 (바깥 `{}`는 JS 표현식 삽입, 안쪽 `{}`는 style 객체)
- `{message}`: state 값을 JSX 안에 그대로 출력
- `onClick={handleEnter}`: 함수 자체를 전달(실행 결과 `handleEnter()`가 아님) → 클릭 시점에 React가 호출

### Counter 함수 — 한 줄씩 해석
```jsx
const [number, setNumber] = useState(0);
```
- 숫자 상태 `number`를 0으로 초기화

```jsx
const increaseByTwo = () => {
    setNumber((prev) => prev + 1);
    setNumber((prev) => prev + 1);
}
```
- setter에 **함수**를 전달(함수형 업데이트) → React가 대기 중인 상태 변경을 순서대로 적용하면서 `prev`에 최신 값을 넣어줌
- 첫 번째 호출: `prev = 0` → `1` / 두 번째 호출: `prev = 1`(방금 계산된 값) → `2` → 최종적으로 +2 성공

```jsx
<button onClick={() => setNumber(number + 1)}>+1</button>
<button onClick={() => setNumber(number - 1)}>-1</button>
```
- 화살표 함수로 감싸서 전달하는 이유: `onClick={setNumber(number+1)}`처럼 바로 호출하면 **렌더링되는 순간 즉시 실행**되어버리기 때문 (클릭 시 실행되게 하려면 함수로 감싸야 함)

```jsx
<button onClick={() => {
    setNumber(number + 1);
    setNumber(number + 1);
}}>+2(의도대로 안됨)</button>
```
- 클릭 한 번으로 `setNumber(number + 1)`을 두 번 호출하지만, 두 호출 모두 **같은 렌더링 시점의 `number`(클로저에 갇힌 값)**를 참조
- 예: `number`가 0일 때 클릭 → 두 호출 모두 `setNumber(0 + 1)` → 최종 결과는 1 (의도한 2가 아님)

```jsx
<button onClick={increaseByTwo}>+2(함수형 업데이트)</button>
```
- 위에서 정의한 `increaseByTwo`를 연결 → 클릭 시 정상적으로 +2

### App 함수 & 렌더링
```jsx
function App() {
    return (
        <>
            <MessageManager />
            <hr />
            <Counter />
        </>
    )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```
- `App`은 `MessageManager`와 `Counter`를 조합하는 **최상위 컴포넌트**
- `ReactDOM.createRoot(...)`: React 18의 새로운 렌더링 API로 `#root` DOM 요소를 React가 관리하는 루트로 지정
- `.render(<App />)`: `App` 컴포넌트를 해당 DOM에 실제로 그려냄 (React 앱의 진입점)

---

## 5. 코드 해석 — 01_event-handling.html

### SignupFormBasic — 한 줄씩 해석
```jsx
const [username, setUsername] = useState('');
const [email, setEmail] = useState('');
```
- 두 입력 필드를 각각 독립된 state로 관리. 초기값은 빈 문자열(`''`) → 빈 input으로 시작

```jsx
const handleSignup = () => {
    alert(`아이디: ${username} \n 이메일: ${email} \n 회원가입 완료.`)
    setUsername('');
    setEmail('');
}
```
- 템플릿 리터럴(백틱)로 `username`, `email` state 값을 문자열에 삽입해 alert로 표시
- 가입 완료 후 두 state를 다시 빈 문자열로 초기화 → **리렌더링되면서 input도 함께 비워짐** (state가 input의 값을 제어하고 있기 때문)

```jsx
<input
    type="text"
    placeholder="아이디"
    value={username}
    onChange={e => setUsername(e.target.value)} />
```
- `value={username}`: input에 표시되는 값이 **state에 종속**됨 (제어 컴포넌트, controlled component)
- `onChange`: 사용자가 타이핑할 때마다 발생하는 이벤트. `e`는 이벤트 객체
- `e.target`: 이벤트가 발생한 DOM 요소(이 input 자신), `e.target.value`: 사용자가 방금 입력해서 바뀐 최신 문자열
- 이 값을 `setUsername`에 넘겨 state를 갱신 → 리렌더링 → `value={username}`이 새 값으로 다시 그려짐 (이 흐름이 있어야 실제로 타이핑한 게 화면에 보임)

```jsx
<button onClick={handleSignup}>가입하기</button>
```
- 클릭 시 `handleSignup` 함수 실행

### SignupFormAdvanced — 한 줄씩 해석
```jsx
const [form, setForm] = useState({
    username: '',
    email: ''
});
const { username, email } = form;
```
- 여러 필드를 **하나의 객체**로 묶어서 단일 state로 관리
- `form.username`, `form.email`처럼 매번 쓰지 않도록 구조분해할당으로 `username`, `email` 변수를 꺼내둠 (읽기 전용 참조, 이 값들을 직접 재할당하진 않음)

```jsx
const handleChange = (e) => {
    const { name, value } = e.target;
    console.log(name, value);

    setForm({
        ...form,
        [name]: value
    });
}
```
- `e.target`에서 `name`(input에 지정된 name 속성 값)과 `value`(입력된 최신 값)를 동시에 꺼냄
- `{ ...form }`: 스프레드 연산자로 기존 `form` 객체의 모든 key-value를 얕은 복사
- `[name]: value`: **계산된 속성명(computed property name)** 문법. `name` 변수에 담긴 문자열("username" 또는 "email")을 실제 key로 사용해 그 자리만 새 값으로 덮어씀
- 예: username input에서 입력 발생 → `name = "username"` → `{ ...form, username: value }`가 되어 `email`은 그대로, `username`만 갱신됨
- **왜 객체를 새로 만드는가**: React는 state가 바뀌었는지 참조(reference)로 비교하므로, 기존 객체를 직접 수정(`form.username = value`)하면 React가 변경을 감지하지 못함 → 반드시 새 객체를 만들어 `setForm`에 전달해야 함

```jsx
const handleSignup = () => {
    alert(`아이디: ${username}, 이메일: ${email}`)
    setForm({ username: '', email: '' });
}
```
- 구조분해된 `username`, `email`로 alert 표시 후, `form` 전체를 초기 상태 객체로 리셋

```jsx
<input
    type="text"
    name="username"
    placeholder="아이디"
    value={username}
    onChange={handleChange} />

<input
    type="email"
    name='email'
    placeholder="이메일"
    value={email}
    onChange={handleChange} />
```
- 두 input 모두 **같은 `handleChange` 함수 하나**를 공유
- 각 input의 `name` 속성이 `form` 객체의 key(`username`, `email`)와 정확히 일치해야 `handleChange` 내부의 `[name]: value` 로직이 올바른 필드를 갱신할 수 있음 → 이 이름 일치가 이 패턴의 핵심 전제 조건

### App 함수
```jsx
function App() {
    return (
        <>
            <SignupFormBasic />
            <hr />
            <SignupFormAdvanced />
        </>
    )
}
```
- 두 방식(Basic/Advanced)의 폼을 한 화면에서 비교할 수 있도록 나란히 렌더링
