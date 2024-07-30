# [JavaScript] Blocking, Non-Blocking

> 이미지 출처 : [www.geeksforgeeks.org](https://www.geeksforgeeks.org/how-node-js-overcome-the-problem-of-blocking-of-i-o-operations/)

# 들어가며

이번 포스트에서는 자바스크립트의 블로킹과 논블로킹을 이해하기 위해 필요한 사전 지식들과 블로킹과 논블로킹에 대하여 다룬다.

---

# 자바스크립트의 싱글 스레드 특성과 비동기 프로그래밍

![자바스크립트 동기와 비동기의 차이 그래프] (https://velog.velcdn.com/images/clydehan/post/396fd283-ed40-4203-8966-14dd08dc01e6/image.png)

> 이미지 출처 : [medium.com/@vivianyim](https://medium.com/@vivianyim/synchronous-vs-asynchronous-javascript-de4918e8ad62)

자바스크립트는 싱글 스레드 언어다. 이는 자바스크립트 엔진이 한 번에 하나의 작업만 처리할 수 있음을 의미한다. 즉, 여러 작업이 동시에 실행되지 않고, 작업들이 순차적으로 처리된다. 이러한 특성은 몇 가지 문제를 일으킬 수 있다.

---

## 싱글 스레드의 문제점

![자바스크립트 싱글 스레드 참고 이미지](https://velog.velcdn.com/images/clydehan/post/d99f46b9-3305-49c3-ab4c-bf9d4764c431/image.png)

> 이미지 출처 : [dev.to/arealesramirez](https://dev.to/arealesramirez/is-node-js-single-threaded-or-multi-threaded-and-why-ab1)

### 작업 지연

긴 작업이 실행되면, 다른 작업이 대기 상태에 놓이게 된다. 예를 들어, 대용량 데이터를 처리하거나 파일을 읽는 작업이 수행될 때, 사용자 입력과 같은 다른 작업들이 지연될 수 있다.

### 응답성 저하

사용자 인터페이스(UI)와 상호작용하는 웹 애플리케이션에서는, 긴 작업으로 인해 UI가 멈추거나 응답하지 않는 상황이 발생할 수 있다. 이는 사용자 경험에 부정적인 영향을 미친다.

### 병목 현상

모든 작업이 순차적으로 처리되므로, 특정 작업이 완료될 때까지 다른 작업이 시작되지 못하는 병목 현상이 발생할 수 있다.

---

## 싱글 스레드 언어의 예시

```js
console.log("작업 1 시작");
for (let i = 0; i < 1000000000; i++) {} // 오래 걸리는 작업
console.log("작업 1 완료");

console.log("작업 2 시작");
for (let i = 0; i < 1000000000; i++) {} // 또 다른 오래 걸리는 작업
console.log("작업 2 완료");
```

위 코드는 두 개의 작업을 순차적으로 처리한다. '작업 1 시작' 메시지가 출력된 후, 첫 번째 `for` 루프가 완료될 때까지 기다리고, 완료되면 '작업 1 완료' 메시지를 출력한다. 그 후 '작업 2 시작' 메시지가 출력되고, 두 번째 for 루프가 완료될 때까지 기다린다. 이렇게 모든 작업이 순차적으로 처리된다.

---

## 비동기 프로그래밍과 이벤트 루프

![자바스크립트 동기 비동기 차이 그래프 참고 이미지](https://velog.velcdn.com/images/clydehan/post/9ff53528-2316-4635-836b-85bcad11d8af/image.png)

> 이미지 출처 : [scoutapm.com](https://scoutapm.com/blog/async-javascript)

자바스크립트는 싱글 스레드의 한계를 극복하기 위해 비동기 프로그래밍과 이벤트 루프를 사용한다.

비동기 프로그래밍과 이벤트 루프는 밀접하게 연관되어 있지만, 동일한 개념은 아니다. 비동기 프로그래밍은 특정 작업이 완료될 때까지 기다리지 않고 즉시 다음 코드를 실행할 수 있게 하여 다른 작업을 동시에 처리할 수 있게 하는 방법론이다. 이벤트 루프는 이러한 비동기 작업을 관리하고, 콜백 함수가 적절한 시점에 실행되도록 하는 메커니즘이다.

비동기 프로그래밍은 비동기 함수(예: `setTimeout`, `fetch`, `Promise`)를 사용하여 구현된다. 이벤트 루프는 이러한 비동기 함수들이 콜백 큐에 추가된 콜백 함수들을 콜 스택이 비어 있을 때 실행함으로써 비동기 프로그래밍이 원활하게 작동하도록 돕는다.

> 💡 **콜백 함수(Callback Function)**
> 콜백 함수는 다른 함수의 인자로 전달되어, 특정 작업이 완료된 후 호출되는 함수다. 주로 비동기 작업에서 사용되어, 작업이 완료된 후 후속 작업을 처리하는 데 유용하다.

---

### 비동기 프로그래밍

비동기 프로그래밍은 작업이 완료되기 전에 제어를 반환하여 다른 작업을 동시에 처리할 수 있게 하는 방법론이다. 비동기 프로그래밍은 네트워크 요청, 파일 읽기/쓰기, 타이머 등 긴 작업을 블로킹 없이 처리할 수 있게 해준다.

### 비동기 코드 예시

```js
console.log("데이터 요청 중...");
fetch("https://example.com/")
  .then((response) => response.json())
  .then((data) => {
    console.log("데이터 수신 완료:", data);
  });
console.log("다른 작업 수행 중...");
```

위 코드는 fetch API를 사용하여 비동기적으로 데이터를 요청하는 예시다. '데이터 요청 중...' 메시지가 출력된 후, 데이터 요청이 비동기적으로 처리되는 동안 '다른 작업 수행 중...' 메시지가 즉시 출력된다. 데이터 요청이 완료되면 콜백 함수가 실행되어 '데이터 수신 완료' 메시지가 출력된다.

---

### 이벤트 루프(Event Loop)

![이벤트 루프 참고 이미지](https://velog.velcdn.com/images/clydehan/post/6e53c167-6b57-4ebe-a9ef-be0cadd68f97/image.png)

> 이미지 출처 : [macarthur.me](https://macarthur.me/posts/navigating-the-event-loop/)

이벤트 루프는 콜 스택과 태스크 큐를 관리하며, 콜 스택이 비어 있는지 확인한다. 콜 스택이 비어 있으면, 태스크 큐에 있는 콜백 함수를 콜 스택으로 이동시켜 실행한다. 이를 통해 비동기 작업의 콜백 함수가 적절한 시점에 실행되도록 보장한다.

> 💡 **콜 스택(Call Stack)**
> 콜 스택은 현재 실행 중인 함수가 저장되는 구조다. 함수가 호출되면 콜 스택에 추가되고, 함수가 반환되면 콜 스택에서 제거된다. 싱글 스레드인 자바스크립트는 이 콜 스택을 통해 작업을 순차적으로 처리한다.

> 💡 **태스크 큐(Task Queue)**
> 태스크 큐는 비동기 작업의 콜백 함수가 대기하는 큐다. 비동기 작업이 완료되면 해당 작업의 콜백 함수가 태스크 큐에 추가된다. 태스크 큐에 쌓인 콜백 함수는 이벤트 루프에 의해 콜 스택이 비어 있을 때 실행된다.

### 이벤트 루프의 작동 방식 예시

```js
console.log("작업 1 시작");
setTimeout(() => {
  console.log("작업 1 완료");
}, 1000); // 1초 후에 작업 1 완료

console.log("작업 2 시작");
setTimeout(() => {
  console.log("작업 2 완료");
}, 500); // 0.5초 후에 작업 2 완료

console.log("작업 3 시작");
```

위 코드는 비동기 작업을 통해 동시에 여러 작업을 처리하는 예시다. '작업 1 시작' 메시지가 출력된 후, `setTimeout`을 사용해 1초 후에 '작업 1 완료' 메시지를 출력하도록 예약한다. 이 작업이 대기 상태로 들어간 후 '작업 2 시작' 메시지가 즉시 출력되고, `setTimeout`을 사용해 0.5초 후에 '작업 2 완료' 메시지를 출력하도록 예약한다. '작업 3 시작' 메시지도 즉시 출력된다.

- **작업 1 시작**
  '작업 1 시작' 메시지가 콜 스택에 쌓이고, 출력된 후 콜 스택에서 제거된다.
  setTimeout (작업 1): 1초 후에 실행될 콜백 함수가 태스크 큐에 추가된다.

- **작업 2 시작**
  '작업 2 시작' 메시지가 콜 스택에 쌓이고, 출력된 후 콜 스택에서 제거된다.
  setTimeout (작업 2): 0.5초 후에 실행될 콜백 함수가 태스크 큐에 추가된다.

- **작업 3 시작**
  '작업 3 시작' 메시지가 콜 스택에 쌓이고, 출력된 후 콜 스택에서 제거된다.

  0.5초 후, 태스크 큐에 대기 중인 '작업 2 완료' 콜백 함수가 콜 스택에 추가되어 실행된다. 그 후 1초가 지나면 '작업 1 완료' 콜백 함수가 태스크 큐에서 콜 스택으로 이동되어 실행된다.

---

# Blocking과 Non-Blocking

![블로킹과 논블로킹 차이 참고 이미지](https://velog.velcdn.com/images/clydehan/post/f4e5ecd1-0d7e-4ced-9372-cf052da87607/image.png)

> 이미지 출처 : [www.geeksforgeeks.org](https://www.geeksforgeeks.org/how-node-js-overcome-the-problem-of-blocking-of-i-o-operations/)

자바스크립트의 동작 방식을 이해하기 위해 Blocking과 Non-Blocking의 개념을 아는 것이 중요하다. 이 두 개념은 코드가 실행되는 방식을 결정하며, 효율적인 프로그램을 작성하는 데 큰 영향을 미친다.

자바스크립트는 기본적으로 동기(Synchronous)로 작동한다. 특정 함수나 명령어를 사용하지 않으면 대부분의 작업이 블로킹(Blocking)된다. 이를 피하기 위해서는 비동기(Asynchronous) 프로그래밍을 활용하여 논블로킹(Non-Blocking)이 되게 하는 것이 중요하다.

비동기 프로그래밍은 위에서 설명했다. 그렇다면 블로킹과 논블로킹은 무엇일까?

---

## Blocking

Blocking은 호출된 함수가 실행을 완료할 때까지 현재의 실행 흐름을 멈추게 한다. 이는 CPU가 다른 작업을 수행하지 못하고, 현재 작업이 완료될 때까지 대기하게 만든다. 블로킹 함수는 실행되는 동안 프로그램의 나머지 부분을 차단(block)하기 때문에 이러한 이름이 붙여졌다.

예를 들어, 긴 연산을 수행하는 동안 브라우저는 사용자의 다른 입력을 처리하지 못하게 된다. 이는 프로그램의 실행이 멈추는 것처럼 보일 수 있으며, 특히 시간이 많이 걸리는 작업이 실행될 때 눈에 띈다.

### Blocking 코드 예시

```js
function blockingTask() {
  let endTime = Date.now() + 3000; // 3초 동안 멈춤
  while (Date.now() < endTime) {
    // 아무것도 하지 않고 대기
  }
  console.log("Blocking task finished");
}

console.log("Start");
blockingTask();
console.log("End");
```

위 코드를 실행하면 "Blocking task finished"가 출력될 때까지 "End"가 출력되지 않는다. blockingTask 함수가 완료될 때까지 모든 것이 멈춘다.

---

## Non-Blocking

Non-Blocking은 함수가 호출된 후 그 작업이 끝나기 전에 바로 다음 코드로 넘어가는 것을 의미한다. 즉, 비동기 작업이 진행되는 동안에도 다른 작업을 계속할 수 있다. 이는 CPU가 멈추지 않고 다른 작업을 처리할 수 있게 해주므로, 전체 프로그램의 효율성을 높인다. 비동기적으로 실행되는 Non-Blocking 함수는 호출된 후 바로 다음 코드로 넘어가기 때문에 프로그램의 다른 부분이 멈추지 않는다.

예를 들어, 네트워크 요청을 하는 동안 사용자 인터페이스는 여전히 반응할 수 있다. 이는 사용자가 애플리케이션을 끊김 없이 사용할 수 있도록 도와준다.

### Non-Blocking 코드 예시

```js
function nonBlockingTask() {
  setTimeout(() => {
    console.log("Non-blocking task finished");
  }, 3000); // 3초 후에 실행
}

console.log("Start");
nonBlockingTask();
console.log("End");
```

위 코드를 실행하면 "Start"와 "End"가 즉시 출력되고, 3초 후에 "Non-blocking task finished"가 출력된다. `setTimeout` 함수는 비동기 함수로, 호출 즉시 반환하고 3초 후에 콜백 함수를 실행한다.

---

## 어떻게 사용해야 하는가?

기본적으로 자바스크립트 코드는 블로킹(Blocking) 방식으로 작동한다. 즉, 아무런 비동기 처리 없이 코드를 작성하면 한 작업이 끝날 때까지 다음 작업이 시작되지 않는다. 논블로킹(Non-Blocking) 코드를 작성하려면 비동기 프로그래밍을 사용해야 한다. `setTimeout`, `setInterval`, `fetch API`, `Promises`, `async/await`를 사용하여 비동기 작업을 처리할 수 있다.

---

## 발생 가능한 문제점

Blocking 코드는 긴 작업이 실행될 때 다른 작업이 지연될 수 있는 단점이 있다. 이는 사용자 경험을 저하시킬 수 있다. 반면, Non-Blocking 코드는 복잡성이 증가하고, 콜백 지옥(callback hell) 등의 문제가 발생할 수 있다. 이러한 문제를 해결하기 위해 프로미스(Promise)와 async/await 같은 비동기 제어 구조가 도입되었다.

### 콜백 지옥

콜백 지옥은 중첩된 콜백 함수들로 인해 코드의 가독성이 떨어지고 유지보수가 어려워지는 문제를 말한다. 이는 비동기 작업을 처리할 때 발생할 수 있는 일반적인 문제다.

```js
doSomething(function (result) {
  doSomethingElse(result, function (newResult) {
    doThirdThing(newResult, function (finalResult) {
      console.log(finalResult);
    });
  });
});
```

### Promise

Promise는 비동기 작업을 처리할 때 콜백 지옥을 피할 수 있는 방법이다. Promise는 비동기 작업의 완료 또는 실패를 나타내는 객체다.

```js
doSomething()
  .then((result) => doSomethingElse(result))
  .then((newResult) => doThirdThing(newResult))
  .then((finalResult) => console.log(finalResult))
  .catch((error) => console.error(error));
```

### async/await

async/await는 Promise를 더욱 간결하게 사용할 수 있게 해주는 문법이다. async 함수는 항상 Promise를 반환하고, await는 Promise가 처리될 때까지 함수의 실행을 일시 정지시킨다.

```js
async function main() {
  try {
    const result = await doSomething();
    const newResult = await doSomethingElse(result);
    const finalResult = await doThirdThing(newResult);
    console.log(finalResult);
  } catch (error) {
    console.error(error);
  }
}

main();
```

---

# 결론

Blocking과 Non-Blocking의 개념을 이해하는 것은 자바스크립트 프로그래밍에서 매우 중요하다. 이를 통해 코드의 효율성을 높이고, 시스템 자원을 효과적으로 사용할 수 있다. 각 개념의 사용 사례와 장단점을 잘 이해하고, 상황에 맞게 적절히 활용하는 것이 중요하다. 이벤트 루프와 비동기 프로그래밍을 통해 자바스크립트는 싱글 스레드의 한계를 극복하고, 더욱 효율적이고 반응성 있는 애플리케이션을 개발할 수 있다.

---

# 참조 문헌

> [[JavaScript] 동기(Sync)와 비동기(Async)](https://velog.io/@clydehan/JavaScript-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Sync-Async) > [[CS, Computer Science] 프로세스와 스레드](https://velog.io/@clydehan/CS-Computer-Science-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C)
> 스레드의 개념과 자바스크립트의 동기와 비동기에 대해서는 해당 링크에서 자세하게 다룬다.
