# [JavaScript] 클로저와 렉시컬 환경 (Closure, Lexical Environment)

# 들어가며

![Closure In JavaScript](https://velog.velcdn.com/images/clydehan/post/1da3415c-4025-46f2-b76a-c723cd007e75/image.png)

> 이미지 출처: [calibraint blog](https://www.calibraint.com/blog/closures-in-javascript-for-beginners)

**클로저**의 원리를 이해하기 위해 다양한 예시와 개념을 함께 설명한다. 이 포스트에서는 **렉시컬 환경**, **클로저의 기본 개념**, **클로저가 사용되는 다양한 사례**, 그리고 **클로저가 코드에 미치는 영향**에 대해 다룬다.

---

# 렉시컬 환경(Lexical Environment)이란?

**렉시컬 환경**(어휘적 환경) 은 코드가 작성된 시점에서의 변수와 함수 선언을 포함하는 환경을 말한다. 자바스크립트 엔진은 코드를 실행할 때 어휘적 환경을 참조하여 변수와 함수의 유효 범위를 결정한다.

**렉시컬 환경** 은 실행 컨텍스트의 한 부분으로, 두 가지 구성 요소를 가진다.

- **환경 레코드(Environment Record)**: 현재 스코프의 변수와 함수 선언을 저장한다.
- **외부 환경 참조(Outer Environment Reference)**: 외부 스코프와의 참조를 가지고 있어, 상위 스코프의 변수와 함수에 접근할 수 있다.

렉시컬 환경은 코드가 작성된 위치를 기준으로 변수와 함수의 유효 범위가 정해진다는 것을 의미한다. 이를 **렉시컬 스코핑(Lexical Scoping)** 이라고 한다.

---

## 📌 환경 레코드(Environment Record)

환경 레코드는 현재 스코프에서 선언된 변수와 함수의 정보를 저장한다. 예를 들어, 함수 내부에서 변수를 선언하면 해당 변수는 환경 레코드에 저장된다.

```js
function foo() {
  let a = 10;
  function bar() {
    let b = 20;
    console.log(a + b); // 30
  }
  bar();
}
foo();
```

위 코드에서 `foo` 함수의 환경 레코드에는 `a` 변수가 저장되고, `bar` 함수의 환경 레코드에는 `b` 변수가 저장된다.

---

## 📌 외부 환경 참조(Outer Environment Reference)

외부 환경 참조는 현재 스코프의 상위 스코프를 가리킨다. 이를 통해 자바스크립트는 현재 스코프에서 변수를 찾지 못하면 상위 스코프로 이동하여 변수를 찾는다.

```js
function outer() {
  let x = 10;
  function inner() {
    console.log(x); // 10
  }
  inner();
}
outer();
```

위 코드에서 `inner` 함수는 외부 환경 참조를 통해 `outer` 함수의 `x` 변수에 접근할 수 있다.

---

## 📌 렉시컬 환경의 동작 방식

**렉시컬 환경**은 함수가 선언될 때 결정되며, 함수가 호출될 때마다 새로운 실행 컨텍스트가 생성되어 렉시컬 환경이 설정된다. 함수 호출 시 생성되는 렉시컬 환경은 해당 함수의 환경 레코드와 외부 환경 참조를 포함한다.

```js
function makeCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

const counter1 = makeCounter();
const counter2 = makeCounter();

console.log(counter1()); // 1
console.log(counter1()); // 2
console.log(counter2()); // 1
```

위 코드에서 `makeCounter` 함수는 호출될 때마다 새로운 렉시컬 환경을 생성하며, 각각의 `counter1`과 `counter2`는 독립적인 렉시컬 환경을 가진다. 따라서 `counter1`과 `counter2`는 서로 다른 `count` 값을 유지한다.

---

# 클로저(Closure)란?

자바스크립트에서 **클로저(Closure)** 는 함수와 함수가 선언된 **렉시컬 환경(Lexical Environment)** 과의 조합을 의미한다. 클로저는 다음과 같은 특성을 가진다.

- **함수가 선언된 스코프를 기억한다**: 함수가 선언될 때의 스코프를 기억하고, 그 스코프에 접근할 수 있다.
- **스코프 외부의 변수에 접근할 수 있다**: 함수가 외부 스코프의 변수에 접근할 수 있어, 변수의 값을 유지하거나 변경할 수 있다.

---

## 📌 실행 컨텍스트(Execution Context)

자바스크립트에서 코드가 실행될 때, 실행 컨텍스트가 생성된다. 실행 컨텍스트는 다음과 같은 구성 요소를 가진다.

- **변수 환경(Variable Environment)**: 모든 지역 변수를 포함하며, 함수 선언은 해당 변수 환경에 저장된다.
- ❗️ **렉시컬 환경(Lexical Environment)**: 현재 실행 중인 코드의 환경으로, 상위 스코프에 대한 참조를 포함한다.
- **this 바인딩(this Binding)**: this 키워드가 참조하는 객체를 결정한다.

```js
function outer() {
  let outerVar = "I am outside!";

  function inner() {
    console.log(outerVar); // 'I am outside!'
  }

  return inner;
}

const innerFunc = outer();
innerFunc(); // 'I am outside!'
```

위 코드에서 `inner` 함수는 `outer` 함수의 **렉시컬 환경**을 기억하며, 이를 통해 `outerVar`에 접근할 수 있다. 이처럼 함수가 자신의 스코프 외부에 있는 변수에 접근할 수 있는 것을 **클로저**라고 한다.

---

## 📌 클로저의 사용 사례와 이점

클로저는 다양한 상황에서 유용하게 사용될 수 있다. 아래에서는 클로저의 핵심적인 사용 사례와 이점을 설명한다.

### 💡 데이터 은닉화

클로저는 데이터 은닉화를 위해 자주 사용된다. 외부에서 접근할 수 없는 변수를 생성하고, 특정 함수만이 그 변수에 접근할 수 있도록 한다. 이를 통해 코드의 안정성과 보안성을 높일 수 있다.

```js
function createCounter() {
  let count = 0;

  return {
    increment() {
      count++;
      console.log(count);
    },
    decrement() {
      count--;
      console.log(count);
    },
  };
}

const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.decrement(); // 1
```

위 코드에서 `count` 변수는 `createCounter` 함수 내부에서 선언되었기 때문에 외부에서 직접 접근할 수 없다. 대신 `increment`와 `decrement` 메서드를 통해서만 `count`를 조작할 수 있다. 이를 통해 데이터 은닉화가 가능해진다.

---

### 💡 반복문과 클로저

클로저는 반복문에서 자주 사용되는 패턴이다. 반복문에서 클로저를 사용하면 반복문 내에서 생성된 변수의 상태를 기억할 수 있다.

```js
for (let i = 1; i <= 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 1000);
}
// 1, 2, 3
```

위 코드에서 `let`을 사용하면 블록 스코프를 가지므로, 각 반복마다 새로운 `i` 변수가 생성되어 클로저가 각각의 `i` 값을 기억하게 된다. 따라서 1초 후에 출력되는 값은 1, 2, 3이 된다.

---

### 💡 상태 유지

클로저를 사용하면 함수 호출 간의 상태를 유지할 수 있다. 이는 상태가 필요한 함수형 프로그래밍에서 매우 유용하다. 클로저를 통해 함수가 호출될 때마다 상태를 유지할 수 있다.

```js
function createAdder(x) {
  return function (y) {
    return x + y;
  };
}

const add5 = createAdder(5);
console.log(add5(2)); // 7
console.log(add5(10)); // 15
```

위 코드에서 `createAdder` 함수는 `x` 값을 기억하는 클로저를 반환하며, 이를 통해 함수 호출 간의 상태를 유지할 수 있다.

---

### 💡 모듈화

클로저를 사용하면 모듈 패턴을 구현할 수 있다. 이는 자바스크립트 코드의 모듈화를 통해 코드의 가독성과 유지보수성을 높일 수 있다. 모듈 패턴을 사용하면 코드를 분리하고 독립적으로 관리할 수 있다.

```js
const CounterModule = (function () {
  let count = 0;

  return {
    increment() {
      count++;
      console.log(count);
    },
    decrement() {
      count--;
      console.log(count);
    },
  };
})();

CounterModule.increment(); // 1
CounterModule.increment(); // 2
CounterModule.decrement(); // 1
```

위 코드에서 `CounterModule`은 즉시 실행 함수 표현식(IIFE)을 사용하여 모듈을 생성하고, 클로저를 통해 `count` 변수를 은닉하여 모듈 패턴을 구현한다.

### 📖 즉시 실행 함수 표현식(IIFE)

즉시 실행 함수 표현식(IIFE: Immediately Invoked Function Expression) 은 정의되자마자 즉시 실행되는 함수 표현식을 의미한다. IIFE는 함수를 정의함과 동시에 호출하기 위해 사용된다. IIFE를 사용하면 코드의 변수와 함수가 외부 스코프에 영향을 주지 않고 독립된 유효 범위를 가지게 할 수 있다.

**즉시 실행 함수 표현식의 기본 형태**

```js
(function () {
  // 내부 로직
})();
```

또는

```js
(function () {
  // 내부 로직
})();
```

---

## 📌 클로저의 주의점

클로저를 사용할 때는 성능 저하 등의 문제가 발생할 수 있다. 대표적인 예는 아래와 같다.

### 💡 메모리 누수

클로저는 참조를 유지하는 변수들이 계속해서 메모리에 남아있기 때문에, 잘못 사용하면 메모리 누수가 발생할 수 있다. 이는 특히 장시간 실행되는 애플리케이션에서 문제를 일으킬 수 있다.

```js
function createFunctionArray() {
  let arr = [];

  for (let i = 0; i < 1000000; i++) {
    arr.push(function () {
      console.log(i);
    });
  }

  return arr;
}

const functionArray = createFunctionArray();
```

위 코드에서 생성된 함수 배열은 많은 메모리를 차지할 수 있으며, 클로저가 변수에 대한 참조를 유지하기 때문에 메모리 누수가 발생할 수 있다.

---

# 고급 패턴

클로저와 함께 사용하는 고급 패턴은 여러 가지가 있다. 이번에는 해당 부분을 자세히 다루지는 않지만, 다음과 같은 고급 패턴이 있다는 점을 짚고 넘어간다.

- 커링(Currying): 하나의 함수가 여러 개의 인수를 받는 대신, 인수를 하나씩 받는 함수들의 연속으로 쪼개지는 패턴이다.
- 부분 적용 함수(Partial Application): 함수의 일부 인수를 고정하여 새로운 함수를 생성하는 패턴이다.

---

# 결론

클로저는 자바스크립트의 강력한 기능 중 하나로, 함수와 함수가 선언된 렉시컬 환경의 조합을 의미한다. 이를 이해하기 위해서는 함수가 선언된 스코프와 해당 스코프에서 선언된 변수에 접근할 수 있는 메커니즘을 이해하는 것이 중요하다.

렉시컬 환경과 실행 컨텍스트의 개념을 깊이 이해하면 클로저의 동작 방식을 더 명확히 파악할 수 있다. 이는 자바스크립트의 핵심 개념 중 하나이며, 이를 이해하면 더 나은 코드 작성과 디버깅에 큰 도움이 된다.

---

# 참고 문헌

> - [MDN_Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

- [MDN_Lexical Environment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#lexical_scoping)
- [You Don't Know JS: Scope & Closures](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/scope-closures)
