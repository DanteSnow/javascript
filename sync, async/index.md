# 동기(Sync)와 비동기(Async)

자바스크립트에서 동기와 비동기는 코드가 실행되는 방식에 차이가 있다.

---

## 동기(Synchronous)

![](https://velog.velcdn.com/images/clydehan/post/8b5d30ba-0440-4b85-8fca-a5b124d06eb8/image.png)

동기 처리는 코드가 작성된 순서대로 하나씩 실행된다. 즉, 한 작업이 완료될 때까지 다음 작업이 시작되지 않는다. 이는 작업이 차례로 실행되므로 예측 가능하고 디버깅이 쉽다. 그러나 시간이 오래 걸리는 작업이 있을 경우 전체 프로그램이 대기 상태에 빠질 수 있다.

### 예시 코드

```js
console.log("작업 1 시작");
console.log("작업 2 시작");
console.log("작업 3 시작");
```

### 설명

위의 코드는 순차적으로 실행되어 "작업 1 시작", "작업 2 시작", "작업 3 시작"이 차례로 출력된다.

---

## 비동기(Asynchronous)

![](https://velog.velcdn.com/images/clydehan/post/cd602422-8a5f-4321-beb1-0fa133e359f4/image.png)

비동기 처리는 특정 작업이 완료될 때까지 기다리지 않고 다음 작업을 바로 시작할 수 있다. 이는 작업이 병렬로 실행될 수 있으며, 특히 시간이 오래 걸리는 작업을 수행할 때 유용하다.

### 예시 코드

```js
console.log("작업 1 시작");
setTimeout(() => {
  console.log("작업 2 완료");
}, 2000);
console.log("작업 3 시작");
```

### 설명

위의 코드는 "작업 1 시작"과 "작업 3 시작"이 먼저 출력되고, 2초 후에 "작업 2 완료"가 출력된다. `setTimeout`은 비동기 함수로, 2초 후에 콜백 함수를 실행한다.

---

# Sync와 Async의 차이

## 실행 방식

동기는 순차적으로 실행되고, 비동기는 동시에 실행될 수 있다. 이는 코드의 흐름과 실행 순서에 큰 차이를 만든다.

---

## 블로킹 여부

동기는 블로킹 방식이고, 비동기는 논블로킹 방식이다. 이는 작업 중에 다른 작업을 수행할 수 있는지 여부를 결정한다.

---

### 블로킹(Blocking)

블로킹 방식은 하나의 작업이 완료될 때까지 모든 다른 작업이 중지되는 방식이다. 예를 들어, 긴 연산을 수행하는 동안 브라우저는 사용자의 다른 입력을 처리하지 못하게 된다. 이는 프로그램의 실행이 멈추는 것처럼 보일 수 있으며, 특히 시간이 많이 걸리는 작업이 실행될 때 눈에 띈다.

#### **특징**

- 하나의 작업이 완료될 때까지 다른 작업을 수행하지 못함.
- 코드의 실행 순서가 예측 가능함.
- 사용자 경험이 저하될 수 있음.

---

### 논블로킹(Non-blocking)

논블로킹 방식은 하나의 작업이 진행되는 동안 다른 작업을 계속 수행할 수 있는 방식이다. 이는 프로그램이 중지되지 않고, 동시에 여러 작업을 처리할 수 있게 한다. 예를 들어, 네트워크 요청을 하는 동안 사용자 인터페이스는 여전히 반응할 수 있다. 이는 사용자가 애플리케이션을 끊김 없이 사용할 수 있도록 도와준다.

#### **특징**

- 하나의 작업이 완료되지 않아도 다른 작업을 수행할 수 있음.
- 코드의 실행 순서가 비동기적으로 이루어짐.
- 사용자 경험이 개선될 수 있음.

---

## 사용 사례

동기는 단순한 연산이나 즉시 실행이 필요한 작업 등 간단하고 짧은 작업에 적합하며, 비동기는 네트워크 요청, 파일 읽기/쓰기 등 시간이 오래 걸리는 작업이나 사용자 경험을 최적화해야 하는 상황에서 사용된다.

---

# Sync와 Async의 사용 사례와 이유

## 동기(Synchronous)

### 단순한 연산

숫자의 합계 계산, 문자열 처리 등 단순한 연산에서 사용된다. 단순한 연산은 시간이 거의 걸리지 않기 때문에 블로킹이 문제가 되지 않는다. 코드가 순차적으로 실행되어도 사용자 경험에 큰 영향을 미치지 않는다.

### 순서대로 처리해야 하는 작업

작업이 순서대로 실행되어야 할 때는 동기 방식을 사용하는 것이 더 직관적이고 안전하다. 예를 들어, 입력 데이터 검증이 있다.

> 💡 입력 데이터 검증
> 사용자의 입력을 받아서 형식을 확인하고, 그 다음에 필수 필드를 확인하고, 마지막으로 데이터의 유효성을 확인한다. 이 과정은 순차적으로 이루어져야 하기 때문에 동기적 처리가 적합하다.

---

## 비동기(Asynchronous)

### 네트워크 요청

서버로부터 데이터 가져오기, API 호출 등의 작업에 비동기가 사용된다. 네트워크 요청은 시간이 오래 걸릴 수 있다. 비동기 방식을 사용하면 데이터를 가져오는 동안 다른 작업을 계속할 수 있어 사용자 경험이 개선된다.

### 파일 읽기/쓰기

파일 입출력은 시간이 오래 걸릴 수 있는 작업이다. 비동기 방식을 사용하면 파일을 읽거나 쓰는 동안 다른 작업을 계속할 수 있다.

### 타이머 및 지연 작업

일정 시간 후 작업 실행(예: 애니메이션, 주기적 업데이트) 등의 작업에 비동기가 사용된다. 타이머를 사용한 작업은 주어진 시간이 지나야 실행된다. 이 동안 다른 작업을 수행할 수 있게 비동기 방식을 사용한다.

### 사용자 인터페이스(UI) 반응성 유지

브라우저에서 사용자 이벤트 처리(예: 클릭, 키보드 입력) 등의 작업에도 비동기가 사용된다. 비동기 방식을 사용하면 사용자가 인터페이스와 상호작용할 때 즉각적인 반응을 제공할 수 있다. 동기 작업이 길어지면 UI가 멈추거나 응답하지 않는 현상이 발생할 수 있다.

---

# 비동기를 동기로 작동시키는 방법

자바스크립트에서는 비동기 코드를 동기처럼 작동시키는 방법이 몇 가지 있다. 이러한 방법들은 주로 개발자가 비동기 작업의 결과를 처리하기 쉽게 만들어준다.

---

## 콜백 함수 (Callback Function)

콜백 함수는 특정 작업이 완료된 후 호출되는 함수이다. 비동기 작업의 결과를 처리하기 위해 널리 사용된다. 콜백 함수를 사용하면 비동기 작업이 완료되었을 때, 그 결과를 다른 함수로 전달할 수 있다. 예를 들어, 데이터를 서버에서 가져오는 작업이 있을 때, 이 작업이 완료되면 콜백 함수가 호출되어 결과를 처리할 수 있다.

### 예시 코드

비동기 작업을 모방하는 함수

```js
function fetchData(callback) {
  // 2초 후에 데이터를 가져오는 작업
  setTimeout(() => {
    const data = "데이터 가져오기 완료";
    // 비동기 작업이 완료되면 콜백 함수 호출
    callback(data);
  }, 2000);
}

// 비동기 작업 시작
console.log("데이터 요청 시작");

// 데이터를 가져온 후 처리하는 콜백 함수
fetchData((data) => {
  console.log(data); // "데이터 가져오기 완료"
  console.log("데이터 요청 완료");
});
```

#### 예시 코드 설명

- fetchData 함수는 인자로 콜백 함수를 받는다.
- setTimeout 함수는 2초 후에 callback을 호출하며, 이때 데이터를 전달힌다.
- fetchData 함수를 호출할 때, 실제로 데이터를 처리하는 콜백 함수를 전달힌다.
- 2초 후에 callback 함수가 호출되고, 데이터를 처리한다.

### 장점

- 간단하고 직관적이다.

### 단점

- "콜백 지옥"(Callback Hell) 문제: 중첩된 콜백 함수로 인해 코드 가독성이 떨어지고, 유지보수가 어려워진다.

콜백 지옥 예시

```Js
function fetchData1(callback) {
  setTimeout(() => {
    callback("데이터 1");
  }, 1000);
}

function fetchData2(callback) {
  setTimeout(() => {
    callback("데이터 2");
  }, 1000);
}

function fetchData3(callback) {
  setTimeout(() => {
    callback("데이터 3");
  }, 1000);
}

fetchData1((data1) => {
  console.log(data1);
  fetchData2((data2) => {
    console.log(data2);
    fetchData3((data3) => {
      console.log(data3);
      console.log("모든 데이터 가져오기 완료");
    });
  });
});

```

---

## 프라미스 (Promise)

프라미스는 비동기 작업의 완료 또는 실패를 나타내는 객체이다. 프라미스를 사용하면 비동기 작업의 결과를 더 구조적으로 처리할 수 있다. 프라미스는 비동기 작업이 성공했을 때 resolve 함수를 호출하고, 실패했을 때 reject 함수를 호출한다. 프라미스는 then과 catch 메서드를 사용하여 비동기 작업의 결과를 처리할 수 있다.

### 코드 예시

프라미스를 반환하는 함수

```js
function fetchData() {
  // 새 프라미스를 생성
  return new Promise((resolve, reject) => {
    // 2초 후에 데이터를 가져오는 작업
    setTimeout(() => {
      const data = "데이터 가져오기 완료";
      // 비동기 작업이 성공했을 때 resolve 호출
      resolve(data);
    }, 2000);
  });
}

// 프라미스를 사용하여 비동기 작업 처리
console.log("데이터 요청 시작");

fetchData()
  .then((data) => {
    console.log(data); // "데이터 가져오기 완료"
    console.log("데이터 요청 완료");
  })
  .catch((error) => {
    console.error("에러 발생:", error);
  });
```

#### 예시 코드 설명

- fetchData 함수는 프라미스를 반환한다. 프라미스는 비동기 작업을 나타내는 객체이다.
- new Promise를 생성할 때, 두 개의 콜백 함수 resolve와 reject를 받는다. resolve는 작업이 성공했을 때 호출되고, reject는 작업이 실패했을 때 호출된다.
- setTimeout 함수는 2초 후에 resolve를 호출하여 데이터를 전달한다.
- fetchData 함수는 프라미스를 반환하므로, then 메서드를 사용하여 프라미스가 완료되었을 때의 작업을 정의할 수 있다.
- then 메서드는 프라미스가 성공했을 때 호출되며, catch 메서드는 프라미스가 실패했을 때 호출된다.

### 장점

- 콜백 함수보다 가독성이 좋고, 코드가 더 구조적이다.
- then과 catch 메서드를 체이닝하여 여러 비동기 작업을 순차적으로 처리할 수 있다.

### 단점

- 여전히 체이닝된 then 블록이 많아지면 가독성이 떨어질 수 있다.

### 프라미스 체이닝(Promise Chaining)

프라미스 체이닝은 하나의 프라미스가 완료된 후에 다른 프라미스를 반환하여 일련의 비동기 작업을 순차적으로 실행하는 기법이다. 이는 then 메서드를 사용하여 여러 프라미스를 연결하여 작성한다. 각 then 블록은 이전 프라미스가 완료된 후 실행되며, 새로운 프라미스를 반환할 수 있다.

---

## async/await

async/await는 ES2017(ES8)에서 도입된 문법으로, 비동기 코드를 동기 코드처럼 작성할 수 있게 해준다. async 함수는 항상 프라미스를 반환하며, await 키워드는 프라미스를 기다린다.

### 예시 코드

async/await

```Js
async function getData() {
  let response = await fetch('https://api.example.com/data');
  let data = await response.json();
  console.log(data);
}

getData();

```

### 장점

- 비동기 코드를 동기 코드처럼 작성할 수 있어 가독성이 크게 향상된다.
- 복잡한 비동기 흐름을 관리하기 쉬워진다.

---

## 비동기를 동기로 작동시키는 이유

비동기를 동기로 작동시키는 이유는 다양한 상황에서 코드의 가독성을 높이고, 복잡한 비동기 흐름을 관리하기 쉽게 하기 위함이다.

### 코드의 가독성 향상

비동기 코드가 복잡해지면, 특히 콜백 함수를 많이 사용하게 되면 "콜백 지옥"이라고 불리는 현상이 발생한다. 이는 코드의 가독성을 떨어뜨리고, 유지보수를 어렵게 만든다. 이럴 때 비동기 코드를 마치 동기 코드처럼 작성하며 가독성을 크게 향상시킬 수 있다.

콜백 지옥 코드 예시

```js
function getData(callback) {
  fetch("https://api.example.com/data", (response) => {
    response.json((data) => {
      callback(data);
    });
  });
}

getData((data) => {
  console.log(data);
});
```

async/await 사용

```Js
async function getData() {
  let response = await fetch('https://api.example.com/data');
  let data = await response.json();
  console.log(data);
}

getData();

```

### 비동기 흐름 제어

여러 비동기 작업을 순차적으로 실행하거나, 특정 조건에 따라 실행해야 할 때 비동기 흐름을 제어하는 것이 복잡해질 수 있다. 이럴 때 비동기 작업의 흐름을 쉽게 제어할 수 있다. 각 비동기 작업이 완료될 때까지 기다렸다가 다음 작업을 실행할 수 있다.

프라미스 체이닝

```js
fetch("https://api.example.com/data1")
  .then((response) => response.json())
  .then((data1) => {
    return fetch("https://api.example.com/data2");
  })
  .then((response) => response.json())
  .then((data2) => {
    console.log(data1, data2);
  })
  .catch((error) => {
    console.error(error);
  });
```

async/await 사용

```js
async function getData() {
  try {
    let response1 = await fetch("https://api.example.com/data1");
    let data1 = await response1.json();

    let response2 = await fetch("https://api.example.com/data2");
    let data2 = await response2.json();

    console.log(data1, data2);
  } catch (error) {
    console.error(error);
  }
}

getData();
```

### 에러 처리 단순화

비동기 작업에서 발생하는 에러를 처리하는 것이 복잡할 수 있다. 특히 여러 단계의 비동기 작업이 있는 경우, 각 단계에서 에러를 처리해야 한다. async/await와 try/catch 블록을 사용하면 비동기 작업의 에러 처리를 동기 코드처럼 간단하게 할 수 있다.

프라미스 체이닝

```js
fetch("https://api.example.com/data")
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

async/await 사용

```Js
async function getData() {
  try {
    let response = await fetch('https://api.example.com/data');
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Error:", error);
  }
}

getData();

```

---

# 참고 문헌

> [Link1](https://inpa.tistory.com/entry/%F0%9F%8C%90-js-async)
