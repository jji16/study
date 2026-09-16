# JavaScript 핵심 문법 정리

> 클래스, 상속, 화살표 함수, 스프레드/나머지 문법, 구조 분해 할당

---

## 목차
1. [클래스 기본 문법](#1-클래스-기본-문법)
2. [클래스 상속](#2-클래스-상속-class-inheritance)
3. [정적 메서드와 정적 프로퍼티](#3-정적-메서드와-정적-프로퍼티-static)
4. [화살표 함수](#4-화살표-함수-arrow-function)
5. [스프레드 / 나머지 문법](#5-스프레드--나머지-문법-spread--rest)
6. [구조 분해 할당](#6-구조-분해-할당-destructuring-assignment)

---

## 1. 클래스 기본 문법

### 1-1. 클래스 선언 (Class Declaration)

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`안녕하세요, 저는 ${this.name}입니다.`);
  }
}

const person1 = new Person('철수', 20); // new 키워드 필수
person1.greet(); // 안녕하세요, 저는 철수입니다.
```

- `class` 키워드로 선언, `new`로 인스턴스 생성
- `constructor`는 인스턴스 생성 시 자동 실행되는 초기화 메서드 (클래스당 1개)
- 함수 선언과 달리 **호이스팅되지 않음** (선언 전 사용 불가)

### 1-2. 클래스 표현식 (Class Expression)

```javascript
// 익명 클래스 표현식
const Person = class {
  constructor(name) {
    this.name = name;
  }
};

// 기명 클래스 표현식
const Animal = class Animal2 {
  constructor(name) {
    this.name = name;
  }
};

const p = new Person('영희');
```

- 변수에 클래스를 **값처럼 할당**하는 방식
- 함수 표현식과 동일한 개념 (조건부 생성, 즉시 사용 등에 유용)

### 1-3. Getter / Setter

속성처럼 접근하지만 실제로는 메서드가 실행되는 문법입니다.

```javascript
class Circle {
  constructor(radius) {
    this._radius = radius;
  }

  // getter: 값을 읽을 때 (괄호 없이 접근)
  get area() {
    return Math.PI * this._radius ** 2;
  }

  // setter: 값을 쓸 때 (괄호 없이 대입)
  set radius(value) {
    if (value < 0) {
      console.log('반지름은 음수일 수 없습니다.');
      return;
    }
    this._radius = value;
  }

  get radius() {
    return this._radius;
  }
}

const circle = new Circle(5);
console.log(circle.area);   // 78.53... (메서드 호출 아님!)
circle.radius = 10;         // setter 호출
console.log(circle.area);   // 314.15...
```

> ⚠️ getter/setter는 함수처럼 `()`를 붙이지 않고 **속성처럼** 사용합니다.

### 1-4. Public Field Declarations (공개 필드 선언)

`constructor` 없이도 클래스 필드를 바로 선언할 수 있습니다.

```javascript
class Counter {
  count = 0;          // 공개 필드 (인스턴스마다 독립적으로 생성)
  label = '카운터';

  increase() {
    this.count++;
  }
}

const counter = new Counter();
counter.increase();
console.log(counter.count); // 1
```

- `constructor` 안에서 `this.count = 0`으로 쓰는 것과 사실상 동일
- 클래스 본문에 바로 작성해서 **초기값을 한눈에 파악**하기 좋음

---

## 2. 클래스 상속 (Class Inheritance)

### 2-1. 상속 기본 문법 (extends / super)

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name}가 소리를 냅니다.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);      // 부모(Animal)의 constructor 호출 - 필수
    this.breed = breed;
  }
}

const dog = new Dog('바둑이', '진돗개');
dog.speak(); // 바둑이가 소리를 냅니다. (부모 메서드 그대로 사용)
```

- `extends`: 부모 클래스를 상속
- `super()`: 부모의 `constructor` 호출. **자식 클래스에 constructor가 있다면 `this`를 쓰기 전에 반드시 먼저 호출**해야 함 (안 하면 에러)

### 2-2. 메서드 오버라이딩 (Method Overriding)

자식 클래스에서 부모의 메서드를 **같은 이름으로 재정의**하는 것.

```javascript
class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  // 부모의 speak()를 재정의
  speak() {
    console.log(`${this.name}(${this.breed})가 멍멍 짖습니다.`);
  }

  // 부모 메서드도 함께 호출하고 싶을 때
  speakBoth() {
    super.speak();       // 부모의 원래 메서드 호출
    this.speak();        // 자식의 재정의된 메서드 호출
  }
}

const dog = new Dog('바둑이', '진돗개');
dog.speak(); // 바둑이(진돗개)가 멍멍 짖습니다.
```

- `super.메서드명()`으로 부모의 원본 메서드를 호출할 수 있음

### 2-3. 생성자 오버라이딩 (Constructor Overriding)

자식 클래스가 부모와 **다른 매개변수 구조**로 `constructor`를 재정의하는 것.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
    this.type = '동물';
  }
}

class Dog extends Animal {
  // 부모보다 매개변수가 더 많음 (오버라이딩)
  constructor(name, breed, age) {
    super(name);           // 부모 constructor로 name 초기화
    this.breed = breed;
    this.age = age;
    this.type = '개';      // 부모에서 받은 값을 덮어씀
  }
}

const dog = new Dog('바둑이', '진돗개', 3);
console.log(dog); 
// Dog { name: '바둑이', type: '개', breed: '진돗개', age: 3 }
```

> 💡 자식 클래스에서 `constructor`를 생략하면, 부모의 `constructor`가 자동으로 그대로 사용됩니다.

---

## 3. 정적 메서드와 정적 프로퍼티 (static)

**인스턴스가 아닌 클래스 자체**에 속하는 메서드/속성입니다. `new`로 만든 개별 객체가 아니라 클래스 이름으로 직접 접근합니다.

### 3-1. Static Method (정적 메서드)

```javascript
class MathUtil {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }
}

