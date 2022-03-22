# Fetch

## ✔️ 간편한 Promise 구현을 위한 Fetch

`fetch`는 프로미스를 재포장해 구문을 조금 더 간결하게 사용할 수 있도록 만든 래퍼이다. 기술적으로 프로미스와 동일하다.  
프로미스 인스턴스를 만들 필요 없이 fetch 메소드에 비동기 요청을 할 URL만 넘기면 리턴 값으로 프로미스 인스턴스를 반환하기 때문에 then~catch로 순차 처리 하는 것도 가능하다.

```javascript
fetch("https://www.request.com/api/data.json")
  .then((response) => {
    console.log(response);
  })
  .catch((error) => {
    console.log(error);
  });
```

fetch는 비동기 요청이 실패했을 경우만 catch() 내 구문을 실행한다. 404 에러(페이지 없음)나 500 에러(HTTP 상태)는 실행이 완료된 것으로 간주하기 때문에 이러한 에러를 확인하기 위해서는 then() 안에서 상태를 확인해 별도로 그에 맞는 처리를 해야 한다.

```javascript
if (response.status >= 200 && response.state <= 299) {
  // 정상적으로 완료
} else {
  // 에러 발생
}
```

### 🚩 참고

[ES6로 기초부터 다시 배우는 자바스크립트 파워북](http://www.yes24.com/Product/Goods/93235652)
