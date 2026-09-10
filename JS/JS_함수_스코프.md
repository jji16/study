# JavaScript 함수(Function)와 스코프(Scope) 정리

## 1. 함수의 기본 구조

함수는 특정 동작을 이름 붙여 재사용할 수 있게 만든 코드 묶음이다.

```javascript
function calculateArea(width, height) {    // 함수 이름과 매개변수(parameter)
    console.log('함수 안으로 들어왔습니다.');   // 실행할 로직
    const area = width * height;
    return area;                            // 반환값 - 호출한 곳으로 값 전달
}

const result = calculateArea(10, 20); // 인자(argument) 전달하며 함수 호출
console.log(result); // 200
```

- 함수를 정의하지 않고 매번 계산하면 코드가 중복된다.
  ```javascript
  const area1 = 10 * 20;
  const area2 = 30 * 40;
  const area3 = 50 * 60;
  ```
  → 함수로 묶으면 재사용이 가능해진다.

---

## 2. 매개변수(Parameter)와 인수(Argument)

- **매개변수(parameter)**: 함수를 정의할 때 값을 받기 위해 설정하는 통로. 함수 내부에서만 사용 가능한 지역 변수.
- **인수(argument)**: 함수를 실제로 호출할 때 넘기는 값.

```javascript
function greet(name) {
    console.log(name);
    return `${name}님 안녕하세요.`;
}

console.log(greet('홍길동'));        // 정상 호출
console.log(greet());                // 인수 부족 → name은 undefined
console.log(greet('홍길동', '이순신')); // 초과 인수는 무시됨
```

### 매개변수 기본값

인수가 전달되지 않거나 `undefined`가 들어오면 기본값이 사용된다.

```javascript
function hi(name = '아무개') {
    return `${name} 안녕?`;
}

console.log(hi());          // 아무개 안녕?
console.log(hi('유관순'));   // 유관순 안녕?
console.log(hi(undefined)); // 아무개 안녕? (기본값 적용)
```

---

## 3. return (반환)

`return`은 함수 실행 결과를 호출한 곳으로 돌려준다.

```javascript
function returnAdd(a, b) {
    return a + b;
}
const returned = returnAdd(10, 20); // 30
```

- `return` 없이 `console.log`만 하는 함수는 값을 반환하지 않고 `undefined`가 담긴다.

```javascript
function printAdd(a, b) {
    console.log(a + b); // 화면에 출력만 함
}
const printed = printAdd(10, 20); // undefined
```

### return 이후 코드는 실행되지 않음

```javascript
function sayHello(name) {
    return `${name}님, 안녕하세요.`;
    console.log('출력이 되나요?'); // 실행되지 않음
}
```

### 값 없는 return / return 자체 생략

두 경우 모두 결과는 `undefined`이다.

```javascript
function noReturn() {
    console.log('함수 호출됨.');
    return; // 값 명시 안 함
}

function emptyFunction() {
    // return문 자체가 없음
}
```

### 조기 종료 (Early Return)

조건이 맞지 않으면 즉시 함수를 종료시켜 이후 로직 실행을 막는 패턴.

```javascript
function registerUser(nickname) {
    if (nickname.length < 2) {
        console.log('닉네임이 너무 짧습니다.');
        return; // 여기서 함수 종료
    }
    console.log(`${nickname}님, 환영합니다.`);
}
```

---

## 4. 함수 표현식과 호이스팅

### 함수 선언문 (Function Declaration)

```javascript
function hello(name) {
    return `${name} 안녕?`;
}
```
- 코드 실행 전에 먼저 준비되므로, 선언 위치보다 **위에서도 호출 가능**하다.
- 이런 동작을 **호이스팅(hoisting)** 이라고 한다.

### 함수 표현식 (Function Expression)

```javascript
const hi = function (name) { // 함수명 생략 가능
    return `${name}님, 안녕하세요.`;
};
```
- 함수를 값으로 만들어 변수에 담는 방식.
- 변수에 할당되기 **전에는 호출할 수 없다** (호이스팅되지 않음 → `ReferenceError`).

### 객체 프로퍼티로 함수 담기

```javascript
const myObject = {
    sayHi: function () {
        console.log('반갑습니다.');
    }
};
myObject.sayHi(); // 반갑습니다.
```

### 함수를 인수로 전달하고, 함수를 반환하기

```javascript
function manager(task, count) {
    console.log('매니저가 업무를 지시합니다.');
    for (let i = 0; i < count; i++) {
        task();
    }
    return function () {
        console.log('모든 업무가 완료되었습니다.');
    };
}

const report = manager(sayHello, 3); // sayHello() 처럼 ()를 붙이면 안 됨 (TypeError 위험)
report();
```

---

## 5. 콜백 함수와 고차 함수

- **콜백 함수**: 지금 바로 실행하는 게 아니라, 다른 함수에게 맡겨두었다가 필요한 시점에 호출되도록 전달하는 함수.
- **고차 함수(Higher-order function)**: 함수를 인수로 받거나 함수를 반환하는 함수.

```javascript
function calcurator(CalculateCallback, a, b) {
    console.log('계산을 시작합니다.');
    // 계산 '시점'은 calcurator가 결정하지만
    // 계산 '방식'은 외부에서 주입받은 콜백 함수가 결정한다.
    const result = CalculateCallback(a, b);
    return result;
}

function add(a, b) { return a + b; }
function multiply(a, b) { return a * b; }

const addResult = calcurator(add, 9, 8);           // 17
const multiplyResult = calcurator(multiply, 9, 8);  // 72
```

### 실용 예제: 배열 정렬

`sort()`라는 고차 함수에 '정렬 기준' 콜백 함수를 전달한다.

