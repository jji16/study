# JavaScript DOM 조작 정리노트

---

## 1. DOM 요소 선택하기

### 1-1. `querySelector` / `querySelectorAll` — CSS 선택자 기반

```js
const $apple = document.querySelector('#apple');      // 조건에 맞는 '첫 번째' 요소 1개
const $fruits = document.querySelectorAll('.fruit');  // 조건에 맞는 '모든' 요소 → NodeList
```

- CSS 선택자 문법(`#id`, `.class`, `tag`, 복합 선택자 등)을 그대로 사용할 수 있어 가장 범용적입니다.
- `querySelector`는 일치하는 요소가 없으면 `null`을 반환하므로, 사용 전에 존재 여부를 확인하는 것이 안전합니다.
- `querySelectorAll`이 반환하는 `NodeList`는 `forEach`를 바로 사용할 수 있지만, `map`/`filter` 같은 배열 메서드는 없습니다. 필요하면 `[...NodeList]` 또는 `Array.from(NodeList)`으로 배열 변환이 필요합니다.

### 1-2. `getElementById` / `getElementsByClassName` — 구식이지만 여전히 사용됨

```js
document.getElementById('id값');            // 요소 1개 (없으면 null)
document.getElementsByClassName('클래스명'); // HTMLCollection (여러 개)
```

### 1-3. 핵심 개념: NodeList(정적) vs HTMLCollection(라이브)

| 구분                        | 반환 메서드                                                    | 특징 |
|----------------------------|--------------------------------------------------------------|---------------------------------------------------------|
| **NodeList (정적)**         | `querySelectorAll`                                           | 선택 시점의 스냅샷. 이후 DOM이 바뀌어도 목록 자체는 변하지 않음 |
| **HTMLCollection (라이브)** | `getElementsByClassName`, `getElementsByTagName`, `children` | DOM 변경 사항이 **실시간으로 목록에 반영됨**                 |

**라이브 컬렉션을 순회하며 수정하면 왜 문제가 생기는가? (01번 파일 보충 설명)**

```js
const $texts = $box.getElementsByClassName('text'); // 라이브 HTMLCollection, 초기 length: 3

for (let i = 0; i < $texts.length; i++) {
    $texts[i].classList.remove('text');
}
```

이 코드가 "첫 번째, 세 번째만 클래스가 제거되는" 이유:

1. `i = 0`: `$texts[0]`(첫 번째 p)에서 `text` 클래스 제거 → 이 요소는 더 이상 `.text`가 아니므로 **즉시 컬렉션에서 빠짐**. 컬렉션은 `[두 번째, 세 번째]`가 되고 `length`는 3 → 2로 줄어듦.
2. `i = 1`: 줄어든 컬렉션의 인덱스 1은 **세 번째 p**가 됨(두 번째가 아님!). 세 번째 p의 클래스가 제거됨 → 컬렉션은 `[두 번째]`, length는 1.
3. `i = 2`: `2 < 1`이 거짓이므로 반복 종료. **두 번째 p는 끝까지 건드려지지 않음.**

즉 인덱스가 밀리면서 요소를 건너뛰는 것이지, "인덱스 번호가 변경"된다기보다는 **컬렉션 자체가 실시간으로 줄어들기 때문**입니다.

### 1-4. 라이브 목록을 안전하게 순회하는 방법

```js
const $texts2 = document.getElementsByClassName('text2');
const textArray = [...$texts2]; // 스프레드 문법으로 '고정된' 일반 배열로 변환

textArray.forEach(item => item.classList.remove('text2'));
```

배열로 복사한 순간의 스냅샷을 순회하므로, 원본 라이브 컬렉션이 실시간으로 줄어들어도 순회에 영향을 받지 않습니다.

> **정리**: 순회 중 요소를 추가/삭제/변경해야 한다면 `querySelectorAll`(정적 NodeList)을 쓰거나, 라이브 컬렉션을 배열로 변환한 뒤 순회하세요.

---

## 2. 노드 탐색 (Node Traversal)

기준 요소를 하나 찾은 뒤, 그 요소의 부모·자식·형제 관계를 따라가며 탐색합니다.

### 2-1. 자식 탐색

