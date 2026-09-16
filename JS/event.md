# JavaScript 이벤트(Event) 총정리
---

## 1. 이벤트 핸들러 등록/제거 (`01_event-handling.html`)

### 방법 1. 인라인 어트리뷰트 (구식)
```html
<button onclick="alert('클릭!');">버튼</button>
```
HTML과 JS가 섞여 유지보수가 어려움 → **비권장**.

### 방법 2. 이벤트 핸들러 프로퍼티 (과도기)
```js
btn.onclick = function () { console.log('임무1'); };
btn.onclick = function () { console.log('임무2'); }; // 이전 함수를 덮어씀
```
하나의 이벤트에 **하나의 함수만** 저장 가능 (변수처럼 동작).

### 방법 3. `addEventListener` (표준)
```js
btn.addEventListener('click', fn1);
btn.addEventListener('click', fn2); // 둘 다 실행됨 (여러 개 등록 가능)
```

### 이벤트 핸들러 제거
```js
const mission = () => alert('임무');
btn.addEventListener('click', mission);
btn.removeEventListener('click', mission);
```
> ⚠️ **익명 함수는 제거 불가.** `removeEventListener`에 새로 만든 익명 함수를 넘기면 참조가 달라서 제거되지 않음. 반드시 **변수에 저장한 함수**를 등록/제거 양쪽에 사용해야 함.

---

## 2. 이벤트 객체와 `this` (`02_event-object-and-this.html`)

- 이벤트 발생 시 브라우저가 **이벤트 객체**를 만들어 핸들러의 **첫 번째 인자**로 전달.
- `e.clientX`, `e.clientY` : 뷰포트 기준 마우스 좌표

### `this` vs `e.target` vs `e.currentTarget`
| 구분                    | 의미                                                                          |
|------------------------|------------------------------------------------------------------------------|
| `e.target`             | 이벤트가 **실제로 발생(시작)한** 요소                                            |
| `e.currentTarget`      | 이벤트 **핸들러가 등록된** 요소                                                 |
| `this` (일반 function)  | `e.currentTarget`과 동일                                                    |
| `this` (화살표 함수)     | 자신의 `this`가 없어 **바깥 스코프의 this**를 그대로 사용 (보통 예상과 다르게 동작) |

```js
btn.addEventListener('click', function (e) {
  console.log(this === e.currentTarget); // true
});

btn.addEventListener('mouseover', (e) => {
  console.log(this); // 바깥 스코프의 this (버튼이 아님!)
  console.log(e.currentTarget); // 항상 버튼
});
```

> ✅함수 종류와 상관없이 일관되게 동작하는 `e.currentTarget`을 쓰는 것이 버그를 줄이는 습관.

---

## 3. 이벤트 전파와 위임 (`03_event-propagation.html`)

### 이벤트 전파 3단계
```
1. 캡처링(Capturing): window → 이벤트 발생 요소 방향으로 내려감
2. 타깃(Target): 실제 이벤트 대상에 도착
3. 버블링(Bubbling): 이벤트 대상 → 부모 방향으로 올라감
```

파일에는 **버블링**만 실습되어 있고 **캡처링 실습 코드가 빠져 있어 추가**

### ➕ 캡처링 단계
`addEventListener`의 세 번째 인자를 `true`로 주면 캡처링 단계에서 실행됩니다.
```js
$parent.addEventListener('click', () => {
  console.log('부모 - 캡처링 단계에서 실행');
}, true); // capture: true

$child.addEventListener('click', () => {
  console.log('자식 - 타깃 단계에서 실행');
});
// 클릭 시 순서: 부모(캡처링) → 자식(타깃) → 부모(버블링, capture 미설정 리스너가 따로 있다면)
```

### 이벤트 위임 (Event Delegation)
자식 요소마다 리스너를 달지 않고, **부모에 하나만** 등록해 버블링을 활용.
```js
$drinkList.addEventListener('click', (e) => {
  const $item = e.target.closest('li'); // 가장 가까운 li 탐색
  if (!$item) return;
  if (!$drinkList.contains($item)) return;
  highlight($item);
});
```
- `closest()` : 자기 자신부터 시작해 조상 방향으로 조건에 맞는 요소를 찾음 (없으면 `null`)
- 장점: 코드가 간결하고, **동적으로 추가된 자식 요소**에도 별도 처리 없이 자동 적용됨.

---

## 4. 이벤트 제어 (`04_event-control.html`)

### `preventDefault()` — 브라우저의 기본 동작 취소
```js
$loginForm.addEventListener('submit', (e) => {
  e.preventDefault(); // 페이지 새로고침/이동 방지
  // 유효성 검사 로직...
});

$input.addEventListener('contextmenu', (e) => {
  e.preventDefault(); // 우클릭 메뉴 방지
});
```

