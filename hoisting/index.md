# [JavaScript] 자바스크립트 호이스팅 쉽게 이해하기 (Hoisting)

# 들어가며

![](https://velog.velcdn.com/images/clydehan/post/fdea1387-7e85-4a58-abb7-29ea7aa11baa/image.png)

> 이미지 출처: [Youtube_Teddy Smith](https://www.youtube.com/watch?app=desktop&v=pJ_oKVFHMK0)

자바스크립트에서 **호이스팅(Hoisting)** 은 변수와 함수 선언이 해당 범위의 최상단으로 끌어올려지는 동작을 의미한다. 이 포스트에서는 호이스팅의 기본 개념, 변수와 함수 선언에서의 호이스팅 동작, 그리고 호이스팅이 코드에 미치는 영향에 대해 다룬다.

❗️ 또한, 변수 선언 및 함수 표현식 사용 시 `var`는 사용하지 않는 추세이지만, 호이스팅의 원리를 이해하기 위해 `var`를 함께 설명한다. `var`는 `let`과 `const`가 도입되기 전에 사용된 변수 선언 방법으로, 호이스팅의 개념을 이해하는 데 도움이 된다.

---

## 📌 변수 선언에서 var를 사용하지 않는 이유

변수 선언 시 `var`는 다음과 같은 이유 때문에 사용하지 않는 추세이며, 대신 `let`과 `const`를 사용하는 것이 권장된다.

### 💡 변수 유효 범위

`var`는 함수 스코프를 가지지만, `let`과 `const`는 블록 스코프를 가져 변수의 유효 범위를 제한하여 코드의 예측 가능성을 높인다.

### 💡 재선언 허용

`var`는 동일한 변수를 여러 번 선언할 수 있어 실수를 유발할 수 있다. 반면, `let`과 `const`는 동일한 변수를 재선언할 수 없다.

### 💡 TDZ(Temporal Dead Zone)

`let`과 `const`는 **TDZ**의 영향을 받아 초기화 전에는 접근할 수 없기 때문에, 초기화되지 않은 변수에 접근하는 오류를 방지할 수 있다.

---

# 호이스팅 기본 개념

호이스팅은 자바스크립트 엔진이 코드를 실행하기 전에 변수와 함수 선언을 해당 범위의 최상단으로 끌어올리는 동작을 말한다. 이를 통해 변수와 함수를 선언하기 전에 참조할 수 있게 된다. 이는 자바스크립트의 실행 컨텍스트와 스코프 체인에 기인한 동작이다.

---

## 📌 호이스팅 예시

```js
console.log(x); // undefined
var x = 5;
console.log(x); // 5
```

위 코드에서 `x`는 선언되기 전에 `console.log`로 참조되지만, 에러가 발생하지 않고 `undefined`가 출력된다. 이는 `var x` 선언이 호이스팅되어 코드의 최상단으로 끌어올려졌기 때문이다.

---

## 📌 왜 호이스팅이 발생하는가?

호이스팅이 발생하는 이유는 자바스크립트의 실행 컨텍스트와 관련이 있다. 자바스크립트 코드를 실행할 때 엔진은 두 가지 단계를 거친다

- **생성 단계 (Creation Phase)** : 실행 컨텍스트가 생성되며, 변수와 함수 선언이 메모리에 등록된다. 이 단계에서 변수는 undefined로 초기화되고, 함수는 전체 함수 정의로 초기화된다.
- **실행 단계 (Execution Phase)** : 실제 코드가 한 줄씩 실행되며, 변수와 함수에 값이 할당된다.

**💡 이러한 두 단계로 인해 변수와 함수 선언이 코드의 최상단으로 끌어올려진 것처럼 동작하게 된다.**

---

### 💡 **실행 컨텍스트란?**

실행 컨텍스트(Execution Context)는 자바스크립트 코드가 실행되는 환경을 의미한다. 각 실행 컨텍스트는 코드가 실행될 때 생성되며, 변수, 함수 선언, this 값 등을 포함한다. 실행 컨텍스트에는 전역 컨텍스트, 함수 컨텍스트, eval 컨텍스트가 있다.

### 💡 **스코프 체인이란?**

스코프 체인(Scope Chain)은 코드의 스코프(변수의 유효 범위)를 계층적으로 연결한 것이다. 실행 컨텍스트가 생성되면 스코프 체인이 함께 생성되며, 각 컨텍스트는 자신만의 스코프와 상위 스코프에 대한 참조를 가진다. 이를 통해 자바스크립트는 변수를 참조할 때 현재 스코프에서 시작하여 상위 스코프를 따라 올라가며 변수를 찾는다.

```js
function outer() {
  var x = 10;
  function inner() {
    var y = 20;
    console.log(x + y); // 30 (inner 함수의 스코프 체인에서 outer 함수의 스코프까지 접근)
  }
  inner();
}
outer();
```

위 코드에서 `inner` 함수는 자신이 속한 `outer` 함수의 스코프 체인을 따라 `x` 변수를 참조할 수 있다.

---

# 변수 호이스팅

자바스크립트에서 변수는 `var`, `let`, `const` 키워드를 사용해 선언된다. 이들 키워드에 따른 호이스팅 동작을 알아보자.

## 📌 `var` 키워드

`var`로 선언된 변수는 선언이 호이스팅되지만, 초기화는 호이스팅되지 않는다. 따라서 변수 선언 전에 해당 변수를 참조하면 `undefined`가 반환된다.

```js
console.log(a); // undefined
var a = 10;
console.log(a); // 10
```

위 코드에서 var a는 최상단으로 호이스팅되지만, a = 10 초기화는 호이스팅되지 않는다.

---

## 📌 `let` 키워드

`let`으로 선언된 변수는 블록 스코프를 가지며, 선언은 호이스팅되지만 초기화 전에는 참조할 수 없다. 이 경우 `ReferenceError`가 발생한다. 이러한 동작을 **일시적 사각지대(TDZ, Temporal Dead Zone)**라고 한다. **TDZ**는 변수가 선언된 이후 초기화되기 전까지의 구간을 의미한다.

```js
console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 20;
console.log(b); // 20
```

위 코드에서 `b`는 블록 스코프 내에서 선언되었지만, 초기화되기 전까지 **TDZ**에 놓여있어 초기화 전에 참조하면 `ReferenceError`가 발생한다.

---

## 📌 `const` 키워드

`const`로 선언된 변수는 블록 스코프를 가지며, 선언과 동시에 초기화되어야 한다. 선언은 호이스팅되지만 초기화 전에는 참조할 수 없다. 따라서 `const`로 선언된 변수도 **TDZ**의 영향을 받는다.

```js
console.log(c); // ReferenceError: Cannot access 'c' before initialization
const c = 30;
console.log(c); // 30
```

---

## 📌 TDZ란 무엇인가?

**TDZ(Temporal Dead Zone)**는 변수 선언이 코드의 최상단으로 호이스팅되지만, 실제로 초기화되기 전까지 변수를 사용할 수 없는 구간을 의미한다. `let`과 `const` 키워드로 선언된 변수는 **TDZ**가 적용되어, 변수 선언 전에 해당 변수를 참조하면 `ReferenceError`가 발생한다. 이는 변수가 선언되었지만 초기화되지 않은 상태이기 때문이다.

---

# 함수 호이스팅

자바스크립트에서 함수는 **함수 선언식(Function Declaration)**과 **함수 표현식(Function Expression)**으로 선언될 수 있다.

---

## 📌 함수 선언식

함수 선언식으로 정의된 함수는 선언과 동시에 호이스팅된다. 따라서 함수 선언 전에 호출할 수 있다.

```js
greet(); // "Hello!"

function greet() {
  console.log("Hello!");
}
```

위 코드에서 `greet` 함수는 선언되기 전에 호출될 수 있다. 이는 함수 선언이 호이스팅되었기 때문이다.

---

## 📌 함수 표현식

함수 표현식으로 정의된 함수는 변수에 할당되며, 변수 호이스팅 규칙을 따른다. 따라서 변수 선언 전에 호출할 수 없다.

### 💡 `var`

```js
sayHello(); // TypeError: sayHello is not a function

var sayHello = function () {
  console.log("Hello!");
};
```

위 코드에서 `sayHello`는 변수 선언이 호이스팅되지만, 함수 할당은 호이스팅되지 않기 때문에 선언 전에 호출할 수 없다.

### 💡 `const`

함수 표현식에서 `var` 대신 `const`를 사용하면 호이스팅 동작이 달라진다. `const`로 선언된 변수는 **TDZ**의 영향을 받아 초기화 전에는 접근할 수 없기 때문에, 초기화 전에 해당 변수를 참조하면 `ReferenceError`가 발생한다.

```js
sayHi(); // ReferenceError: Cannot access 'sayHi' before initialization

const sayHi = function () {
  console.log("Hi!");
};
```

위 코드에서 `sayHi`는 `const`로 선언되어 **TDZ**의 영향을 받기 때문에, 초기화 전에 접근하면 `ReferenceError`가 발생한다.

### 💡 함수 표현식에서 const를 사용하는 이유

함수 표현식에서는 일반적으로 `const`를 사용한다. 이는 다음과 같은 이유 때문이다.

- **변경 불가능성:** `const`로 선언된 변수는 재할당이 불가능하여, 함수가 정의된 이후에는 변경되지 않음을 보장한다.
- **블록 스코프:** `const`는 블록 스코프를 가지므로, 함수가 블록 내부에서만 유효하게 되어 코드의 안정성을 높인다.
- **TDZ(Temporal Dead Zone):** `const`는 **TDZ**의 영향을 받아 초기화 전에는 접근할 수 없기 때문에, 초기화되지 않은 변수에 접근하는 오류를 방지할 수 있다.

---

# 호이스팅의 이점과 주의점

## 📌 이점

### 💡 가독성 향상

함수 선언을 최상단에 배치함으로써 코드의 구조를 명확하게 하고 가독성을 높일 수 있다. 이는 함수가 호출되기 전에 정의되어 있어야 한다는 제약 없이, 코드의 논리적 흐름을 더 쉽게 파악할 수 있게 해준다.

```js
function processArray(arr) {
  // 함수 선언을 최상단에 배치하여 코드의 흐름을 명확하게 함
  function double(num) {
    return num * 2;
  }

  return arr.map(double);
}

const numbers = [1, 2, 3, 4];
console.log(processArray(numbers)); // [2, 4, 6, 8]
```

### 💡 유연성 증가

변수와 함수 선언을 코드 어디서든 사용할 수 있어 유연한 코딩이 가능하다. 이는 코드 작성 시 함수를 나중에 정의하더라도 미리 호출할 수 있는 유연성을 제공한다.

```js
console.log(greet()); // "Hello!"

function greet() {
  return "Hello!";
}
```

---

## 📌 주의점

### 💡 예측 불가능성

호이스팅으로 인해 변수나 함수가 예상치 못한 시점에 참조될 수 있어 디버깅이 어려울 수 있다. 특히, `var` 키워드로 선언된 변수는 선언 전에 참조할 경우 `undefined`가 반환되어 의도하지 않은 동작을 초래할 수 있다.

```js
console.log(name); // undefined
var name = "Alice";
console.log(name); // "Alice"
```

### 💡 TDZ

`let`과 `const` 키워드의 **TDZ**로 인해 변수가 선언되었지만 초기화되지 않은 상태에서 참조할 경우 `ReferenceError`가 발생할 수 있다. 이는 변수가 선언된 시점과 초기화된 시점 사이의 구간에서 발생하는 오류를 방지하기 위한 메커니즘이다.

```js
console.log(tempVar); // ReferenceError: Cannot access 'tempVar' before initialization
let tempVar = 10;
console.log(tempVar); // 10
```

### 💡 변수 유효 범위 관리의 어려움

`var`는 함수 스코프를 가지기 때문에, 블록 레벨 스코프를 지원하는 `let`이나 `const`와 다르게 동작할 수 있다. 이는 변수의 유효 범위를 관리하는 데 혼란을 초래할 수 있다.

```js
if (true) {
  var x = 5;
}
console.log(x); // 5

if (true) {
  let y = 10;
}
console.log(y); // ReferenceError: y is not defined
```

### 💡 변수 재선언 허용

`var`는 동일한 변수를 여러 번 선언할 수 있어 실수를 유발할 수 있다. 반면, `let`과 `const`는 동일한 변수를 재선언할 수 없어 이러한 오류를 방지할 수 있다.

```js
var a = 1;
var a = 2; // 재선언 가능
console.log(a); // 2

let b = 1;
let b = 2; // SyntaxError: Identifier 'b' has already been declared
```

---

# 결론

호이스팅은 자바스크립트의 중요한 특징 중 하나로, 변수와 함수 선언을 코드의 최상단으로 끌어올리는 동작을 의미한다. `var`, `let`, `const` 키워드와 함수 선언식, 함수 표현식에서 호이스팅 동작을 이해하고, 이를 적절히 관리함으로써 코드의 예측 가능성과 가독성을 높일 수 있다.

특히, `var`는 **함수 스코프**, **재선언 허용**, **TDZ** 등의 이유로 인해 사용되지 않는 추세이며, 대신 `let`과 `const`를 사용하는 것이 권장된다.

---

# 참고문헌

> [MDN_Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
