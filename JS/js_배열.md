# 오늘의 JavaScript 학습 정리

## 1. 배열(Array) 기초

### 핵심 개념
- 배열은 관련된 값들을 하나의 목록으로 묶어서 관리하는 자료구조
- **인덱스(index)**: 배열에서 요소의 위치를 나타내는 번호로, **0부터 시작**
- `배열이름[인덱스]`로 요소에 접근
- 존재하지 않는 인덱스에 접근하면 `undefined` 반환
- `배열.length`로 배열의 길이(요소 개수) 확인
- 인덱스로 직접 접근해 요소를 교체(재할당) 가능
- `for`문으로 배열 순회 가능
- `typeof 배열` → `"object"` (배열도 객체의 일종)
- `Array.isArray(배열)`로 진짜 배열인지 확인 (typeof로는 구분 안 되기 때문)

```js
const fruits = ['바나나', '복숭아', '키위'];
console.log(fruits[1]);       // 복숭아
console.log(fruits[3]);       // undefined
fruits[1] = '딸기';            // 교체
console.log(Array.isArray(fruits)); // true
```

### 
- **음수 인덱스는 지원하지 않음**: `fruits[-1]`은 마지막 요소가 아니라 `undefined` (Python과 다름! 마지막 요소는 `fruits[fruits.length - 1]` 또는 `fruits.at(-1)` 사용)
- `const`로 선언해도 배열 **내부 요소는 변경 가능**(재할당만 불가). 배열 자체를 통째로 바꾸는 것(`fruits = [...]`)만 금지됨

---

## 2. 배열 메소드 (추가/삭제/검색)

### 배운 내용
| 메소드          |  기능                                | 반환값           | 원본 변경 |
-------------------------------------------------------------------------------------
| `push(값)`     | 배열 끝에 요소 추가                    | 변경된 배열의 길이 |    O     |
| `pop()`        | 배열 끝 요소 제거                     | 제거된 요소       |     O    |
| `unshift(값)`  | 배열 맨 앞에 요소 추가                 | 변경된 배열의 길이 |     O    |
| `shift()`      | 배열 맨 앞 요소 제거                   | 제거된 요소      |      O   |
| `indexOf(값)`  | 값이 처음 나오는 인덱스 반환 (없으면 -1) | 인덱스(숫자)      |     X    |
| `includes(값)` | 값이 배열에 있는지 여부                 | boolean         |     X    |

### 
- **`push`/`unshift`는 여러 값을 한 번에 추가 가능**: `food.push('a', 'b', 'c')`
- **원본을 바꾸지 않는 추가/삭제 방법도 있음** (불변성 유지가 중요할 때 유용):
  - `[...food, '새메뉴']` → 새 배열에 추가
  - `food.slice(1)` → 첫 요소를 제외한 새 배열
  - `food.concat('새메뉴')` → 새 배열로 합치기
- `splice(시작인덱스, 삭제개수, 추가할값...)` : 배열 중간을 자르거나 끼워 넣을 때 사용 (원본 변경, 오늘 다룬 4개 메서드 외에 실무에서 자주 씀)
- `lastIndexOf(값)` : 뒤에서부터 검색해 값의 인덱스 반환

---

## 3. forEach / map

### 핵심 개념
- **`forEach`**: 배열의 각 요소를 순회하며 콜백 실행. **반환값은 항상 `undefined`** (값을 모아주지 않음, 단순 반복 작업용)
- **`map`**: 각 요소를 콜백의 반환값으로 바꾼 **새로운 배열**을 만들어 반환. **원본 배열은 변경하지 않음**
- 콜백함수는 `(요소, 인덱스)` 형태로 현재 요소와 인덱스를 전달받을 수 있음

```js
students.forEach((student, index) => {
    console.log((index + 1) + '번째 이름: ', student.name);
});

const studentNames = students.map(student => student.name);
```

### 
- **forEach vs map 선택 기준**: "새로운 배열이 필요한가?" → 필요하면 `map`, 단순 출력/로깅/외부 변수 조작이면 `forEach`
- `map`, `forEach` 콜백은 사실 `(element, index, array)` **3개의 인자**를 받을 수 있음 (세 번째는 원본 배열 자체)
- `map`을 쓰고 결과를 안 쓰는 것은 안티패턴 → 그럴 땐 `forEach`를 써야 함
- 배열 안의 **객체**를 map으로 다룰 때, 얕은 복사만 되므로 `student.score += 5`처럼 원본 객체의 속성을 직접 수정하면 원본에 영향이 감 (오늘 예제처럼 `return student.score + 5`로 새 값을 만드는 게 안전)