### `stopPropagation()` — 버블링/캡처링 전파 중단
```js
$modalContent.addEventListener('click', (e) => {
  e.stopPropagation(); // 부모(모달 배경)로 이벤트가 전파되지 않음
});
```

### ➕ `stopImmediatePropagation()` 
`stopPropagation()`은 **상위 요소로의 전파만** 막지만, `stopImmediatePropagation()`은 **같은 요소에 등록된 다른 리스너 실행까지** 막습니다.
```js
btn.addEventListener('click', (e) => {
  e.stopImmediatePropagation();
  console.log('첫 번째 리스너');
});
btn.addEventListener('click', () => {
  console.log('두 번째 리스너'); // 실행되지 않음!
});
```

---

## 5. ➕ `addEventListener`의 옵션 객체

`addEventListener(type, handler, options)`의 세 번째 인자로 객체를 넘기면 세밀하게 제어할 수 있습니다.

```js
element.addEventListener('click', handler, {
  once: true,     // 한 번 실행 후 자동으로 제거됨
  capture: true,  // 캡처링 단계에서 실행
  passive: true   // preventDefault를 호출하지 않겠다고 미리 약속 (스크롤 성능 최적화)
});
```

- `once: true` : 회원가입 완료 버튼처럼 **한 번만 실행**되어야 하는 핸들러에 유용 (수동으로 `removeEventListener` 호출할 필요 없음).
- `passive: true` : `touchstart`, `wheel`, `scroll` 같은 이벤트에서 **스크롤 성능**을 높여줌. 대신 이 옵션을 쓰면 내부에서 `preventDefault()`를 호출해도 무시됨(경고 발생 가능).

---

## 6. 마우스 이벤트 (`05_mouse-and-keyboard-events.html`)

| 이벤트                         | 발생 시점                                   |
|-------------------------------|-------------------------------------------|
| `click`                       | 마우스 왼쪽 버튼을 눌렀다 뗄 때               |
| `dblclick`                    | 더블 클릭                                  |
| `mousedown`                   | 버튼을 누르는 순간                          |
| `mouseup`                     | 버튼에서 손을 떼는 순간                     |
| `mousemove`                   | 마우스가 움직일 때                         |
| `mouseenter` / `mouseleave`   | 요소에 들어오고 나갈 때 (버블링 없음)        |
| `mouseover` / `mouseout`      | 요소(+자식)에 들어오고 나갈 때 (버블링 있음) |

```js
$btn.addEventListener('mousedown', (e) => {
  console.log(e.button); // 0: 왼쪽, 1: 휠(가운데), 2: 오른쪽
});
```

> ➕ `mouseenter`/`mouseleave`는 파일에 등장하지 않았지만, `mouseover`/`mouseout`과 자주 비교되는 짝 개념이라 참고로 추가합니다. 둘의 차이는 **버블링 여부**와 **자식 요소 경계 통과 시 재발생 여부**입니다.

---

## 7. 키보드 이벤트 (`05_mouse-and-keyboard-events.html`)

| 이벤트     | 설명                                    |
|-----------|----------------------------------------|
| `keydown` | 키를 누르는 순간 (누르고 있으면 계속 발생)  |
| `keyup`   | 키에서 손을 떼는 순간                    |
| `input`   | 실제 값이 변경된 직후 (가장 최신 값 반영)  |

- `e.key` : 입력된 **문자** (예: `'a'`, `'A'`, `'Enter'`)
- `e.code` : 실제 **물리 키 코드** (예: `'KeyA'`, `'Enter'`) — 키보드 레이아웃과 무관하게 동일

```js
$input.addEventListener('keydown', (e) => {
  if (e.key === 'Enter') {
    console.log('검색 실행');
  }
});
```
> 💡 방금 입력된 문자까지 반영된 **최신 값**이 필요하면 `keydown/keyup`보다 `input` 이벤트가 더 적합 (한글 조합 등에서도 안전).

---

## 8. 폼 이벤트 (`06_form-events.html`)

| 이벤트    | 설명                                                         |
|----------|-------------------------------------------------------------|
| `focus`  | 요소에 포커스가 들어올 때                                      |
| `blur`   | 요소에서 포커스가 빠져나갈 때                                  |
| `input`  | 값이 바뀔 때마다 (타이핑마다)                                 |
| `change` | 값이 바뀌고 **포커스를 벗어날 때** (체크박스/셀렉트는 선택 즉시)  |
| `submit` | 폼이 제출될 때                                             |

