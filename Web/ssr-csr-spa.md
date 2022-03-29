# CSR과 SSR

## ✔️ Server-Side Rendering

<img src="https://miro.medium.com/max/1400/1*jJkEQpgZ8waQ5P-W5lhxuQ.png"/>

SSR은 클라이언트에게 보여 줄 페이지를 모두 서버에서 구성하여 보여 주는 방식이다. 클라이언트에게 요청을 받으면 각 요청에 맞는 HTML 파일을 렌더링해서 브라우저에 띄운다. 서버에서 렌더링을 작업하는 방식이다.

### 장점

- 빠른 초기 로딩 속도
- SEO (Search Engine Optimization) 최적화

### 단점

- 불필요한 부분도 같이 렌더링
- 사용자 경험: 요청 시마다 새로고침 하는 등

## ✔️ SPA: Single Page Application

SPA는 기본적으로 하나의 페이지로 구성되며 기존 서버 사이드 렌더링과 비교했을 때 배포가 간단하고 네이티브 앱과 유사한 사용자 경험을 제공할 수 있다는 장점이 있다.

이전에 많이 쓰이던 전통적인 화면 전환 방식은 새로운 페이지를 요청할 때마다 정적 리소스가 다운되고 전체 페이지를 다시 렌더링해 새로고침이 발생하므로 사용성이 좋지 않다. 그리고 필요 없는 부분까지 새로 렌더링하므로 비효율적이다.

SPA는 기본적으로 웹 애플리케이션에 필요한 모든 정적 리소스를 최초 접근 시 한 번만 다운로드한다. 이후 새로운 페이지가 요청되면 페이지 갱신에 필요한 데이터만을 JSON으로 전달받아 페이지를 갱신하므로 전체 트래픽을 감소시킬 수 있고, 필요한 부분만 갱신하므로 새로고침이 발생하지 않아 네이티브 앱과 유사한 사용자 경험을 제공할 수 있다.

SPA의 핵심 가치는 UX 향상에 있다.

### SPA의 단점

- 초기 구동 속도가 느림
- SEO 이슈

## ✔️ Client-Side Rendering

<img src="https://miro.medium.com/max/1400/1*CRiH0hUGoS3aoZaIY4H2yg.png"/>

CSR은 클라이언트 측에서 HTML을 받은 후 script가 동작하면서 클라이언트 측에서 렌더링을 진행하는 것이다. 점점 더 복잡해지는 웹 페이지를 구현하기 위해 CSR 방식이 등장했다.

CSR은 사용자의 요청에 따라 필요한 부분만 응답받아 렌더링하는 방식이다.

### 장점

- 초기 화면 제외했을 때 빠른 속도
- 필요한 부분만 요청하고 응답하기 때문에 서버 부하 감소
- 사용자 경험 -> 깜박임 x

### 단점

- SEO 불리
- 초기 로딩 속도 느림

---

### 🚩 참고

- [The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)
- [Single Page Application & Routing](https://poiemaweb.com/js-spa)
- [SSR(서버사이드 렌더링)과 CSR(클라이언트 사이드 렌더링)](https://miracleground.tistory.com/165)