| 프로퍼티                                     | 반환 대상                                 | 텍스트/주석 노드 포함 여부                    |
|--------------------------------------------|------------------------------------------|--------------------------------------------|
| `.childNodes`                              | 모든 자식 **노드** (NodeList)              | 포함 (공백, 줄바꿈, 주석까지 모두 노드로 포함됨) |
| `.children`                                | 모든 자식 **요소** (HTMLCollection, 라이브) | 미포함                                      |
| `.firstElementChild` / `.lastElementChild` | 첫/마지막 자식 **요소**                     | 미포함                                      |
| `.firstChild` / `.lastChild`               | 첫/마지막 자식 **노드**                     | 포함 (공백 텍스트 노드가 걸릴 수 있어 주의)     |

```js
const $family = document.getElementById('family');
$family.childNodes;              // 공백/주석 텍스트 노드까지 포함된 NodeList
$family.children;                // 실제 <li> 요소만 담긴 HTMLCollection
$family.firstElementChild;       // 첫 번째 <li>
```

> 보충: HTML 코드에서 태그 사이의 줄바꿈이나 공백도 하나의 **텍스트 노드**로 취급됩니다. 그래서 `.firstChild`를 썼을 때 예상과 달리 빈 텍스트 노드가 잡히는 경우가 흔합니다. 이런 혼란을 피하려면 이름에 `Element`가 들어간 프로퍼티(`children`, `firstElementChild` 등)를 쓰는 것이 안전합니다.

### 2-2. 부모 탐색

```js
$me.parentNode;     // 부모 노드 (요소가 아닌 노드가 올 수도 있음, 예: document)
$me.parentElement;  // 부모가 반드시 '요소'일 때 사용 (더 안전, 없으면 null)
```

### 2-3. 형제 탐색

```js
$me.nextElementSibling;      // 다음 형제 요소
$me.previousElementSibling;  // 이전 형제 요소
```

`.nextSibling` / `.previousSibling`도 존재하지만 텍스트 노드(공백)를 포함해서 반환하므로, 요소만 다룰 때는 `Element`가 붙은 프로퍼티를 사용하는 것이 원칙입니다.

---

## 3. 노드 정보 확인 및 텍스트 조작

### 3-1. `nodeType` — 노드의 종류 구분

| 값 | 종류                |
|---|---------------------|
| 1 | 요소 노드 (Element)  |
| 3 | 텍스트 노드 (Text)   |
| 8 | 주석 노드 (Comment)  |
| 9 | 문서 노드 (Document) |

```js
$infoArea.nodeType;      // 1 (요소)
$infoArea.firstChild.nodeType; // 3 (텍스트)
```

### 3-2. 텍스트 조작 방법 비교

| 프로퍼티        | 적용 대상               | 특징 |
|----------------|------------------------|-------------------------------------------------------------------------------------------------------------------|
| `.nodeValue`   | **텍스트 노드에만**      | 요소 노드에 사용하면 `null`. 자식 텍스트 노드를 직접 찾아가야 하는 번거로움                                                 |
| `.textContent` | **요소 노드**에 바로 사용 | 자손의 텍스트를 모두 합쳐서 반환/치환. HTML 태그는 문자열로 취급(파싱 안 됨)                                                |
| `.innerText`   | 요소 노드               | **화면에 렌더링된** 텍스트 기준(CSS `display:none` 등 영향을 받음). 레이아웃을 다시 계산하므로 `textContent`보다 성능이 떨어짐 |

```js
$contentArea.textContent = 'textContent로 쉽게 변경완료!! <span>태그는?</span>';
```

> ⚠️ 보충 설명: 위 코드를 실행하면 `<span>` 태그가 **HTML로 파싱되지 않고 화면에 그대로 문자열(`<span>태그는?</span>`)로 출력**됩니다. `textContent`는 항상 "순수 텍스트"로만 취급하기 때문입니다. HTML 태그를 실제로 렌더링하려면 `innerHTML`을 사용해야 합니다(단, 보안 문제는 4장 참고).

**언제 무엇을 쓸까?**
- 자식 텍스트 노드 하나만 정밀하게 다루고 싶다 → `nodeValue` (실무에서는 잘 안 씀)
- 요소 내부 텍스트를 통째로 읽거나 바꾸고 싶다 → `textContent` (기본 선택)
- 화면에 실제로 "보이는" 텍스트만 필요하다(줄바꿈, 숨김처리 반영) → `innerText`

---

## 4. DOM 요소 생성·삽입·삭제

### 4-1. `innerHTML` — 내부 전체 교체

```js
$area1.innerHTML = '<ul><li>새로운 리스트 아이템</li></ul>';
```

