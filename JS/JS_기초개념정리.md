# JavaScript 기초 개념 정리

코드를 짜면서 구조를 이해하는 데 필요한 핵심 개념만 순서대로 정리했다. 각 항목은 짧은 코드 예제와 함께 본다.

---

## 1. 변수와 상수

```js
var a = 1;    // 함수 스코프, 재선언 가능 (요즘은 거의 안 씀)
let b = 2;    // 블록 스코프, 재할당 가능
const c = 3;  // 블록 스코프, 재할당 불가 (재선언도 불가)

b = 20;       // OK
// c = 30;    // 에러: Assignment to constant variable
```

- 기본은 `const`, 값이 바뀔 예정이면 `let`. `var`는 쓰지 않는다.
- `const`는 "재할당 불가"이지 "불변"이 아니다. 객체/배열 내부는 바꿀 수 있다.

```js
const arr = [1, 2, 3];
arr.push(4);   // OK, arr 자체를 재할당하는 게 아니라 내부만 바꿈
console.log(arr); // [1, 2, 3, 4]
```

---

## 2. 자료형

### 원시 타입 (Primitive)
값 자체가 복사되는 타입. 변경 불가능(immutable).

```js
let num = 42;              // number
let str = "hello";         // string
let bool = true;           // boolean
let empty = null;          // null (의도적으로 "없음")
let notDefined;            // undefined (아직 값이 없음)
let big = 10n;             // bigint
let sym = Symbol("id");    // symbol
```

### 참조 타입 (Reference)
객체(object), 배열(array), 함수(function) — 참조(주소)가 복사되는 타입.

```js
let objA = { x: 1 };
let objB = objA;      // 같은 객체를 참조
objB.x = 99;
console.log(objA.x);  // 99 (같은 걸 가리키므로 같이 바뀜)
```

### 타입 확인 & 형 변환

```js
typeof 42;          // "number"
typeof "hi";         // "string"
typeof undefined;   // "undefined"
typeof null;        // "object" (JS의 유명한 버그성 동작)
Array.isArray([]);  // true

Number("123");   // 123
String(123);     // "123"
Boolean(0);      // false (0, "", null, undefined, NaN → false / 나머지는 true)
```

---

## 3. 연산자

```js
5 + 3;   // 8
5 - 3;   // 2
5 * 3;   // 15
5 / 3;   // 1.666...
5 % 3;   // 2 (나머지)
5 ** 2;  // 25 (거듭제곱)

1 == "1";   // true  (값만 비교, 타입 변환 O)
1 === "1";  // false (값 + 타입 비교, 변환 X) → 실무에서는 항상 === 사용

let x = null;
x ?? "기본값";   // null/undefined일 때만 기본값 → "기본값"
x || "기본값";   // falsy 전체(0, "" 포함)일 때 기본값

let y = { name: "kim" };
y?.name;      // 옵셔널 체이닝: y가 null/undefined면 에러 없이 undefined 반환
y?.age?.toFixed?.(); // 중첩된 접근도 안전하게
```

---

## 4. 조건문 & 반복문

```js
// if / else if / else
let score = 85;
if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else {
  console.log("C");
}

// switch
switch (score) {
  case 100:
    console.log("만점");
    break;
  default:
    console.log("만점 아님");
}

// for
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// for...of : 배열, 문자열 등 순회 가능한 값(iterable)
for (const v of [10, 20, 30]) {
  console.log(v);
}

// for...in : 객체의 key 순회
for (const key in { a: 1, b: 2 }) {
  console.log(key); // "a", "b"
}

// while
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}
```

---

## 5. 함수

```js
// 함수 선언식 - 호이스팅됨 (선언 전에 호출 가능)
function add(a, b) {
  return a + b;
}

// 함수 표현식 - 호이스팅 안 됨
const sub = function (a, b) {
  return a - b;
};

// 화살표 함수 - 짧은 문법, this를 자기 것으로 안 가짐(중요)
const mul = (a, b) => a * b;

// 기본값 & 나머지 매개변수
function greet(name = "손님", ...others) {
  console.log(`안녕하세요 ${name}`, others);
}
greet();               // 안녕하세요 손님 []
greet("kim", "a", "b"); // 안녕하세요 kim ['a', 'b']
```

### 일반 함수 vs 화살표 함수의 `this` 차이 (자주 헷갈리는 부분)

```js
const obj = {
  name: "kim",
  normal: function () {
    console.log(this.name); // obj를 가리킴 → "kim"
  },
  arrow: () => {
    console.log(this.name); // 자기 것 없음, 바깥(전역) this를 그대로 씀 → undefined
  },
};
obj.normal(); // "kim"
obj.arrow();  // undefined
```