```js
$form.forms.joinForm; // document.forms.폼이름
$form.username;       // name 속성으로 요소 접근

$introduce.addEventListener('input', (e) => {
  $counter.textContent = e.target.value.length;
});

$form.addEventListener('submit', (e) => {
  if ($username.value.trim().length < 2) {
    e.preventDefault();
    $error.textContent = '이름은 2글자 이상 입력해주세요.';
    return;
  }
});
```

### 🐛 06번 파일

```js
const checkedCount =
    Array.from($hobbies)
        .filter((hobby) => hobby.checked)
        .length;

if (checkedCount < 2) {
  e.preventDefault();
  $error.textContent = '관심사는 2개 이상 선택해 주세요.';
  return;
}
```
> ➕ `change` 이벤트도 파일에 없었지만 체크박스(`hobby`)는 `change` 이벤트로 선택 즉시 감지하는 것이 더 자연스러운 사용 예이므로 참고로 언급합니다.

---

## 9. ➕ 윈도우 / 문서 이벤트

폼·마우스·키보드 이벤트만 다뤄서, 페이지/윈도우 단위 이벤트가 빠져 있었습니다.

```js
// 문서(HTML) 파싱이 끝났을 때 (이미지 등 리소스는 기다리지 않음)
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM 준비 완료');
});

// 모든 리소스(이미지, CSS 등)까지 로드가 끝났을 때
window.addEventListener('load', () => {
  console.log('페이지 전체 로드 완료');
});

// 창 크기가 바뀔 때
window.addEventListener('resize', () => {
  console.log(window.innerWidth, window.innerHeight);
});

// 스크롤할 때
window.addEventListener('scroll', () => {
  console.log(window.scrollY);
});

// 탭을 닫거나 새로고침하기 직전
window.addEventListener('beforeunload', (e) => {
  e.preventDefault();
  e.returnValue = ''; // 저장하지 않은 내용이 있을 때 브라우저 경고창 표시
});
```

---

## 10. ➕ 커스텀 이벤트

지금까지는 브라우저가 자동으로 발생시키는 이벤트만 다뤘지만, **직접 이벤트를 만들어 발생**시킬 수도 있습니다.

```js
// 이벤트 생성 (detail에 원하는 데이터를 담을 수 있음)
const myEvent = new CustomEvent('login-success', {
  detail: { username: '홍길동' }
});

// 이벤트 리스너 등록
document.addEventListener('login-success', (e) => {
  console.log(`${e.detail.username}님, 환영합니다!`);
});

// 이벤트 발생시키기
document.dispatchEvent(myEvent);
```
컴포넌트 간 통신이나, 라이브러리 없이 상태 변화를 알릴 때 유용합니다.

---

## 11. ➕ 디바운스(Debounce) / 스로틀(Throttle)

`input`, `resize`, `scroll`처럼 **짧은 시간에 매우 자주 발생하는 이벤트**는 매번 처리하면 성능 문제가 생길 수 있습니다.

```js
// 디바운스: 마지막 입력 후 일정 시간이 지나야 실행 (검색어 자동완성 등)
function debounce(fn, delay = 300) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

$searchInput.addEventListener('input', debounce((e) => {
  console.log('검색 요청:', e.target.value);
}, 300));
```
```js
// 스로틀: 일정 시간 간격으로만 실행 (스크롤 위치 추적 등)
function throttle(fn, interval = 200) {
  let waiting = false;
  return (...args) => {
    if (waiting) return;
    fn(...args);
    waiting = true;
    setTimeout(() => (waiting = false), interval);
  };
}

window.addEventListener('scroll', throttle(() => {
  console.log(window.scrollY);
}, 200));
```

---

## 전체 정리.

| 주제          | 핵심 키워드                                                          |
|--------------|--------------------------------------------------------------------|
| 등록 방법     | 인라인 어트리뷰트 / 프로퍼티 / `addEventListener`                      |
| 제거         | 이름 있는 함수 참조 필요, 익명 함수는 제거 불가                          |
| 이벤트 객체   | `e.target`, `e.currentTarget`, `this`                             |
| 전파         | 캡처링 → 타깃 → 버블링, 이벤트 위임                                   |
| 제어         | `preventDefault`, `stopPropagation`, `stopImmediatePropagation`  |
| 옵션         | `once`, `capture`, `passive`                                     |
| 마우스/키보드 | `click`, `mousedown`, `keydown`, `e.key` vs `e.code`             |
| 폼           | `focus`, `blur`, `input`, `change`, `submit`                    |
| 윈도우/문서   | `DOMContentLoaded`, `load`, `resize`, `scroll`, `beforeunload`  |
| 커스텀 이벤트 | `CustomEvent`, `dispatchEvent`                                  |
| 성능         | 디바운스, 스로틀                                                  |