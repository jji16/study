# JavaScript 기초 - 변수와 타입 변환

## 1. 변수란?

변수(Variable)는 **데이터를 저장하기 위한 공간**입니다.

JavaScript에서는 주로 `let`과 `const`를 사용하여 변수를 만듭니다.

---

# 2. 변수 선언

**변수 선언**은 변수를 만드는 것입니다.

```javascript
let name;
```

위 코드에서 `name`이라는 변수가 만들어집니다.

아직 값을 할당하지 않았으므로 다음과 같이 출력하면:

```javascript
console.log(name);
```

결과는 다음과 같습니다.

```text
undefined
```

`undefined`는 **값이 아직 할당되지 않은 상태**를 의미합니다.

---

# 3. 변수 할당

**할당(Assignment)**은 변수에 값을 저장하는 것을 의미합니다.

```javascript
let name;

name = "홍길동";
```

위 코드는 두 단계로 이루어집니다.

```javascript
// 1. 변수 선언
let name;

// 2. 변수에 값 할당
name = "홍길동";
```

`=`는 오른쪽의 값을 왼쪽 변수에 저장한다는 의미입니다.

```text
name = "홍길동"
```

즉:

```text
"홍길동" → name 변수에 저장
```

---

# 4. 선언과 동시에 할당

변수를 선언하면서 동시에 값을 저장할 수도 있습니다.

```javascript
let name = "홍길동";
```

이를 **선언과 동시에 할당**한다고 합니다.

다른 예시:

```javascript
let age = 20;
```

```javascript
const price = 10000;
```

선언과 할당을 따로 작성하면:

```javascript
let name;
name = "홍길동";
```

선언과 동시에 작성하면:

```javascript
let name = "홍길동";
```

실제 개발에서는 대부분 필요한 경우 선언과 동시에 값을 할당합니다.

---

# 5. let과 const

## let

`let`은 값을 나중에 변경할 수 있는 변수입니다.

```javascript
let age = 20;

age = 21;

console.log(age);
```

결과:

```text
21
```

---

## const

`const`는 한 번 선언한 변수에 **다른 값을 재할당할 수 없습니다.**

```javascript
const name = "홍길동";

console.log(name);
```

다음과 같이 재할당하면 오류가 발생합니다.

```javascript
const name = "홍길동";

name = "김철수"; // 오류
```

기본적으로 값이 변경될 필요가 없다면 `const`를 사용하는 습관을 들이는 것이 좋습니다.

---

# 6. 변수명 규칙

변수 이름을 만들 때는 몇 가지 규칙이 있습니다.

## ① 영문자, 숫자, `_`, `$` 사용 가능

```javascript
let name;
let age1;
let user_name;
let $price;
```

---

## ② 숫자로 시작할 수 없음

잘못된 예시:

```javascript
let 1name = "홍길동";
```

올바른 예시:

```javascript
let name1 = "홍길동";
```

숫자는 변수명의 중간이나 끝에 사용할 수 있습니다.

---

## ③ 공백을 사용할 수 없음

잘못된 예시:

```javascript
let user name = "홍길동";
```

올바른 예시:

```javascript
let userName = "홍길동";
```

또는:

```javascript
let user_name = "홍길동";
```

---

## ④ 예약어는 사용할 수 없음

JavaScript에서 특별한 기능을 위해 이미 사용하는 단어를 **예약어**라고 합니다.

예를 들어 다음과 같은 단어는 변수명으로 사용할 수 없습니다.

```javascript
let if = 10;    // 오류
let for = 20;   // 오류
let const = 30; // 오류
```

---

## ⑤ 대소문자를 구별함

JavaScript는 대소문자를 구별합니다.

```javascript
let name = "홍길동";
let Name = "김철수";
```

`name`과 `Name`은 서로 다른 변수입니다.

```javascript
console.log(name);
console.log(Name);
```

결과:

```text
홍길동
김철수
```

---

## ⑥ 의미 있는 이름 사용하기

가능하면 변수의 역할을 쉽게 알 수 있도록 이름을 작성하는 것이 좋습니다.

좋지 않은 예시:

```javascript
let a = "홍길동";
let b = 20;
```

좋은 예시:

```javascript
let userName = "홍길동";
let userAge = 20;
```

JavaScript에서는 여러 단어로 이루어진 변수명에 **camelCase(카멜 케이스)**를 많이 사용합니다.

```javascript
let userName;
let userAge;
let productPrice;
```

첫 번째 단어는 소문자로 시작하고, 다음 단어의 첫 글자를 대문자로 작성합니다.

---

# 7. console.log()

`console.log()`는 값을 **브라우저 개발자 도구의 콘솔(Console)에 출력하는 기능**입니다.

문자열 출력:

```javascript
console.log("안녕하세요");
```

숫자 출력:

```javascript
console.log(100);
```

변수 출력:

```javascript
const name = "홍길동";

console.log(name);
```

결과:

```text
홍길동
```

여러 값을 함께 출력할 수도 있습니다.

```javascript
const name = "홍길동";
const age = 20;

console.log(name, age);
```

---

# 8. 백틱(`)과 템플릿 리터럴

문자열은 큰따옴표(`"`) 또는 작은따옴표(`'`)로 작성할 수 있습니다.

```javascript
const name = "홍길동";

console.log("안녕하세요");
```

