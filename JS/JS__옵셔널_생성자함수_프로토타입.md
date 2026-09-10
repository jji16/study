# JavaScript 정리노트 — 옵셔널 체이닝 / null 병합 / 생성자 함수 / 프로토타입

---

## 1. 옵셔널 체이닝 연산자 (`?.`)

좌항이 `null` 또는 `undefined`이면 즉시 `undefined`를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다. **에러 없이** 안전하게 중첩된 프로퍼티에 접근할 때 사용한다.

```js
const obj = null;
const val = obj?.value; // undefined (TypeError 발생 안 함)
```

### `&&` 단축 평가와의 차이

```js
const str = '';
const len1 = str && str.length; // '' (빈 문자열이 Falsy라서 여기서 멈춤 → 원하는 값이 아님)
const len2 = str?.length;       // 0  (null/undefined가 아니므로 정상적으로 length 반환)
```

`&&`는 **Falsy 여부**로 판단하지만, `?.`는 오직 **`null` / `undefined` 여부**로만 판단한다.

### 📌 추가로 알아두면 좋은 활용

| 문법           |    용도           | 예시                                             |
|---------------|-------------------|-------------------------------------------------|
| `obj?.prop`   | 프로퍼티 접근       | `user?.profile?.email`                          |
| `obj?.[expr]` | 대괄호(동적 키) 접근 | `arr?.[0]`, `obj?.["key"]`                      |
| `func?.()`    | 함수 호출           | `callback?.()` — callback이 없어도 에러 없이 무시  |

- 체이닝 중간에 `null`/`undefined`를 만나면 **뒤 표현식은 평가되지 않고(단락 평가) 즉시 undefined 반환**한다.
- 좌항이 아예 선언되지 않은 변수면 `?.`를 써도 `ReferenceError`는 발생한다 (선언은 되어 있어야 함).

---

## 2. null 병합 연산자 (`??`)

좌항이 `null` 또는 `undefined`일 때만 우항 값을 반환하고, 그 외에는 좌항 값을 그대로 반환한다.

```js
const test = null ?? '기본 값';  // '기본 값'
const value = '' ?? '기본 값';   // '' (빈 문자열은 null/undefined가 아니므로 좌항 그대로)
```

### `||`와의 차이 (기존 파일에서 다룬 내용)

```js
const value = '' || '기본 값'; // '기본 값' (빈 문자열도 Falsy라서 대체됨 — 의도와 다를 수 있음)
```

`0`, `''`, `false`처럼 "값은 있지만 Falsy한" 데이터를 유효한 값으로 취급하고 싶다면 `||` 대신 `??`를 써야 한다.

### 📌 추가로 알아두면 좋은 내용 (파일에 없던 내용)

- **논리 연산자와 섞어 쓰기 금지**: `a || b ?? c`처럼 `??`를 `&&`/`||`와 괄호 없이 함께 쓰면 `SyntaxError`가 발생한다. 반드시 `(a || b) ?? c`처럼 괄호로 우선순위를 명시해야 한다.
- **null 병합 할당 연산자 `??=`**: 값이 없을 때만 기본값을 대입.
  ```js
  let config = {};
  config.timeout ??= 3000; // timeout이 null/undefined일 때만 3000 대입
  ```
- **옵셔널 체이닝과 조합**하면 실무에서 매우 자주 쓰는 패턴이 된다.
  ```js
  
  const user = {
    profile:{
        name: '홍길동',
        email: 'aa@aa.com'
    }
  };

  const email = user?.profile?.email ?? '이메일 없음';
  ```

---

## 3. 생성자 함수 (Constructor Function)

객체 리터럴을 여러 개 반복해서 만드는 대신, **틀(템플릿)**을 만들어 `new`로 찍어내는 방식.

```js
function Studnet(name, age) { // 관례상 첫 글자 대문자
    this.name = name;
    this.age = age;
    this.getInfo = function () {
        return `${this.name}는 ${this.age}세 입니다.`;
    };
}

const student3 = new Studnet('원숭이', 40);
const student4 = new Studnet('고릴라', 30);

console.log(student3 === student4); 
// false → 서로 다른 독립된 객체
// new Studnet(...)를 각각 따로 호출할 때마다 메모리에 새로운 객체가 만들어지므로,
// student3과 student4는 서로 다른(독립된) 객체를 가리키게 된다.
```

### `new` 키워드가 내부적으로 하는 일

1. 빈 객체(`{}`)를 하나 생성한다. (빈 상자를 하나 준비)
2. 그 객체를 함수의 `this`로 바인딩한다. (상자에 "this"라는 이름표를 붙임)
3. 함수 본문을 실행하며 `this`에 프로퍼티를 채운다. (상자 안에 물건(name, age)을 담음)
4. `return`으로 다른 객체를 명시하지 않으면, 완성된 `this`를 자동으로 반환한다. (다 채운 상자를 그대로 건네줌)

