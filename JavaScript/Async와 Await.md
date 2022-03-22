# Async와 Await

## ✔️ Async/Await란?

Async/Await는 프로미스의 사용 방법을 개선하기 위해 ES8에서 새롭게 소개된 비동기 처리 문법이다.

> ⚠️ 따라서 구 버전 웹 브라우저에서는 지원되지 않으므로 사용 전 호환성 확인이 필요하다.

Async 함수 내부에서 프로미스를 사용해 비동기 요청 결과를 반환한다.

[Promise](https://github.com/ttaerrim/Today-I-Learned/blob/main/JavaScript/Promise.md)의 다중 프로미스 처리를 Async/Await를 사용해 바꿔 보자.

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

let asyncFunc = async () => {
  try {
    let res = await asyncWork(100);
    console.log("resolve1: " + res);
    res = await asyncWork(res);
    console.log("resolve2: " + res);
    res = await asyncWork(res);
    console.log("resolve3: " + res);
  } catch (error) {
    console.log("catch: " + error);
  }
};

asyncFunc();
```

```console
resolve1: 80
resolve2: 60
catch: 40
```

Async와 Await는 항상 쌍으로 사용해야 한다. 함수 내부에 `await`를 사용한다면 함수를 선언할 때 `async` 함수로 선언해야 한다.

async 함수를 실행하면 async 함수 안에 선언된 프로미스가 실행되는데, await 키워드가 있는 프로미스는 이행 완료 상태가 될 때까지 다음 행의 코드를 실행하지 않고 대기한다.

이행 완료가 되면 콘솔에 응답 내용을 출력하고 다음 코드를 실행한다.

프로미스 비동기 코드에서 거부(reject)가 발생하면 try ~ catch 처리에 따라 catch() 예외 처리를 실행하고 비동기 처리를 종료한다.

## ✔️ Async/Await의 장점

- 일반 자바스크립트 코드가 동기적으로 실행되는 것과 같은 **형태**로 비동기 처리를 순차적으로 실행
- 자바스크립트의 동기적 함수/코드와 이질감이 있는 프로미스의 체인 방식 비동기 처리와 콜백 함수를 일일이 작성해야 하는 불편함이 사라짐
- Fetch보다 가독성이 좋음
