# Promise

## ✔️ Promise란?

중첩되는 비동기 통신 로직과 콜백 지옥의 문제를 해결하기 위한 기법이다.

> 인터넷 익스플로러는 프로미스를 지원하지 않기 때문에 인터넷 익스플로러 호환성이 필요하다면 프로미스 대신 AJAX를 사용해야 한다.

## ✔️ 프로미스 객체의 상태

프로미스 인스턴스는 진행 중인 비동기 작업, 그에 따른 결과 처리 방법, 상태 정보를 가지고 있다.

프로미스 인스턴스는 `Pending`, `Fulfilled`, `Rejected` 3개의 상태 정보를 가진다. 그리고 상태에 따른 2개의 메소드 `resolve`와 `reject`를 가진다.

| 상태                 | 설명                                                             |
| :------------------- | :--------------------------------------------------------------- |
| 대기 중(Pending)     | 프로미스 객체의 기본 상태, 비동기 처리 결과가 나오지 않은 상태   |
| 이행 완료(Fulfilled) | 비동기 처리가 완료되어 결과를 얻은 상태. resolve() 함수를 호출함 |
| 거부됨(Rejected)     | 비동기 처리는 완료되었지만 실패한 상태. reject() 함수를 호출함   |

## ✔️ 프로미스 기본 사용법

```javascript
let myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    let result = "promise fulfilled";
    resolve(result);
  }, 1000);
});

myPromise
  .then((successMessage) => {
    console.log(successMessage);
  })
  .catch((failMessage) => {
    console.log(failMessage);
  });
```

새로운 프로미스 객체를 작성할 때는 파라메터 2개를 가지는 비동기 처리 함수를 작성한다. 첫 번째 파라미터는 성공했을 때 호출하는 **이행 완료 함수명**이고, 두 번째 파라미터는 실패했을 때 호출하는 **거부 함수명**이다.

관습적으로 이행 완료 함수명은 resolve, 거부 함수명은 reject로 작성하지만 success, fail 등으로 작성해도 괜찮다.

파라메터에서 정한 함수명을 호출하면 then()과 catch() 중 하나를 호출한다. resolve()가 호출되면 then() 메서드의 파라메터에 정의한 콜백 함수가 실행되고, reject()가 호출되면 catch() 메서드의 파라메터에 정의한 콜백 함수가 실행된다.

## ✔️ 다중 프로미스 처리

```javascript
function asyncWork(value) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      value -= 20;
      if (value > 50) {
        resolve(value);
      } else {
        reject(value);
      }
    });
  });
}

asyncWork(100)
  .then((value) => {
    console.log(value);
    return asyncWork(value);
  })
  .then((value) => {
    console.log(value);
    return asyncWork(value);
  })
  .then((value) => {
    console.log(value);
    return asyncWork(value);
  })
  .catch((error) => {
    console.log("catch: " + error);
  });
```

```console
80
60
catch: 40
```

asyncWork 함수는 프로미스 인스턴스를 반환한다. 따라서 함수를 호출하며 메서드 체인으로 then()을 실행할 수 있다. 예제에서도 then()을 연달아 체인 방식으로 여러 개 호출한다.

다중 프로미스는 체인으로 호출하는 then() 메서드의 콜백 함수의 리턴 값으로 프로미스 인스턴스를 생성하는 함수를 다시 호출한다는 것이다.

반환된 프로미스 인스턴스는 다음 번 then()의 프로미스 인스턴스가 된다.

then() 메서드는 콜백 함수에서 반환하는 asyncWork() 프로미스 인스턴스의 비동기 처리가 완료되기 전까지 다음 then()을 호출하지 않고 대기한다.

따라서 모든 비동기 처리 작업은 then의 콜백 함수 순서대로 하나씩 실행되고, 콜백 함수의 파라메터를 통해 결과 값을 순차적으로 전달한다.

순서대로 then() 메서드를 붙여 나열하기 때문에 비동기 처리를 위해 코드를 중첩할 필요가 없다.

마지막으로 프로미스 인스턴스 상태가 거절 상태가 되면 다음 then()이 아닌 catch()를 실행하며 에러 메시지를 출력하고 종료한다.

예제는 value를 20씩 차감하며 다음 then()을 호출하고, 값이 50보다 작으면 거절 상태로 변경한 후 catch()를 호출하면서 비동기 처리 체인을 종료한다.

프로미스를 통한 비동기 처리 작업은 중복 작성할 필요가 없기 때문에 처리 구조의 효율이 좋아지고 코드 량도 줄일 수 있다. 또한 비동기 처리 흐름이 한눈에 들어오고 실행 구조와 순서를 명확하게 파악할 수 있다.

## ✔️ `Promise.all()`

`Promise.all()` 메소드는 Array와 같이 순회 가능한 객체의 모든 프로미스가 이행 완료한 후 이행한 프로미스를 반환한다. 만일 프로미스 중 하나가 거부된다면, 첫 번째로 거부된 프로미스를 즉시 반환한다.

Promise.all()은 여러 개의 비동기 작업을 병렬로 처리하고 싶을 때 사용한다. 순차적으로 진행해야 하는 비동기 작업이 아니라면 Promise.all()을 사용하여 한꺼번에 처리하고, 시간을 단축할 수 있다.

```javascript
async function display(text, time) {
  return new Promise((resolve, reject) => {
    setTimeout(
      () =>
        typeof text === "string"
          ? resolve("string입니다")
          : reject("string이 아닙니다"),
      time
    );
  });
}

/* await를 사용했을 때 */

console.time("소요 시간");
await display("태림", 3000);
await display("ttaerrim", 2000);
await display("LTR", 1000);
console.timeEnd("소요 시간");

// 소요 시간: 6011.14306640625 ms

/* Promise.all 사용했을 때 */
console.time("소요 시간");
await Promise.all([
  display("태림", 3000),
  display("ttaerrim", 2000),
  display("LTR", 1000),
]);
console.timeEnd("소요 시간");

// 소요 시간: 3001.381103515625 ms
```

에러를 처리하는 경우에도 Promise.all()의 경우가 걸리는 시간이 더 짧다.

```javascript
/* await를 사용했을 때 */

console.time("소요 시간");
try {
  await display(1, 3000);
  await display(2, 2000);
  await display(3, 1000);
} catch (error) {
  console.log(error);
}
console.timeEnd("소요 시간");

// 소요 시간: 3002.85791015625 ms

/* Promise.all 사용했을 때 */
console.time("소요 시간");
try {
  await Promise.all([display(1, 3000), display(2, 2000), display(3, 1000)]);
} catch (error) {
  console.log(error);
}
console.timeEnd("소요 시간");

// 소요 시간: 1002.778076171875 ms
```

같은 비동기 처리를 하는 경우이지만, await로 각각 실행시켰을 경우 첫 번째 함수가 실행되는 데 걸리는 시간인 3초를 모두 다 채우고 반환된다. Promise.all()을 사용했을 경우에는 가장 빨리 에러를 반환하는 함수를 기준으로 동작해 가장 짧은 시간인 1초만에 에러를 반환한다.

### 🚩 참고

[ES6로 기초부터 다시 배우는 자바스크립트 파워북](http://www.yes24.com/Product/Goods/93235652)  
[Promise.all()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)  
[언제 Promise.all을 사용해야 될까?](https://code-masterjung.tistory.com/91)