그냥 일반 function student(){...} 안에 this.getInfo() = function() {...}를 사용하면
new를 생성할 때마다 계속해서 메모리를 사용하기 때문에 메모리 낭비가 된다.
그것을 방지하기 위해서 prototype을 사용

Studnet.prototype.getInfo = function () {
    return `${this.name}는 ${this.age}세 입니다.`;
};
s1.getInfo === s2.getInfo; // true ← 하나의 함수를 공유

### 📌 추가로 알아두면 좋은 내용

- **`new`를 빼먹으면?** 이 예제 파일들처럼 `this.name = name` 형태로 작성된 함수를 `new` 없이 호출하면, non-strict 모드에서는 `this`가 전역 객체(브라우저는 `window`)를 가리켜 전역 변수를 오염시키는 심각한 버그가 된다. (`02_instance-creation-process.js`에서 `'use strict'`를 쓴 이유가 바로 이 문제를 막기 위함 — strict 모드에서는 `this`가 `undefined`가 되어 즉시 에러로 드러난다.)

- **메서드를 생성자 안에 정의하면 비효율적**: `this.getInfo = function(){...}`처럼 쓰면 인스턴스를 만들 때마다 함수가 **매번 새로** 생성되어 메모리를 낭비한다. → 이 문제의 해결책이 바로 아래 6번의 `prototype`이다.

- 화살표 함수는 자기 `this`를 갖지 않으므로(상위 스코프의 this를 그대로 씀) **생성자 함수로 사용할 수 없다** (`new` 호출 시 `TypeError`).

---

## 4. 인스턴스 생성 과정 & `new.target`

```js
function Dog(name, age) {
    if (!new.target) {          // new 없이 호출되면 new.target은 undefined
        console.log('new 없이 호출함, new 붙이셈.');
        return new Dog(name, age); // 강제로 new를 붙여 재호출
    }
    this.name = name;
    this.age = age;
}

const dog = Dog('바둑이', 3); // new 없이 호출해도 안전하게 방어됨
```

`new.target`은 `new`로 호출됐을 때는 해당 생성자 함수 자신을, 일반 함수처럼 호출됐을 때는 `undefined`를 가리킨다. 이를 이용하면 사용자가 실수로 `new`를 빠뜨려도 방어 코드로 안전하게 처리할 수 있다.

### `new` 유무에 따른 차이 요약 (파일 주석 표 정리)

| 구분       | `new` 없이 호출 | `new`로 호출 |
|------------------|--------------------------------------|----------------------------------------- |
| `this`           | 전역 객체(또는 strict 모드에선 `undefined`) | 새로 만든 빈 객체                       |
| 프로토타입 연결    | 안 됨                                     | `prototype` 체인 자동 연결             |
| 반환값            | `return` 명시 안 하면 `undefined`          | `return` 명시 안 해도 `this` 자동 반환  |
| 매 호출마다 새 객체 | 보장 안 됨                                | 항상 새로운 독립 객체 생성               |

---

## 5. 프로토타입 체인 (`Object.create`)

`Object.create(부모객체)`로 새 객체를 만들면, 이 객체는 **자신에게 없는 프로퍼티를 찾을 때 부모(프로토타입)까지 거슬러 올라가서** 찾아본다. 이 연결 고리를 **프로토타입 체인**이라 부른다.

```js
const user = {
    id: 'user',
    activate: true,
    login() { console.log(`${this.id}님이 로그인 되었습니다.`); }
};

const student = Object.create(user);
student.passion = true;

student.activate; // true → 자신에게 없어서 user까지 올라가서 찾음
Object.getPrototypeOf(student) === user; // true

Object.hasOwn(student, 'activate'); // false → 직접 소유 X (프로토타입 소유)
Object.hasOwn(student, 'passion');  // true  → 직접 소유 O
'activate' in student;              // true  → 프로토타입 체인까지 포함해서 검사
```
## prototype을 사용하는 이유

prototype은 단순히 메모리 낭비를 줄이는 것뿐 아니라, **상속**의 기반이 되는 개념이다.

- **공유**: 같은 생성자로 만든 모든 인스턴스가 prototype에 정의된 메서드를 하나만 만들어서 함께 사용한다 → 메모리 절약
- **상속**: 프로토타입 체인을 통해 상위 객체(부모)의 프로퍼티와 메서드를 하위 객체(자식)가 그대로 물려받아 쓸 수 있다

이 덕분에 중복 코드를 줄이고, 공통 기능을 한 곳에 모아 관리할 수 있어 **코드 가독성과 간결함**이 좋아진다.

### 핵심 포인트 정리

- **`this`는 호출 시점의 "점(.) 앞의 객체"를 가리킨다.** `greedyStudent.login()`을 호출하면 `login` 메서드는 `user`에 있지만, `this.id`는 호출한 주체인 `greedyStudent.id`를 가리킨다.
- **`delete`는 자기 자신이 직접 가진 프로퍼티만 지울 수 있다.** `greedyStudent.id`를 삭제하면 다시 프로토타입 체인을 타고 올라가 `student` → `user`의 `id`를 참조하게 된다.