console.log(MathUtil.add(2, 3));       // 5 (클래스에서 바로 호출)
console.log(MathUtil.multiply(2, 3));  // 6

const m = new MathUtil();
// m.add(2, 3); ❌ 에러! 인스턴스에서는 static 메서드 호출 불가
```

- 인스턴스를 만들지 않고 **유틸리티 함수**처럼 사용할 때 유용
- 예: `Array.isArray()`, `Object.keys()`, `Math.random()` 등 내장 static 메서드

### 3-2. Static Property (정적 프로퍼티)

```javascript
class Counter {
  static count = 0; // 모든 인스턴스가 공유하는 값

  constructor() {
    Counter.count++;  // 인스턴스가 생성될 때마다 증가
  }
}

new Counter();
new Counter();
new Counter();

console.log(Counter.count); // 3
```

- 모든 인스턴스가 **공유하는 값**을 저장할 때 사용 (예: 생성된 인스턴스 개수 세기)
- 인스턴스별 데이터가 아니라 **클래스 차원의 데이터**

| 구분 | 일반(인스턴스) 멤버 | static 멤버 |
|------|----------------------|-------------|
| 접근 방법 | `인스턴스.멤버` | `클래스명.멤버` |
| 저장 위치 | 각 인스턴스마다 별도 | 클래스에 단 하나 |
| 용도 | 개별 객체의 데이터 | 공용 유틸/카운터 등 |

---

## 4. 화살표 함수 (Arrow Function)

### 4-1. 기본 문법 (Basic Syntax)

```javascript
// 일반 함수
function add(a, b) {
  return a + b;
}

// 화살표 함수
const add2 = (a, b) => {
  return a + b;
};

// 중괄호 생략 (암묵적 반환, return 없이도 자동 반환)
const add3 = (a, b) => a + b;

// 매개변수가 1개면 괄호 생략 가능
const square = x => x * x;

// 매개변수가 없으면 괄호 필수
const sayHi = () => console.log('Hi!');

// 객체를 바로 반환할 때는 소괄호로 감싸기
const makeObj = (name) => ({ name: name });
```

### 4-2. 화살표 함수의 특징 (Features)

**1) `this`를 자신만의 것으로 갖지 않음 (상위 스코프의 this를 그대로 사용)**

```javascript
class Timer {
  constructor() {
    this.seconds = 0;
  }

  start() {
    // 일반 함수였다면 여기서 this는 window/undefined가 됨
    setInterval(() => {
      this.seconds++;   // 화살표 함수라 Timer 인스턴스의 this 유지
      console.log(this.seconds);
    }, 1000);
  }
}
```

**2) `arguments` 객체가 없음**

```javascript
function normal() {
  console.log(arguments); // 사용 가능
}

const arrow = () => {
  console.log(arguments); // ❌ 에러! (상위 스코프의 arguments를 찾으려 함)
};
```

**3) 생성자로 사용할 수 없음 (`new` 불가)**

```javascript
const Person = (name) => {
  this.name = name;
};

