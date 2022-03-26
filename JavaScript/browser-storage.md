# 브라우저 저장소

## 🌐 웹 스토리지 (Web Storage)

Web Storage란 HTML5부터 제공하는 기능으로, 해당 도메인과 관련된 특정 데이터를 서버가 아닌 클라이언트 웹 브라우저에 저장할 수 있도록 하는 기능이다.

쿠키(cookie)와 비슷한 기능이고, 키-값 쌍으로 데이터를 저장하고 키를 기반으로 데이터를 조회할 수 있다. Local Storage와 Session Storage를 따로 두어 데이터의 지속성을 구분할 수 있어 응용 환경에 맞는 선택이 가능하다.

Web Storage는 사이트의 도메인 단위로 접근이 제한된다. A 도메인에서 저장한 데이터는 B 도메인에서 조회할 수 없다.

웹 스토리지는 보안성이 없으므로 인증, 결제와 관련된 중요 정보를 저장해서는 안 된다.

### ✔️ 로컬 스토리지 (Local Storage)

로컬 스토리지는 브라우저에 반영구적으로 데이터를 저장하며 브라우저를 종료해도 데이터가 유지된다. 도메인이 다른 경우에는 로컬 스토리지에 접근할 수 없다. `www.google.com`에서 로컬 스토리지에 저장한 데이터를 `www.naver.com`에서 접근할 수 없다는 뜻이다.

코드 상에서는 다음과 같이 접근할 수 있다.

```javascript
localStorage.setItem("item1", 10);
localStorage.setItem("item2", "new item");
localStorage.setItem("item3", 0.543534);

console.log(localStorage.getItem("item1"));
console.log(localStorage.getItem("item2"));
console.log(localStorage.getItem("item3"));
```

크롬 브라우저의 개발자 도구 - 애플리케이션 - 로컬 스토리지에서 확인할 수 있다.
<img width="1435" alt="스크린샷 2022-03-26 21 59 21" src="https://user-images.githubusercontent.com/43867711/160240532-bd672448-ccc4-4bb2-aa39-5f97015a8266.png"/>

콘솔에는 다음과 같이 출력된다.

<img width="154" alt="스크린샷 2022-03-26 21 59 28" src="https://user-images.githubusercontent.com/43867711/160240538-f6d459ad-9041-4655-b4e7-941b873b3616.png"/>

로컬 스토리지에는 문자열만 사용할 수 있고, 숫자나 객체 등을 저장하면 자동으로 문자열로 캐스팅된다.

### ✔️ 세션 스토리지 (Session Storage)

세션 스토리지는 각 세션마다 데이터가 개별적으로 저장된다. 브라우저에서 여러 개의 탭을 실행하면 탭마다 개별적으로 데이터가 저장되는 것이다. 세션 스토리지는 세션을 종료하면 데이터가 자동으로 삭제되며, 같은 도메인이더라도 세션이 다르면 데이터에 접근할 수 없다. 세션 스토리지에서 데이터를 읽고 쓰는 방법은 로컬 스토리지에서 데이터에 접근하는 방법과 동일하다.

```javascript
sessionStorage.setItem("item1", 10);
sessionStorage.setItem("item2", "new item");
sessionStorage.setItem("item3", 0.543534);

console.log(sessionStorage.getItem("item2"));
console.log(sessionStorage.getItem("item2"));
console.log(sessionStorage.getItem("item3"));
```

### ❗️ 로컬 스토리지와 세션 스토리지의 차이

로컬 스토리지와 세션 스토리지의 차이는 **영구성**에 있다. 로컬 스토리지는 데이터를 직접 지우기 전까지 브라우저에 데이터가 남고, 세션 스토리지는 윈도우나 브라우저 탭을 닫을 경우 삭제된다.

따라서 지속적으로 필요한 데이터는 로컬 스토리지에 저장하고, 일시적으로 필요한 데이터는 세션 스토리지에 저장하는 것이 바람직하다.

## 🍪 쿠키 (Cookie)

쿠키는 브라우저에 저장되는 작은 크기의 문자열로, HTTP 프로토콜의 일부이다.

쿠키는 주로 웹 서버에 의해 만들어진다. 서버가 HTTP 응답 헤더의 `set-cookie`에 내용을 넣어 전달하면, 브라우저는 이 내용을 자체적으로 브라우저에 저장한다. 브라우저는 사용자가 쿠키를 생성하도록 한 동일 서버에 접속할 때마다 쿠키의 내용을 Cookie 요청 헤더에 넣어서 함께 전달한다.

쿠키는 클라이언트 식별과 같은 인증에 가장 많이 쓰인다.

1. 사용자가 로그인하면 서버는 HTTP 응답 헤더의 `set-cookie`에 담긴 "세션 식별자(session identifier)" 정보를 사용하여 쿠키를 설정한다.
2. 사용자가 동일 도메인에 접속하려고 하면 브라우저는 HTTP Cookie 헤더에 인증 정보가 담긴 고유값(세션 식별자)을 함께 넣어 서버에 요청을 보낸다.
3. 서버는 브라우저가 보낸 요청 헤더의 세션 식별자를 읽어 사용자를 식별한다.

`document.cookie` 프로퍼티를 이용하면 브라우저에서도 쿠키에 접근할 수 있다.

#### 쿠키의 단점

- 4KB의 데이터 저장 제한으로 사이즈가 작음
- HTTP Request에 암호화되지 않은 상태로 사용하기 때문에 보안에 취약
- 모든 HTTP Request에 포함되어 있기 때문에 웹 서비스 성능에 영향 끼칠 수 있음 -> 데이터 전달 낭비

### ❗️ 웹 스토리지와 쿠키의 차이

|                  | 쿠키                                    | 웹 스토리지        |
| :--------------- | :-------------------------------------- | :----------------- |
| 서버 전송        | 모든 HTTP 요청에 포함되어 서버로 전송됨 | 서버 전송하지 않음 |
| 용량 제한        | 최대 4KB                                | 최대 5MB           |
| 영구 데이터 저장 | 만료일을 설정해야 함                    | 영구 저장 가능     |

## ✔️ 사용 예시

| 브라우저 저장소 | 예시                          |
| :-------------- | :---------------------------- |
| 로컬 스토리지   | 자동 로그인                   |
| 세션 스토리지   | 입력 폼 정보, 비회원 장바구니 |
| 쿠키            | 다시 보지 않음 팝업 창        |

### 🚩 참고

[ES6로 기초부터 다시 배우는 자바스크립트 파워북](http://www.yes24.com/Product/Goods/93235652)  
[웹 스토리지 (Web Storage)의 특성과 사용법](https://untitledtblog.tistory.com/47)  
[브라우저 저장소(웹저장소), Cookie, Session란?](https://akdl911215.tistory.com/317?category=975797)  
[쿠키와 document.cookie](https://ko.javascript.info/cookie)