---

## 4. filter / find / some / every

### 핵심 개념
| 메소드 | 기능 | 반환값 |
|----------|---------------------------------------------|-------------------------|
| `filter` | 조건을 통과하는 **모든** 요소를 모아 새 배열 반환 | 배열 (없으면 빈 배열 `[]`) |
| `find`   | 조건을 통과하는 **첫 번째** 요소 하나를 반환     | 요소 또는 `undefined`     |
| `some`   | 조건을 만족하는 요소가 **하나라도** 있는지       | boolean                  |
| `every`  | **모든** 요소가 조건을 만족하는지               | boolean                  |

```js
const highScores = students.filter(student => student.score >= 85);
const firstHighScorer = students.find(student => student.score >= 85);
```

- `find`로 못 찾으면 `undefined` 반환 → **옵셔널 체이닝(`?.`)** 으로 안전하게 접근 가능: `firstHighScorer?.name`

### 
- `findIndex(조건)`: `find`와 비슷하지만 요소가 아니라 **인덱스**를 반환 (못 찾으면 -1)
- **빈 배열에서 `every`는 항상 `true`**를 반환함 (수학적 진공 참, vacuous truth) — 헷갈리기 쉬운 포인트
- `filter`와 `find`의 콜백은 반드시 **boolean처럼 평가되는 값**을 반환해야 함(내부적으로 true/false로 취급)

---

## 5. sort / reduce

### 핵심 개념
- **`sort()`**: 원본 배열을 **직접 정렬**(원본 변경). 인자가 없으면 **문자열 기준**으로 정렬되기 때문에 숫자 정렬 시 반드시 비교함수 필요
  - `(a, b) => a - b` : 오름차순
  - `(a, b) => b - a` : 내림차순
  - 반환값이 음수면 a를 앞에, 양수면 b를 앞에, 0이면 순서 유지
- **`reduce(콜백, 초기값)`**: 배열을 순회하며 누적값을 계산, 최종적으로 하나의 값으로 축약(reduce)
  - 콜백은 `(누적값, 현재값)`을 받고, 매 호출의 반환값이 다음 누적값이 됨
  - 빈 배열도 **초기값을 주면** 안전하게 동작 (`[].reduce((s,c)=>s+c, 0)` → `0`)

```js
numbers.sort((a, b) => a - b);
const total = amounts.reduce((sum, current) => sum + current, 0);
```

### 
- **`sort()`가 문자열 정렬을 기본으로 하는 이유**: 숫자를 문자로 변환 후 유니코드 순서로 비교하기 때문에 `[10, 1, 9]`처럼 정렬하면 `[1, 10, 9]`처럼 이상하게 나옴 → 항상 비교함수 명시 습관 들이기
- **초기값 없이 `reduce`를 호출하면** 배열의 첫 번째 요소가 초기값이 되고, 빈 배열이면 **에러 발생** → 항상 초기값을 명시하는 습관 추천
- `reduce`는 합계 계산 외에도 최댓값 찾기, 객체로 그룹핑하기, 배열을 평탄화하기 등 다양하게 활용 가능 (map+filter를 reduce 하나로 대체 가능)

---

## 6. String(문자열) 메소드

### 핵심 개념
- 문자열은 **불변(immutable)** 값 → 메서드 실행 후 원본은 그대로, 새 문자열을 반환
- `trim()` : 앞뒤 공백 제거
- `toLowerCase()` / `toUpperCase()` : 대소문자 변환
- `indexOf(검색어)` : 검색어 시작 인덱스 반환 (없으면 -1)
- `includes(검색어)` : 포함 여부 (대소문자 구분함)
- `slice(시작, 끝)` : 시작 인덱스부터 끝 인덱스 **직전까지** 잘라 반환
- `lastIndexOf(문자)` : 마지막으로 나오는 위치 찾기 (확장자 추출 등에 활용)
- `split(구분자)` : 문자열을 배열로 분리
- **메서드 체이닝**: `split()` 후 바로 `filter()`를 연결해서 빈 문자열 제거하는 패턴

```js
const tagList = tags.split('#').filter(tag => tag !== '');
```