// new Person('철수'); ❌ 에러! Person is not a constructor
```

**4) 메서드로 쓰기에 부적합한 경우가 있음**

```javascript
const obj = {
  name: '철수',
  // ❌ 화살표 함수는 obj를 가리키지 못함 (상위 스코프인 전역의 this 사용)
  greet: () => {
    console.log(this.name); // undefined
  },
  // ✅ 일반 함수를 사용해야 obj를 정상적으로 가리킴
  greet2() {
    console.log(this.name); // 철수
  }
};
```

| 구분 | 일반 함수 | 화살표 함수 |
|------|-----------|-------------|
| `this` | 호출 방식에 따라 결정 | 선언된 위치의 상위 스코프 `this` 고정 |
| `arguments` | 있음 | 없음 |
| 생성자 사용(`new`) | 가능 | 불가능 |
| 객체 메서드 | 적합 | 부적합 (this 문제) |
| 콜백 함수 (setTimeout 등) | this 주의 필요 | this 유지되어 편리 |

---

## 5. 스프레드 / 나머지 문법 (Spread & Rest)

같은 `...` 기호를 쓰지만 **위치(맥락)에 따라 역할이 다릅니다.**

- **나머지(Rest)**: 여러 값을 **하나로 모을 때** (매개변수, 구조 분해 할당의 왼쪽)
- **전개(Spread)**: 하나로 뭉친 것을 **펼칠 때** (배열/객체 리터럴, 함수 호출)

### 5-1. Rest Parameter (나머지 매개변수)

함수에 전달된 인자들을 **배열로 모아서** 받습니다.

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, cur) => acc + cur, 0);
}

console.log(sum(1, 2, 3));       // 6
console.log(sum(1, 2, 3, 4, 5)); // 15

// 일부는 개별 매개변수, 나머지는 rest로
function introduce(name, age, ...hobbies) {
  console.log(`${name}(${age}) 취미: ${hobbies.join(', ')}`);
}
introduce('철수', 20, '축구', '독서', '게임');
// 철수(20) 취미: 축구, 독서, 게임
```

> ⚠️ rest 매개변수는 반드시 **맨 마지막**에 와야 합니다.

### 5-2. Spread Syntax (스프레드 문법, 전개 문법)

배열이나 객체를 **개별 요소로 펼칠 때** 사용합니다.

```javascript
// 배열 펼치기
const arr1 = [1, 2, 3];
console.log(...arr1); // 1 2 3

// 함수 호출 시 배열을 인자로 펼치기
function sum(a, b, c) {
  return a + b + c;
}
console.log(sum(...arr1)); // 6

// 배열 합치기
const arr2 = [4, 5, 6];
const merged = [...arr1, ...arr2];
console.log(merged); // [1, 2, 3, 4, 5, 6]

// 객체 합치기
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj); // { a: 1, b: 2, c: 3, d: 4 }
```

### 5-3. 스프레드 문법을 이용한 배열/객체 복사

```javascript
// 배열 복사 (얕은 복사)
const original = [1, 2, 3];
const copy = [...original];
copy.push(4);
console.log(original); // [1, 2, 3]  (원본 영향 없음)
console.log(copy);     // [1, 2, 3, 4]

// 객체 복사 (얕은 복사)
const originalObj = { name: '철수', age: 20 };
const copyObj = { ...originalObj };
copyObj.age = 30;
console.log(originalObj); // { name: '철수', age: 20 } (원본 유지)
console.log(copyObj);     // { name: '철수', age: 30 }

// 복사하면서 일부 속성 덮어쓰기 (자주 쓰는 패턴)
const updated = { ...originalObj, age: 25 };
console.log(updated); // { name: '철수', age: 25 }
```

> ⚠️ 스프레드는 **얕은 복사(shallow copy)**입니다. 중첩된 객체/배열은 참조가 그대로 복사되므로 내부까지 완전히 독립시키려면 `structuredClone()`이나 깊은 복사가 필요합니다.

```javascript
const nested = { info: { age: 20 } };
const shallow = { ...nested };
shallow.info.age = 99;
console.log(nested.info.age); // 99 (내부 객체는 같이 바뀜! - 얕은 복사의 한계)
```

| 구분 | Rest (나머지) | Spread (전개) |
|------|----------------|----------------|
| 위치 | 매개변수, 구조 분해의 **왼쪽(할당받는 쪽)** | 배열/객체 리터럴, 함수 호출의 **오른쪽(값 넣는 쪽)** |
| 동작 | 여러 값을 **모아서** 하나(배열)로 | 하나로 뭉친 것을 **펼쳐서** 여러 값으로 |
| 예시 | `function f(...args)` | `[...arr1, ...arr2]` |

---

## 6. 구조 분해 할당 (Destructuring Assignment)

배열/객체의 값을 꺼내 개별 변수에 쉽게 할당하는 문법입니다.

