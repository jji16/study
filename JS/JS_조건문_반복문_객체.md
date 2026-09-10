# JavaScript 조건문, 반복문, 객체(Object) 정리

## 1. if문

`if`문은 **특정 조건이 참(true)일 때 코드를 실행하는 조건문**이다.

### 기본 구조

```javascript
if (조건) {
  실행할 코드;
}
```

### 예제

```javascript
const age = 20;

if (age >= 18) {
  console.log("성인입니다.");
}
```

`age >= 18`이라는 조건이 `true`이므로 중괄호 `{}` 안의 코드가 실행된다.

---

### if...else문

조건이 거짓(false)일 경우 다른 코드를 실행하고 싶을 때 사용한다.

```javascript
const age = 15;

if (age >= 18) {
  console.log("성인입니다.");
} else {
  console.log("미성년자입니다.");
}
```

---

### if...else if...else문

여러 개의 조건을 순서대로 확인할 때 사용한다.

```javascript
const score = 85;

if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else if (score >= 70) {
  console.log("C");
} else {
  console.log("D");
}
```

조건은 위에서부터 순서대로 검사하며, 조건을 만족하면 해당 코드가 실행되고 이후 조건은 검사하지 않는다.

---

# 2. switch문

`switch`문은 **하나의 값에 따라 여러 가지 경우를 처리할 때 사용하는 조건문**이다.

### 기본 구조

```javascript
switch (값) {
  case 값1:
    실행할 코드;
    break;

  case 값2:
    실행할 코드;
    break;

  default:
    실행할 코드;
}
```

### 예제

```javascript
const fruit = "apple";

switch (fruit) {
  case "apple":
    console.log("사과입니다.");
    break;

  case "banana":
    console.log("바나나입니다.");
    break;

  case "orange":
    console.log("오렌지입니다.");
    break;

  default:
    console.log("알 수 없는 과일입니다.");
}
```

### break가 필요한 이유

`break`가 없으면 조건을 만족한 이후에도 다음 `case`가 계속 실행될 수 있다.

```javascript
const number = 1;

switch (number) {
  case 1:
    console.log("1입니다.");

  case 2:
    console.log("2입니다.");
}
```

결과:

```text
1입니다.
2입니다.
```

따라서 일반적으로 각 `case`의 마지막에는 `break`를 작성한다.

---

# 3. while문

`while`문은 **조건이 참(true)인 동안 코드를 반복 실행하는 반복문**이다.

### 기본 구조

```javascript
while (조건) {
  반복할 코드;
}
```

### 예제

```javascript
let number = 1;

while (number <= 5) {
  console.log(number);
  number++;
}
```

실행 결과:

```text
1
2
3
4
5
```

`number`가 5 이하인 동안 반복되고, `number++`를 통해 값이 1씩 증가한다.

---

## 무한 반복 주의

조건이 계속 `true`이면 반복문이 종료되지 않는다.

```javascript
while (true) {
  console.log("무한 반복");
}
```

다음 코드도 문제가 발생한다.

```javascript
let number = 1;

while (number <= 5) {
  console.log(number);
}
```

`number`의 값이 변경되지 않기 때문에 조건이 계속 참이 되어 무한 반복된다.

---

# 4. for문

`for`문은 **특정 횟수만큼 반복해야 할 때 주로 사용하는 반복문**이다.

### 기본 구조

```javascript
for (초기값; 조건식; 증감식) {
  반복할 코드;
}
```

### 예제

```javascript
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
```

구조:

```javascript
for (
  let i = 1; // 초기값
  i <= 5;    // 조건식
  i++        // 증감식
) {
  console.log(i);
}
```

실행 결과:

```text
1
2
3
4
5
```

### for문의 실행 과정

```text
1. 초기값 설정
2. 조건 확인
3. 코드 실행
4. 증감식 실행
5. 다시 조건 확인
```

조건이 `false`가 될 때까지 반복한다.

---

# 5. continue문

`continue`문은 **현재 반복을 건너뛰고 다음 반복으로 이동하는 문장**이다.

### 예제

```javascript
for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    continue;
  }

  console.log(i);
}
```

실행 결과:

```text
1
2
4
5
```

`i`가 3일 때 `continue`가 실행된다.

따라서 아래의 `console.log(i)`는 실행하지 않고 다음 반복으로 이동한다.

### 실행 과정

```text
1 → 실행
2 → 실행
3 → 건너뜀
4 → 실행
5 → 실행
```

---

# 6. break문

