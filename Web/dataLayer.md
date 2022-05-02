# 데이터 레이어 (Data Layer)

## ✔️ 데이터 레이어란?
데이터 레이어란 **태그 관리자**로 정보를 전달하기 위한 자바스크립트 개체이다.

페이지 제목, URL과 같은 페이지 정보나 결제 수단, 상품 카테고리 등 회원의 구매 정보 등을 넣을 수 있고, 그 값을 GTM(Google Tag Manager)으로 수집하여 GA(Google Analytics)에서 분석 용도로 활용할 수 있다.

```javascript
const sendAPEvent = params => {
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push(params)
};

sendAPEvent({ userId: 'abf3-3242-ffs1-23bd', 
              weather: 'cloudy',
              event: "ApplyButtonClickOnChallengePage" 
            })
```

구글 도움말 문서를 보면 데이터 레이어 사용을 권장하고 있다.  
데이터 레이어는 좀 더 유연하고 쉽게 GA를 구축할 수 있게 해 주며 불필요한 개발 리소스가 낭비되지 않게 해 준다.  
위 코드를 확인해 보면 `dataLayer.push`로 배열을 전달해 준다. userId, weather, event 정보를 담고 있는데 이것은 GA가 자동으로 수집하지 않는 정보이다.  
때문에 별도로 수집할 필요가 있고, dataLayer가 그 역할을 도와준다고 보면 된다.