### 6-1. 배열 구조 분해 할당 (Array Destructuring)

**기본 문법**

```javascript
const [a, b, c] = [1, 2, 3];
console.log(a, b, c); // 1 2 3
```

**다양한 사용법**

```javascript
// 일부 건너뛰기
const [first, , third] = [1, 2, 3];
console.log(first, third); // 1 3

// 기본값
const [x = 10, y = 20] = [5];
console.log(x, y); // 5 20

// 나머지(rest)와 함께 사용
const [head, ...tail] = [1, 2, 3, 4];
console.log(head, tail); // 1 [2, 3, 4]

// 변수 값 교환 (swap)
let m = 1, n = 2;
[m, n] = [n, m];
console.log(m, n); // 2 1

// 함수 반환값 분해
function getCoords() {
  return [10, 20];
}
const [posX, posY] = getCoords();
```

### 6-2. 객체 구조 분해 할당 (Object Destructuring)

**기본 문법**

배열과 달리 **순서가 아닌 속성 이름(key)** 기준으로 값을 꺼냅니다.

```javascript
const { name, age } = { name: '철수', age: 20 };
console.log(name, age); // 철수 20
```

**다양한 사용법**

```javascript
// 이름 바꾸기 (renaming)
const { name: userName } = { name: '철수' };
console.log(userName); // 철수

// 기본값
const { name, age = 30 } = { name: '철수' };
console.log(age); // 30

// 기본값 + 이름 바꾸기 동시에
const { name: userName2 = '익명' } = {};
console.log(userName2); // 익명

// 나머지 속성(rest)
const { name: n1, ...rest } = { name: '철수', age: 20, city: '서울' };
console.log(n1, rest); // 철수 { age: 20, city: '서울' }
```

### 6-3. 중첩 구조 분해 (Nested Destructuring)

객체나 배열 안에 또 객체/배열이 있을 때, **한 번에 깊숙이 값을 꺼낼 수 있습니다.**

```javascript
// 중첩 객체
const user = {
  name: '철수',
  address: {
    city: '서울',
    zip: '12345'
  }
};
const { address: { city, zip } } = user;
console.log(city, zip); // 서울 12345

// 중첩 배열
const arr = [1, [2, 3], 4];
const [a, [b, c], d] = arr;
console.log(a, b, c, d); // 1 2 3 4

// 배열+객체 혼합 중첩
const data = { name: '철수', scores: [90, 85, 100] };
const { name, scores: [first, second, third] } = data;
console.log(name, first, second, third); // 철수 90 85 100
```

> ⚠️ 중첩 구조 분해 시, 중간 속성이 `undefined`이면 에러가 납니다. `spec: { cpu } = {}` 처럼 **기본값을 반드시 지정**해줘야 안전합니다.

```javascript
function printSpec({ spec: { cpu } = {} } = {}) {
  console.log(cpu);
}
printSpec(); // undefined (에러 없음)
```

### 6-4. 함수 매개변수에서의 구조 분해 (Function Parameters)

함수의 인자로 객체/배열을 받을 때 매우 자주 사용됩니다.

```javascript
// 객체 매개변수 구조 분해
function printUser({ name, age }) {
  console.log(`${name}은 ${age}살입니다.`);
}
printUser({ name: '영희', age: 25 });

// 배열 매개변수 구조 분해
function sum([a, b]) {
  return a + b;
}
sum([3, 4]); // 7

// 기본값과 함께 (매개변수 자체가 안 넘어와도 안전)
function greet({ name = '손님' } = {}) {
  console.log(`안녕하세요, ${name}님!`);
}
greet();                 // 안녕하세요, 손님님!
greet({ name: '철수' }); // 안녕하세요, 철수님!

// 여러 옵션(설정 객체)을 받을 때 흔히 쓰는 패턴
function createUser({ name, age, role = '사용자' } = {}) {
  console.log(name, age, role);
}
createUser({ name: '철수', age: 20 }); // 철수 20 사용자
```

---

## 📌 전체 한눈에 정리

| 주제 | 핵심 키워드 |
|------|--------------|
| 클래스 기본 | `class`, `constructor`, `get`/`set`, 필드 선언 |
| 클래스 상속 | `extends`, `super()`, 메서드/생성자 오버라이딩 |
| static | `static` 메서드/프로퍼티 (클래스 자체 소속) |
| 화살표 함수 | `this` 고정, `arguments` 없음, 생성자 불가 |
| 스프레드/나머지 | `...` (모으기 vs 펼치기), 얕은 복사 |
| 구조 분해 할당 | 배열(순서 기준) vs 객체(이름 기준), 중첩, 함수 매개변수 |