### 📌 추가로 알아두면 좋은 내용

- `Object.hasOwn(obj, key)` (ES2022, 신문법)은 기존의 `obj.hasOwnProperty(key)`를 대체하는 더 안전한 방식이다. `Object.create(null)`처럼 프로토타입이 없는 객체에서도 안전하게 동작한다.
- `in` 연산자는 **자신 + 프로토타입 체인 전체**를 검사하지만, `hasOwn`은 **자기 자신만** 검사한다는 차이를 꼭 구분해야 한다.
- 프로토타입 체인은 결국 `Object.prototype`까지 올라가고, 그 위(`Object.prototype`의 프로토타입)는 `null`이다. 이 지점이 체인의 끝이다.

---

## 6. 생성자 함수의 `prototype` 프로퍼티

모든 함수는 기본적으로 `prototype`이라는 프로퍼티(객체)를 가지고 있다. `new`로 인스턴스를 만들면, 그 인스턴스의 내부 `[[Prototype]]`이 생성자 함수의 `prototype` 객체를 가리키게 자동 연결된다.

```js
function Student(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype.activate = true;
Student.prototype.getInfo = function () {
    return `${this.name}는 ${this.age}세 입니다.`;
};

const student3 = new Student('홍길동', 20);
const student4 = new Student('장보고', 30);

Object.getPrototypeOf(student3) === Student.prototype; // true
Object.hasOwn(student3, 'getInfo');                     // false → 공유 객체에 정의됨
student3.getInfo === student4.getInfo;                  // true  → 동일한 함수를 "공유"
```

### 왜 `prototype`에 메서드를 정의하는가? (3번 항목의 문제 해결)

- `this.getInfo = function(){...}`처럼 생성자 내부에 메서드를 두면 인스턴스마다 함수가 **각각 새로 생성**되어 메모리를 낭비한다.
- 반면 `Student.prototype.getInfo = ...`처럼 정의하면, **모든 인스턴스가 동일한 함수 하나를 공유**한다 (`student3.getInfo === student4.getInfo` → `true`).

### `[[Prototype]]` vs `prototype` 구분 (파일 하단 주석 정리)

| 구분                           | 설명                                                                              |
|-------------------------------|-----------------------------------------------------------------------------------|
| `prototype`                   | **생성자 함수만** 갖는 프로퍼티. 이 함수로 만들어질 인스턴스들의 "부모"가 될 객체를 담고 있음 |
| `[[Prototype]]` (`__proto__`) | **모든 객체**가 내부에 숨겨서 갖는, "내 부모 객체가 누구인지"를 가리키는 연결고리           |

`new`로 인스턴스를 생성하면 → 인스턴스의 `[[Prototype]]`이 생성자 함수의 `prototype`을 가리키도록 연결되고 → 인스턴스에 없는 프로퍼티를 찾을 땐 이 연결을 타고 올라간다 (프로토타입 체인).

### 📌 추가로 알아두면 좋은 내용

- `Book` 예제처럼 `getTotal`을 `prototype`에 두면, `book1.price`만 바꿔도 `book1.getTotal(1)`은 자기 자신의 `price`를 참조하므로 정상 동작한다 — **메서드는 공유하지만 데이터(`price`, `title`)는 인스턴스별로 독립적**이라는 점이 핵심이다.
- ES6의 `class` 문법은 사실 이 `함수 + prototype` 패턴을 더 읽기 쉽게 감싼 **문법적 설탕(syntactic sugar)** 이다. 내부 동작 원리는 완전히 동일하다.
  ```js
  class Student {
      constructor(name, age) { this.name = name; this.age = age; }
      getInfo() { return `${this.name}는 ${this.age}세 입니다.`; } // 자동으로 prototype에 등록됨
  }
  ```
- `Object.create(부모)`(5번) vs `new 생성자함수()`(6번)는 결국 같은 목적(프로토타입 체인 연결)을 서로 다른 방식으로 달성하는 두 가지 접근법이다.

---

## 전체 핵심 요약

| 주제              | 한 줄 요약 |
|------------------|---------------------------------------------------|
| `?.` 옵셔널 체이닝 | null/undefined일 때만 안전하게 멈추고 undefined 반환  |
| `??` null 병합    | null/undefined일 때만 기본값으로 대체 (Falsy 전체 X)  |
| 생성자 함수        | `new`로 호출해 동일한 형태의 객체를 반복 생성하는 틀    |
| `new.target`     | 생성자 함수가 `new` 없이 호출됐는지 방어적으로 검사      |
| `Object.create`  | 특정 객체를 프로토타입으로 지정해 새 객체 생성           |
| `함수.prototype`  | 그 함수로 만든 모든 인스턴스가 공유하는 메서드 저장소    |