- 문자열을 실제 HTML로 **파싱**해서 기존 내용을 통째로 교체합니다.
- `textContent`는 문자열을 그대로 텍스트로 넣지만, `innerHTML`은 태그를 해석해서 요소를 만듭니다.

> ⚠️ **보안 보충 설명 (XSS)**: 사용자가 입력한 값을 검증 없이 `innerHTML`에 그대로 넣으면, `<img src=x onerror="...">`처럼 악성 스크립트가 실행될 수 있는 **XSS(Cross-Site Scripting) 공격**에 노출됩니다. 사용자 입력을 화면에 출력할 때는 기본적으로 `textContent`를 사용하고, HTML 마크업이 꼭 필요한 경우에만 신뢰할 수 있는 데이터에 한해 `innerHTML`을 사용하세요. (실무에서는 DOMPurify 같은 sanitize 라이브러리를 함께 쓰는 것이 일반적입니다.)

### 4-2. `insertAdjacentHTML` — 특정 위치에 삽입

기존 내용을 유지하면서 지정한 위치에 HTML을 삽입합니다. 위치는 대상 요소를 기준으로 4곳입니다.

```
<!-- beforebegin -->
<div id="area2">
  <!-- afterbegin -->
  내용
  <!-- beforeend -->
</div>
<!-- afterend -->
```

```js
$area2.insertAdjacentHTML('beforebegin', '<p>...</p>'); // 대상 요소 바깥, 앞
$area2.insertAdjacentHTML('afterbegin', '<p>...</p>');  // 대상 요소 안쪽, 맨 앞
$area2.insertAdjacentHTML('beforeend', '<p>...</p>');   // 대상 요소 안쪽, 맨 뒤
$area2.insertAdjacentHTML('afterend', '<p>...</p>');    // 대상 요소 바깥, 뒤
```

> 보충: 텍스트만 삽입할 때는 `insertAdjacentText`, 이미 만들어진 노드(Element)를 삽입할 때는 `insertAdjacentElement`도 같은 4가지 위치 인자를 지원합니다.

### 4-3. `createElement` + `appendChild` — 요소를 직접 조립

```js
const $newLi = document.createElement('li'); // 1. 노드 생성
$newLi.textContent = '콜라';                  // 2. 내용 채우기
$drinkList.appendChild($newLi);               // 3. 원하는 위치에 삽입
```

- HTML 문자열을 파싱하지 않으므로 `innerHTML`보다 **성능이 좋고 XSS로부터 안전**합니다.
- 여러 요소를 한 번에 추가할 때 매번 `appendChild`로 실제 DOM을 건드리면 리플로우(reflow)가 반복되어 비효율적입니다. 이때 `DocumentFragment`를 사용합니다.

```js
const $fragment = document.createDocumentFragment(); // 화면에 그려지지 않는 임시 조립 공간
['사이다', '우유'].forEach(text => {
    const $li = document.createElement('li');
    $li.textContent = text;
    $fragment.appendChild($li);   // 임시 공간에서 미리 조립
});
$drinkList.appendChild($fragment); // 실제 DOM에는 한 번만 반영 (성능 이점)
```

> 참고: `appendChild(fragment)`가 실행되면 fragment 내부의 자식 노드들만 옮겨지고, fragment 자체는 비워집니다(fragment는 DOM 트리에 남지 않음).

### 4-4. 정밀 조작: 삽입·이동·교체·삭제

```js
// 1. 특정 노드 앞에 삽입 — 부모.insertBefore(새노드, 기준노드)
$foodList.insertBefore($newFood, $pizza);

// 2. 노드 이동 — 이미 DOM에 있는 노드를 appendChild/insertBefore로 옮기면
//    복사가 아니라 '이동'되며, 기존 위치에서는 자동으로 제거된다.
$drinkList2.appendChild($pizza);

// 3. 노드 교체 — 부모.replaceChild(새노드, 기존노드)
$drinkList.replaceChild($newDrink, $drinkList2.firstElementChild);

// 4. 노드 삭제
$foodList.removeChild($pasta);  // 부모가 자식을 지정해서 제거 (구식 방식)
$pizza.remove();                // 요소 스스로 제거 (최신 방식, 더 간결)
```

> 보충: `insertBefore`의 두 번째 인자(기준 노드)는 **반드시 첫 번째 인자를 호출한 부모의 실제 자식**이어야 합니다. 그렇지 않으면 에러가 발생합니다. 최신 브라우저에서는 `$foodList.insertBefore(A, B)` 대신 `B.before(A)` 같은 더 짧은 메서드(`before`, `after`, `append`, `prepend`, `replaceWith`)도 사용할 수 있습니다.

