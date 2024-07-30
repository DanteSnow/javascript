# [JavaScript] 자바스크립트 화살표 함수 (Arrow Function)

# 들어가며

![](https://velog.velcdn.com/images/clydehan/post/97b98cf4-541e-47a2-8bba-fc4fdb6f785e/image.png)

> 이미지 출처 : [Link](https://betterprogramming.pub/difference-between-regular-functions-and-arrow-functions-f65639aba256)

자바스크립트에서 함수를 작성하는 방법 중 하나인 애로우 함수(Arrow Function)에 대해 알아보자. 이 포스트에서는 애로우 함수의 기본 문법, 변환법, 장단점, 그리고 다양한 사용 예시를 통해 애로우 함수에 대해 다룬다.

---

# 애로우 함수 기본 문법

애로우 함수는 function 키워드 대신 => (화살표)를 사용하여 함수를 정의하는 새로운 방법이다. 애로우 함수의 기본 문법은 다음과 같다.

```Js
// 기본 문법
(param1, param2, ..., paramN) => { statements }

// 예시
const add = (a, b) => {
  return a + b;
};

```

## 단일 매개변수와 단일 문장의 경우

애로우 함수는 단일 매개변수를 받을 때 매개변수의 괄호를 생략할 수 있으며, 단일 문장을 반환할 때 중괄호와 return 키워드를 생략할 수 있다.

```js
// 매개변수가 하나인 경우 괄호 생략
const square = (x) => x * x;

// 문장이 하나인 경우 중괄호와 return 생략
const add = (a, b) => a + b;
```

---

# 애로우 함수 변환법

기존의 function 키워드로 작성된 함수를 애로우 함수로 변환하는 방법을 알아보자.

## 기본 함수 -> 애로우 함수

```js
// 기본 함수
function multiply(a, b) {
  return a * b;
}

// 애로우 함수
const multiply = (a, b) => a * b;
```

## 익명 함수 -> 애로우 함수

```js
// 익명 함수
const divide = function (a, b) {
  return a / b;
};

// 애로우 함수
const divide = (a, b) => a / b;
```

### 익명 함수란?

익명 함수(Anonymous Function)는 이름이 없는 함수로, 주로 일회성으로 사용되는 경우가 많다. 이러한 함수는 변수에 할당되거나 다른 함수의 인수로 전달될 수 있다.

```js
// 익명 함수를 변수에 할당
const greet = function () {
  console.log("Hello, World!");
};

greet(); // Hello, World!

// 익명 함수를 다른 함수의 인수로 전달
setTimeout(function () {
  console.log("Time out!");
}, 1000);
```

---

# 애로우 함수의 장점

## 1. 간결한 문법

애로우 함수는 코드의 가독성을 높이고, 함수를 간결하게 작성할 수 있게 한다.

```js
// 기존 함수
const numbers = [1, 2, 3, 4, 5];
const squares = numbers.map(function (n) {
  return n * n;
});

// 애로우 함수
const squares = numbers.map((n) => n * n);
```

## 2. 간편한 익명 함수

애로우 함수는 익명 함수를 간편하게 작성할 수 있다.

```js
// 기존 함수
const array = [1, 2, 3];
const doubled = array.map(function (n) {
  return n * 2;
});

// 애로우 함수
const doubled = array.map((n) => n * 2);
```

## 3. this 바인딩

애로우 함수는 자신만의 this 컨텍스트를 가지지 않고, 상위 스코프의 this를 그대로 사용한다. 이는 콜백 함수 내부에서 this를 사용할 때 유용하다.

```js
// 기존 함수
function Person() {
  this.age = 0;

  setInterval(function () {
    this.age++;
    console.log(this.age);
  }, 1000);
}

// 애로우 함수
function Person() {
  this.age = 0;

  setInterval(() => {
    this.age++;
    console.log(this.age);
  }, 1000);
}
```

### this 바인딩이란?

this는 자바스크립트에서 함수가 호출될 때 정해지는 특별한 객체로, 함수가 호출되는 맥락(context)에 따라 달라진다. this는 객체 메서드, 생성자 함수, 이벤트 핸들러 등에서 많이 사용된다. this가 가리키는 대상은 함수가 어떻게 호출되었는지에 따라 다르다.

#### 📌 일반적인 함수에서의 this

일반 함수에서는 this가 글로벌 객체(브라우저에서는 window, Node.js에서는 global)를 가리킨다. 하지만 엄격 모드('use strict'를 사용한 경우)에서는 undefined를 가리킨다.

```js
function showThis() {
  console.log(this);
}

showThis(); // window (엄격 모드에서는 undefined)
```

#### 📌 메서드에서의 this

객체의 메서드에서 this는 해당 메서드를 호출한 객체를 가리킨다.

```js
const obj = {
  name: "John",
  greet: function () {
    console.log(this.name);
  },
};

obj.greet(); // 'John'
```

#### 📌 생성자 함수에서의 this

생성자 함수에서 this는 새로 생성되는 객체를 가리킨다.

```js
function Person(name) {
  this.name = name;
}

const person = new Person("Alice");
console.log(person.name); // 'Alice'
```

---

# 애로우 함수의 단점

## 1. 고유한 this, arguments, super, new.target이 없음

애로우 함수는 고유한 this, arguments, super, new.target을 가지지 않으며, 이는 상위 스코프에서 상속된다.

```js
const obj = {
  value: 10,
  method: function () {
    console.log(this); // obj

    const innerFunction = () => {
      console.log(this); // obj
    };

    innerFunction();
  },
};

obj.method();
```

## 2. 생성자로 사용할 수 없음

애로우 함수는 생성자 함수로 사용할 수 없으며, new 키워드를 사용하면 에러가 발생한다.

```js
const Foo = () => {};
const obj = new Foo(); // TypeError: Foo is not a constructor
```

## 3. prototype 속성이 없음

애로우 함수는 prototype 속성을 가지지 않으므로, 프로토타입 메서드를 정의할 수 없다.

```js
const Foo = () => {};
console.log(Foo.prototype); // undefined
```

---

# 애로우 함수의 다양한 사용 예시

## 1. 배열 메서드와 함께 사용

애로우 함수는 배열 메서드(map, filter, reduce 등)와 함께 자주 사용된다.

```js
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4]
```

## 2. 이벤트 핸들러

애로우 함수는 이벤트 핸들러로 사용될 때 상위 스코프의 this를 유지할 수 있다.

```js
class MyClass {
  constructor() {
    this.value = 42;
    document.addEventListener("click", this.handleClick);
  }

  handleClick = () => {
    console.log(this.value); // 42
  };
}

const myClass = new MyClass();
```

## 3. 콜백 함수

애로우 함수는 콜백 함수로 사용할 때 간결한 문법을 제공한다.

```js
setTimeout(() => {
  console.log("Hello, World!");
}, 1000);
```

---

# 결론

애로우 함수는 자바스크립트에서 함수를 작성하는 간결하고 효율적인 방법을 제공한다. 고유한 this 바인딩 덕분에 콜백 함수나 이벤트 핸들러로 사용할 때 유용하며, 간단한 문법으로 가독성을 높인다. 하지만 생성자 함수나 프로토타입 메서드를 정의할 수 없다는 단점이 있다. 따라서 상황에 따라 적절한 함수 표현식을 선택하여 사용하는 것이 중요하다.

---

# 참고문헌

> [MDN_Arrow_Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