### 
- `replace(찾을값, 바꿀값)` / `replaceAll(찾을값, 바꿀값)` : 문자열 치환 (replace는 첫 번째만, replaceAll은 전체)
- `startsWith(문자열)` / `endsWith(문자열)` : 특정 문자로 시작/끝나는지 확인 (오늘 배운 indexOf/includes와 함께 검색 로직에 자주 쓰임)
- `padStart(길이, 채울문자)` / `padEnd(길이, 채울문자)` : 자릿수 맞추기 (예: 시간 `09:05`처럼 0 채우기)
- 템플릿 리터럴 ( `` `문자열 ${변수}` `` )도 문자열을 다룰 때 자주 함께 쓰이는 문법이므로 알아두면 좋음
- `slice`와 `substring`은 비슷하지만 음수 인덱스 처리 방식이 다름 (`slice(-4)`는 뒤에서 4글자 → 확장자 추출 등에 유용)

---

## 7. Math 객체

### 핵심 개념
- `Math`는 **new로 생성하지 않고** `Math.메서드()`로 바로 사용하는 표준 빌트인 객체
- `Math.round()` : 반올림
- `Math.floor()` : 내림
- `Math.ceil()` : 올림
- `Math.random()` : 0 이상 1 미만의 임의의 실수 반환
- **랜덤 정수 만들기 공식**: `Math.floor(Math.random() * n)` → 0~n-1 사이의 정수
- **배열에서 무작위 요소 뽑기**: `배열[Math.floor(Math.random() * 배열.length)]`

```js
const menuIndex = Math.floor(Math.random() * menus.length);
console.log(menus[menuIndex]);
```

### 
- `Math.max(...숫자들)` / `Math.min(...숫자들)` : 최댓값/최솟값 (배열에 적용하려면 스프레드 문법과 함께: `Math.max(...numbers)`)
- `Math.abs(숫자)` : 절댓값
- `Math.pow(밑, 지수)` 또는 `밑 ** 지수` : 거듭제곱
- **특정 범위(min~max)의 랜덤 정수 공식**: `Math.floor(Math.random() * (max - min + 1)) + min`
- `Math.trunc()` : 소수점을 그냥 버림 (floor와 달리 음수에서 다르게 동작: `Math.trunc(-3.6)` → `-3`, `Math.floor(-3.6)` → `-4`)

---

## 8. 화살표 함수 (Arrow Function)

### 핵심 개념
- 기본형: `const 함수이름 = (매개변수) => { 실행구문 }`
- 매개변수가 **1개**면 소괄호 생략 가능: `x => x * x`
- 매개변수가 **없으면** 소괄호 생략 불가: `() => '안녕하세요.'`
- 실행구문이 **1줄**이면 중괄호 생략 가능(암묵적 반환), **2줄 이상**이면 중괄호+`return` 필수
- 중괄호를 쓰면서 `return`을 빠뜨리면 `undefined` 반환됨 (실수하기 쉬운 부분!)
- **객체를 바로 반환**할 때는 `({ ... })`처럼 **소괄호로 감싸야** 함 (중괄호가 객체인지 함수 본문인지 구분하기 위해)
- 화살표 함수도 **콜백**으로 다른 함수에 전달 가능

```js
const wrongSquare = x => { x * x; }; // return 없음 → undefined 주의!
const createUser = (id, name) => ({ id: id, name: name }); // 객체 반환 시 소괄호 필수
```

### 
- **화살표 함수는 자신만의 `this`를 갖지 않음** — 선언된 위치의 상위 스코프 `this`를 그대로 사용(이 부분이 일반 `function`과 가장 큰 차이점이며, 객체 메서드로는 잘 안 쓰는 이유)
- 화살표 함수는 **생성자로 사용 불가** (`new arrowFunc()` → 에러)
- 화살표 함수에는 **`arguments` 객체가 없음** (필요하면 나머지 매개변수 `...args` 사용)
- 오늘 파일에 `'use strict';`가 있었는데, 이는 자바스크립트를 더 엄격한 규칙으로 실행하게 하는 선언(예: 선언 안 된 변수에 값 할당 시 에러 발생 등) — 모듈이나 최신 문법에서는 기본으로 strict mode가 적용되는 경우가 많음

---

##  요약

| 주제 | 핵심 키워드 |
|------------------------|---------------------------------------------|
| 배열 기초               | 인덱스, length, Array.isArray                |
| 배열 메소드              | push/pop, unshift/shift, indexOf, includes |
| forEach/map            | 순회 vs 새 배열 생성                          |
| filter/find/some/every | 조건 필터링, 검색, 진위 판별                   |
| sort/reduce            | 정렬, 누적 계산                               |
| String                 | trim, 대소문자, slice, split, 체이닝          |
| Math                   | 반올림/내림/올림, random                      |
| 화살표 함수              | 축약 문법, this 바인딩 없음                   |