---

## 5. 어트리뷰트(Attribute) vs 프로퍼티(Property)

### 5-1. 기본 조작 메서드

```js
$input.getAttribute('value');            // 읽기
$input.setAttribute('value', 'panda');   // 쓰기(없으면 추가, 있으면 변경)
$input.hasAttribute('class');            // 존재 확인 → true/false
$input.removeAttribute('value');         // 삭제
```

### 5-2. 어트리뷰트와 프로퍼티의 차이 

- **어트리뷰트(attribute)**: HTML 소스 코드에 작성된 **초기값**. `getAttribute`로 읽으면 최초 설정값이 그대로 유지됩니다.
- **프로퍼티(property)**: 브라우저가 HTML을 파싱해서 만든 **DOM 객체의 현재 상태(실시간 값)**. 사용자의 입력이나 스크립트에 의해 계속 바뀝니다.

```js
const $nickname = document.getElementById('nickname'); // value="JSBeginner"

$nickname.oninput = () => {
    console.log('프로퍼티: ', $nickname.value);             // 사용자가 지금 입력 중인 값
    console.log('어트리뷰트: ', $nickname.getAttribute('value')); // 여전히 'JSBeginner' (변하지 않음)
};
```

- 대부분의 표준 속성(`id`, `class` 등)은 어트리뷰트와 프로퍼티가 항상 동기화되지만, `value`처럼 **사용자 상호작용으로 값이 바뀌는 속성**은 어트리뷰트(초기값)와 프로퍼티(현재값)가 분리되어 관리됩니다.
- `checked`, `selected`처럼 **boolean 속성**은 타입도 다릅니다:

```js
$checkbox.getAttribute('checked'); // null (어트리뷰트를 설정한 적 없으므로) → 문자열 또는 null
$checkbox.checked;                 // false (프로퍼티는 항상 boolean 타입)
```

> **실무 원칙**: 정적인 초기값을 다루거나 HTML 구조 자체를 확인할 때는 attribute 메서드를, 사용자와 상호작용하는 **현재 상태값**을 다룰 때는 property(`.value`, `.checked` 등)를 사용합니다.

### 5-3. `data-*` 어트리뷰트와 `.dataset` (05번 파일 보충 — 실전 예제 추가)

원본 파일의 `<script>`는 `boardList` 변수만 선언되어 있고 실제 활용 예제가 비어 있었습니다. 아래에 채워 넣습니다.

```html
<ul class="board-list">
    <li data-board-id="13" data-category-name="맛집">망원 맛집 추천 부탁드려요!</li>
    <li data-board-id="15" data-category-name="운동">한강 러닝 크루 구해요!</li>
    <li data-board-id="18" data-category-name="경제">요즘 재태크 어떻게 하시나요?</li>
</ul>
```

```js
const boardList = document.querySelector('.board-list');

// 1. 읽기: data-board-id → dataset.boardId (케밥 케이스가 카멜 케이스로 자동 변환됨)
boardList.querySelectorAll('li').forEach(li => {
    console.log(li.dataset.boardId, li.dataset.categoryName);
    // '13' 맛집 / '15' 운동 / '18' 경제  → dataset 값은 항상 '문자열' 타입
});

// 2. 클릭한 게시글의 data 값 활용 (이벤트 위임)
boardList.addEventListener('click', (e) => {
    const $li = e.target.closest('li');
    if (!$li) return;

    const boardId = $li.dataset.boardId;         // 예: '15'
    const category = $li.dataset.categoryName;    // 예: '운동'
    console.log(`${category} 게시판의 ${boardId}번 글로 이동합니다.`);
    // location.href = `/board/${boardId}`; 같은 실제 페이지 이동에 활용 가능
});

// 3. 쓰기: dataset에 새 값을 대입하면 data-* 어트리뷰트가 자동 생성/변경됨
$li && ($li.dataset.readCount = 1); // → HTML에 data-read-count="1" 로 반영
```