하지만 문자열 안에 변수의 값을 넣을 때는 **백틱(`)**을 사용하면 편리합니다.

```javascript
const name = "홍길동";
const age = 20;

console.log(`${name}의 나이는 ${age}살입니다.`);
```

결과:

```text
홍길동의 나이는 20살입니다.
```

## `${}`의 의미

백틱 안에서 `${변수명}`을 사용하면 변수의 값을 문자열에 넣을 수 있습니다.

```javascript
const name = "홍길동";

console.log(`안녕하세요, ${name}님`);
```

결과:

```text
안녕하세요, 홍길동님
```

---

# 9. 변수에 현재 들어 있는 값에 따라 타입이 결정

JavaScript는 **동적 타입 언어(Dynamically Typed Language)**입니다.

JavaScript에서는 변수를 선언할 때 일반적으로 타입을 직접 지정하지 않습니다.

```javascript
let data = 10;
```

현재 `data`에는 숫자 `10`이 들어 있으므로 숫자 타입입니다.

```javascript
console.log(typeof data);
```

결과:

```text
number
```

하지만 같은 변수에 문자열을 저장할 수도 있습니다.

```javascript
let data = 10;

data = "안녕하세요";

console.log(typeof data);
```

결과:

```text
string
```

즉, JavaScript에서는 **현재 변수에 들어 있는 값에 따라 타입이 결정됩니다.**

```javascript
let data = 10;

console.log(typeof data); // number

data = "Hello";

console.log(typeof data); // string

data = true;

console.log(typeof data); // boolean
```

---

# 10. typeof

`typeof`는 값이나 변수의 데이터 타입을 확인할 때 사용합니다.

```javascript
console.log(typeof "Hello");
```

결과:

```text
string
```

숫자:

```javascript
console.log(typeof 10);
```

결과:

```text
number
```

논리값:

```javascript
console.log(typeof true);
```

결과:

```text
boolean
```

변수에도 사용할 수 있습니다.

```javascript
const age = 20;

console.log(typeof age);
```

결과:

```text
number
```

---

# 11. 암묵적 타입 변환

**암묵적 타입 변환(Implicit Type Conversion)**은 개발자가 직접 타입을 변경하지 않아도 JavaScript가 자동으로 타입을 변환하는 것입니다.

## 문자열과 숫자의 덧셈

```javascript
console.log("10" + 20);
```

결과:

```text
1020
```

JavaScript가 숫자 `20`을 문자열 `"20"`으로 자동 변환합니다.

```text
"10" + 20
```

↓

```text
"10" + "20"
```

↓

```text
"1020"
```

따라서 결과의 타입은 문자열입니다.

```javascript
const result = "10" + 20;

console.log(typeof result);
```

결과:

```text
string
```

---

## 문자열과 숫자의 뺄셈

```javascript
console.log("10" - 5);
```

결과:

```text
5
```

이번에는 JavaScript가 문자열 `"10"`을 숫자 `10`으로 자동 변환합니다.

```text
"10" - 5
```

↓

```text
10 - 5
```

↓

```text
5
```

---

## `==` 비교에서의 암묵적 타입 변환

```javascript
console.log(10 == "10");
```

결과:

```text
true
```

`==`는 비교하는 과정에서 타입을 자동으로 변환할 수 있습니다.

반면 `===`는 값과 타입을 함께 비교합니다.

```javascript
console.log(10 === "10");
```

결과:

```text
false
```

따라서 일반적으로 `==`보다 `===`를 사용하는 것이 권장됩니다.

---

# 12. 명시적 타입 변환

**명시적 타입 변환(Explicit Type Conversion)**은 개발자가 직접 원하는 데이터 타입으로 변환하는 것입니다.

대표적인 방법은 다음과 같습니다.

```javascript
Number()
String()
Boolean()
```

---

## Number()

`Number()`는 값을 숫자 타입으로 변환합니다.

```javascript
const value = "100";

const numberValue = Number(value);

console.log(numberValue);
console.log(typeof numberValue);
```

결과:

```text
100
number
```

문자열로 받은 숫자를 계산할 때 사용할 수 있습니다.

```javascript
const price = "10000";
const quantity = "2";

const result = Number(price) * Number(quantity);

console.log(result);
```

결과:

```text
20000
```

---

## String()

`String()`은 값을 문자열 타입으로 변환합니다.

```javascript
const age = 20;

const textAge = String(age);

console.log(textAge);
console.log(typeof textAge);
```

결과:

```text
20
string
```

---

## Boolean()

`Boolean()`은 값을 `true` 또는 `false`로 변환합니다.

```javascript
console.log(Boolean(1));
```

결과:

```text
true
```

```javascript
console.log(Boolean(0));
```

결과:

```text
false
```

대표적인 변환 결과는 다음과 같습니다.

| 값 | Boolean 변환 결과 |
|---|---|
| `true` | `true` |
| `false` | `false` |
| `1` | `true` |
| `0` | `false` |
| `"Hello"` | `true` |
| `""` | `false` |
| `null` | `false` |
| `undefined` | `false` |

---

# 13. 전체 예제

```javascript
// 변수 선언
let name;

// 값 할당
name = "홍길동";

// 선언과 동시에 할당
let age = 20;

console.log(name);
console.log(age);

// 백틱을 사용하여 변수 값을 문자열에 넣기
console.log(`${name}의 나이는 ${age}살입니다.`);


// 변수에 저장된 값에 따라 타입 결정
let data = 10;

console.log(typeof data); // number

data = "Hello";

console.log(typeof data); // string

data = true;

console.log(typeof data); // boolean


// 암묵적 타입 변환
console.log("10" + 20); // "1020"
console.log("10" - 5);  // 5


// 명시적 타입 변환
const value = "100";

console.log(Number(value)); // 100
console.log(String(100));   // "100"
console.log(Boolean(1));    // true
```