---

## 6. 배열 다루기 (자주 쓰는 메서드)

```js
const nums = [1, 2, 3, 4, 5];

nums.map(n => n * 2);          // [2,4,6,8,10] - 각 요소를 변환한 새 배열
nums.filter(n => n % 2 === 0); // [2,4]        - 조건에 맞는 것만
nums.reduce((acc, n) => acc + n, 0); // 15      - 누적 계산
nums.find(n => n > 3);         // 4             - 조건에 맞는 첫 요소
nums.includes(3);              // true
nums.forEach(n => console.log(n)); // 그냥 순회(반환값 없음)

nums.push(6);     // 끝에 추가
nums.pop();       // 끝에서 제거
nums.shift();     // 앞에서 제거
nums.unshift(0);  // 앞에 추가
nums.slice(1, 3); // 원본 안 건드리고 일부만 잘라 반환
```

`map/filter/reduce`는 **원본을 바꾸지 않고 새 배열을 반환**한다. `push/pop/splice`는 원본 자체를 바꾼다. 이 차이가 구조 이해에서 중요하다.

---

## 7. 객체 다루기

```js
const user = {
  name: "kim",
  age: 20,
  greet() {
    console.log(`안녕, 나는 ${this.name}`);
  },
};

user.name;      // "kim"
user["name"];   // 동일 (변수로 key 접근할 때 유용)
user.greet();   // "안녕, 나는 kim"

// 구조 분해 할당
const { name, age } = user;
console.log(name, age); // "kim" 20

// 전개 연산자로 복사/합치기
const user2 = { ...user, age: 21 }; // user는 그대로, age만 바뀐 새 객체

Object.keys(user);    // ["name", "age", "greet"]
Object.values(user);  // ["kim", 20, f]
Object.entries(user); // [["name","kim"], ["age",20], ...]
```

---

## 8. 스코프와 클로저 (구조 이해에서 제일 중요한 개념)

**스코프**: 변수가 어디까지 유효한지의 범위.

```js
function outer() {
  let x = 10;
  function inner() {
    console.log(x); // 바깥 함수의 변수에 접근 가능
  }
  inner();
}
```

**클로저**: 함수가 자신이 선언될 때의 스코프(환경)를 기억하는 것. 함수가 실행이 끝나도 그 스코프의 변수를 계속 붙잡고 있을 수 있다.

```js
function makeCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

const counter = makeCounter();
counter(); // 1
counter(); // 2
counter(); // 3
// count는 makeCounter 밖에서는 직접 접근 불가 → 캡슐화된 상태 관리
```

이 패턴은 상태를 감추고 싶을 때(예: 카운터, 캐시, 모듈 패턴) 자주 쓴다.

---

## 9. 비동기 처리

### 콜백 → 프로미스 → async/await 순으로 발전했다

```js
// 콜백 방식 (오래된 방식, 중첩되면 "콜백 지옥")
setTimeout(() => {
  console.log("1초 후 실행");
}, 1000);

// 프로미스
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("데이터"), 1000);
  });
}
fetchData().then(data => console.log(data)).catch(err => console.error(err));

// async/await - 프로미스를 동기 코드처럼 읽히게 함 (실무에서 가장 많이 씀)
async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
getData();
```

- `async` 함수는 항상 프로미스를 반환한다.
- `await`는 `async` 함수 안에서만 쓸 수 있다.
- 여러 개를 동시에 기다리려면 `Promise.all([...])`.

---

## 10. 프로토타입과 클래스 (객체지향 구조)

```js
// class 문법 (내부적으로는 프로토타입 기반)
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name}가 소리를 냅니다.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name}: 멍멍!`);
  }
}

const d = new Dog("초코");
d.speak(); // "초코: 멍멍!"
```

- `class`는 문법 설탕(syntactic sugar)이고, 실제로는 프로토타입 체인으로 동작한다.
- `extends`로 상속, `super()`로 부모 생성자 호출.

---

## 11. 모듈 (파일 단위로 코드 나누기)

```js
// math.js
export const add = (a, b) => a + b;
export default function multiply(a, b) {
  return a * b;
}

// main.js
import multiply, { add } from "./math.js";
console.log(add(1, 2));      // 3
console.log(multiply(2, 3)); // 6
```

- `export`(named): 여러 개 내보낼 때 `{ }`로 가져옴.
- `export default`: 파일당 하나, 가져올 때 이름 자유.

---