`break`문은 **반복문이나 switch문을 즉시 종료하는 문장**이다.

### for문에서 사용

```javascript
for (let i = 1; i <= 10; i++) {
  if (i === 5) {
    break;
  }

  console.log(i);
}
```

실행 결과:

```text
1
2
3
4
```

`i`가 5가 되면 `break`가 실행되고 반복문 전체가 종료된다.

---

## continue와 break의 차이

| 키워드        | 역할                     |
| ---------- | ---------------------- |
| `continue` | 현재 반복만 건너뛰고 다음 반복으로 이동 |
| `break`    | 반복문 전체를 종료             |

### continue 예제

```javascript
for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    continue;
  }

  console.log(i);
}
```

결과:

```text
1
2
4
5
```

3만 건너뛰고 반복문은 계속 실행된다.

### break 예제

```javascript
for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    break;
  }

  console.log(i);
}
```

결과:

```text
1
2
```

3이 되는 순간 반복문 전체가 종료된다.

---

# 7. Object(객체)

JavaScript의 객체(Object)는 **여러 개의 데이터를 하나로 묶어서 관리할 수 있는 자료형**이다.

객체는 일반적으로 `키(Key)`와 `값(Value)`의 형태로 데이터를 저장한다.

### 기본 구조

```javascript
const 객체이름 = {
  키: 값,
  키: 값
};
```

### 예제

```javascript
const user = {
  name: "홍길동",
  age: 25,
  job: "개발자"
};
```

구조:

```text
user
 ├── name: "홍길동"
 ├── age: 25
 └── job: "개발자"
```

여기서:

* `name`, `age`, `job` → 키(Key) 또는 속성(Property)
* `"홍길동"`, `25`, `"개발자"` → 값(Value)

---

## 객체의 값 가져오기

### 1. 점 표기법

객체 이름 뒤에 `.`을 사용하여 값을 가져온다.

```javascript
console.log(user.name);
console.log(user.age);
```

결과:

```text
홍길동
25
```

---

### 2. 대괄호 표기법

대괄호 `[]`를 사용해서 값을 가져올 수도 있다.

```javascript
console.log(user["name"]);
console.log(user["age"]);
```

점 표기법과 대괄호 표기법 모두 객체의 값을 가져올 수 있다.

---

## 객체의 값 변경

객체의 속성 값을 변경할 수 있다.

```javascript
const user = {
  name: "홍길동",
  age: 25
};

user.age = 30;

console.log(user.age);
```

결과:

```text
30
```

---

## 객체에 새로운 속성 추가

객체를 생성한 후 새로운 데이터를 추가할 수도 있다.

```javascript
const user = {
  name: "홍길동"
};

user.age = 25;

console.log(user);
```

결과:

```javascript
{
  name: "홍길동",
  age: 25
}
```

---

## 객체의 함수: 메서드(Method)

객체에는 데이터뿐만 아니라 함수도 저장할 수 있다.

객체 내부에 저장된 함수를 **메서드(Method)**라고 한다.

```javascript
const user = {
  name: "홍길동",

  introduce: function () {
    console.log("안녕하세요.");
  }
};

user.introduce();
```

실행 결과:

```text
안녕하세요.
```

---

# 8. 전체 핵심 정리

| 문법          | 역할                 |
| ----------- | ------------------ |
| `if`        | 조건에 따라 코드 실행       |
| `if...else` | 조건에 따라 서로 다른 코드 실행 |
| `switch`    | 하나의 값에 따라 여러 경우 처리 |
| `while`     | 조건이 참인 동안 반복       |
| `for`       | 특정 횟수만큼 반복         |
| `continue`  | 현재 반복을 건너뛰고 다음 반복  |
| `break`     | 반복문 또는 switch문 종료  |
| `Object`    | 여러 데이터를 하나로 묶어서 관리 |

---

# 9. 종합 예제

```javascript
const user = {
  name: "홍길동",
  age: 25
};

if (user.age >= 20) {
  console.log(user.name + "님은 성인입니다.");
}

for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    continue;
  }

  if (i === 5) {
    break;
  }

  console.log(i);
}
```

실행 과정:

1. 객체 `user`에 이름과 나이를 저장한다.
2. `if`문으로 나이가 20 이상인지 확인한다.
3. `for`문으로 1부터 반복한다.
4. 숫자가 3이면 `continue`를 사용하여 건너뛴다.
5. 숫자가 5이면 `break`를 사용하여 반복문을 종료한다.

---