> - `data-이름`(케밥 케이스) ↔ `dataset.이름`(카멜 케이스)로 자동 변환됩니다. (`data-category-name` ↔ `dataset.categoryName`)
> - `dataset`으로 읽고 쓰는 값은 항상 **문자열**입니다. 숫자로 써야 한다면 `Number(li.dataset.boardId)`처럼 변환이 필요합니다.
> - data 어트리뷰트는 브라우저 개발자 도구(Elements 탭)에서 누구나 볼 수 있으므로, 민감한 정보나 보안이 필요한 데이터는 저장하면 안 됩니다.
> - 이벤트를 각 `<li>`마다 개별로 등록하지 않고 부모(`boardList`)에 한 번만 등록한 뒤 `e.target.closest('li')`로 클릭된 요소를 찾는 방식을 **이벤트 위임**이라고 하며, 목록처럼 항목이 많거나 동적으로 추가되는 경우에 효율적입니다.

---

## 6. CSS 조작하기

### 6-1. 인라인 스타일 직접 제어 — `element.style`

```js
$box1.style.width = '200px';
$box1.style.height = '100px';
$box1.style.backgroundColor = '#ffe4e6'; // CSS의 background-color → 카멜 케이스로 접근
```

- 요소에 `style="..."` 어트리뷰트를 직접 추가/변경하는 것과 같습니다.
- CSS 속성 이름의 하이픈(`-`)은 카멜 케이스로 바꿔서 접근합니다. (`background-color` → `backgroundColor`)
- 특정 값 하나만 급하게 바꿀 때는 편리하지만, 스타일이 많아지면 CSS와 JS 양쪽에 스타일 정보가 흩어져 유지보수가 어려워집니다. → 상태별로 여러 스타일을 적용할 때는 클래스 제어를 권장합니다.

### 6-2. 클래스 목록 제어 — `element.classList`

```js
$box2.className;   // 'box' → 전체 클래스를 '문자열'로 확인 (직접 대입 시 기존 클래스 전부 교체됨, 주의)
$box2.classList;    // DOMTokenList → 클래스를 개별적으로 다루기 위한 객체
```

| 메서드                     | 동작                                                                |
|---------------------------|--------------------------------------------------------------------|
| `classList.add('a', 'b')` | 클래스 추가 (여러 개 동시 가능)                                        |
| `classList.remove('a')`   | 클래스 제거                                                          |
| `classList.toggle('a')`   | 있으면 제거, 없으면 추가 → 결과적으로 **추가되었는지 여부(boolean)를 반환** |
| `classList.contains('a')` | 해당 클래스가 있는지 확인 (true/false)                                 |

```js
$changeBtn.onclick = () => {
    $box2.classList.add('circle', 'red-bg', 'big-text');
    $box2.classList.remove('circle');
};

$toggleBtn.onclick = () => {
    const isAdded = $box2.classList.toggle('red-bg');
    $toggleBtn.textContent = isAdded ? '배경색 제거' : '배경색 적용';
};
```

> `classList.toggle('클래스', 조건)`처럼 두 번째 인자로 boolean을 넘기면, 조건이 `true`일 때만 강제로 추가하고 `false`일 때만 강제로 제거하도록 지정할 수도 있습니다. (`toggle`의 자동 판단 없이 원하는 상태를 명시적으로 고정하고 싶을 때 유용)

### 6-3. `className` vs `classList` 요약

- `className = '문자열'`로 대입하면 **기존 클래스가 전부 사라지고 새 문자열로 완전히 교체**됩니다. (`$box2.className = 'circle'`을 실행하면 `.box`에 있던 테두리/크기 스타일까지 모두 사라짐)
- 클래스를 하나씩 추가·제거·토글해야 한다면 항상 `classList`를 사용하는 것이 안전합니다.

---

## 전체 흐름 요약

| 주제 | 핵심 API | 기억할 포인트 |
|----------|-------------------------------------------------------------------|--------------------------------------------------------------------------------|
| 선택      | `querySelector(All)`, `getElementById`, `getElementsByClassName`  | NodeList(정적) vs HTMLCollection(라이브)                                        |
| 탐색      | `children`, `parentElement`, `nextElementSibling`                 | `Element`가 붙은 프로퍼티로 텍스트 노드 혼란 방지                                   |
| 텍스트    | `textContent`, `nodeValue`, `innerText`                            | 실무 기본은 `textContent`                                                      |
| 구조 변경 | `innerHTML`, `createElement`+`appendChild`, `insertAdjacentHTML`   | 사용자 입력에는 XSS 주의, `innerHTML` 대신 `textContent`/`createElement` 우선 고려 |
| 속성      | `getAttribute/setAttribute`, `.value`, `.dataset`                 | 어트리뷰트(초기값) vs 프로퍼티(현재값) 구분                                         |
| 스타일    | `.style`, `.classList`                                            | 상태 기반 스타일링은 클래스 제어로                                                  |