```javascript
const numbers = [3, 10, 1, 6, 9];

numbers.sort(function (a, b) {
    return a - b; // 음수면 a가 앞으로, 양수면 b가 앞으로 → 오름차순
});

console.log(numbers); // [1, 3, 6, 9, 10]
```

---

## 6. 스코프 (Scope)

스코프란 변수를 사용할 수 있는 범위를 말한다.

### 함수 스코프

```javascript
function calculateArea(width, height) {
    const area = width * height;
    return area;
}
// console.log(area);  // Error: 함수 밖에서 접근 불가
// console.log(width); // Error
```

### 블록 스코프

```javascript
const outerValue = '바깥쪽 값';
if (true) {
    const blockValue = '블록 안의 값';
    console.log(outerValue); // 안쪽에서는 바깥 값 접근 가능
    console.log(blockValue);
}
// console.log(blockValue); // Error: 블록 밖에서 접근 불가
```

### 같은 이름의 변수 (섀도잉)

```javascript
const message = '바깥';

function showMessage() {
    const message = '안쪽';
    console.log(message); // 안쪽
}
showMessage();
console.log(message); // 바깥 (서로 영향 없음)
```

### 중첩 스코프

안쪽 스코프는 바깥쪽 값에 접근 가능하지만, 반대는 불가능하다.

```javascript
const outValue = '바깥쪽 변수';

if (true) {
    const blockValue = '블록 변수';
    const sayHi = function () {
        const localValue = '함수 지역 변수';
        console.log(outValue);   // 접근 가능
        console.log(blockValue); // 접근 가능
        console.log(localValue);
    };
    sayHi();
    // console.log(localValue); // Error
}
// console.log(blockValue); // Error
```

### 함수는 "정의된 위치"의 스코프를 기억한다 (렉시컬 스코프)

```javascript
const label = '바깥';

function printLabel() {
    console.log(label); // 정의된 위치의 바깥 이름을 찾음 → '바깥'
}

function run() {
    const label = 'run 안쪽';
    printLabel(); // 여전히 '바깥' 출력 (호출 위치가 아니라 정의 위치 기준)
}
run();
```

---

## 7. var vs let

### var의 함수 스코프

`var`는 블록을 무시하고 함수 전체 범위를 가진다. `for`문 안의 `var i`가 바깥의 `var i`를 덮어쓴다.

```javascript
function comparaVar() {
    var i = 100;
    for (var i = 0; i < 3; i++) {
        console.log('var 안: ', i); // 0, 1, 2
    }
    console.log('var 밖: ', i); // 3
}
```

### let의 블록 스코프

`let`은 블록 범위를 가지므로, `for`문 안의 `i`와 바깥의 `i`는 서로 다른 변수다.

```javascript
function comparaLet() {
    let i = 100;
    for (let i = 0; i < 3; i++) {
        console.log('let 안: ', i); // 0, 1, 2
    }
    console.log('let 밖: ', i); // 100
}
```

### 재선언 / 재할당 차이

| 키워드 | 재선언 | 재할당 |
|--------|--------|--------|
| `var`  | 허용   | 허용   |
| `let`  | 불가   | 허용   |
| `const`| 불가   | 불가   |

```javascript
var oldMessage = '처음';
var oldMessage = '변경'; // 재선언 허용

let message = '처음';
message = '변경'; // 재할당은 허용, 재선언(let message = ...)은 불가

const greeting = '안녕하세요.';
// greeting = '안녕히 가세요.';      // Error: 재할당 불가
// const greeting = '안녕히 가세요.'; // Error: 재선언 불가
```

### 초기화 시점 차이 (호이스팅 & TDZ)

```javascript
function compareInitialization() {
    console.log('var 선언 전', oldValue); // undefined (호이스팅되어 미리 선언됨)
    var oldValue = '준비됨';
    console.log('var 대입 후', oldValue); // 준비됨

    // console.log(value); // Error: TDZ(Temporal Dead Zone)로 인해 접근 불가
    let value;
    console.log('let 선언 후', value); // undefined
    value = '준비됨';
    console.log('let 대입 후', value); // 준비됨
}
```

- `var`는 선언 전에 접근해도 `undefined`로 처리된다 (호이스팅).
- `let`/`const`는 선언 전 접근 시 **TDZ(일시적 사각지대)** 로 인해 `ReferenceError`가 발생한다.

---

## 8. const와 객체

`const`는 재할당이 불가능하지만, 객체의 **프로퍼티 값은 변경 가능**하다.

```javascript
const student = {
    name: '판다',
    age: 5
};

student.name = '코알라'; // 허용: 객체 자체가 아니라 프로퍼티만 변경
console.log(student.name); // 코알라

// student = { name: '고릴라', age: 10 }; // Error: 객체 자체의 재할당은 불가
```

---

## 핵심 요약

- **함수**: 매개변수/인수로 값을 주고받고, `return`으로 결과를 돌려준다.
- **Early Return**: 조건이 안 맞으면 즉시 종료해 코드 흐름을 단순화한다.
- **함수 선언문**은 호이스팅되지만, **함수 표현식**은 되지 않는다.
- **콜백 함수 / 고차 함수**: 동작 방식을 외부에서 주입받는 패턴.
- **스코프**: 함수 스코프(`var`) vs 블록 스코프(`let`, `const`), 렉시컬 스코프(정의 위치 기준).
- **var**는 재선언 가능·호이스팅 시 `undefined`, **let/const**는 재선언 불가·TDZ 존재.
- **const**는 재할당만 금지될 뿐, 객체 내부 프로퍼티는 자유롭게 수정 가능하다.