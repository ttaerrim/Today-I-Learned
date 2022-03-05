# Next.js 시작하기

## 터미널에서 프로젝트 만들기

```bash
npx create-next-app@latest
# or
yarn create next-app
```

타입스크립트로 프로젝트를 만들고 싶다면?

```bash
npx create-next-app@latest --typescript
# or
yarn create next-app --typescript
```

## CRA 프로젝트와 다른 점

CRA로 만든 리액트 프로젝트의 소스 코드를 보면 이런 식이다.

```html
<!DOCTYPE html>
<html lang="ko-KR">
  <head>
    <title>커리어 여정을 행복하게, 원티드</title>
    <script defer src="/static/js/bundle.js"></script>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

보다시피 `id='root'`인 div 안에 아무 내용도 없다.

Next.js로 리액트 프로젝트를 만들면 어떨까?

![](https://user-images.githubusercontent.com/43867711/156883950-4f2867a0-6dc8-434a-bb62-7e0a70cbfcef.png)

너무 길어서 코드 전체를 가져오진 못했지만 <body> 안에 많은 내용이 미리 들어가 있다.

왜 이런 차이가 발생하는 걸까?

## CSR vs SSR

이유는 CSR와 SSR에 있다. CRA는 CSR로 실행되고, Next.js는 SSR로 실행되기 때문이다.

### CSR

> Client Side Rendering

CSR은 웹 페이지의 렌더링이 브라우저 측에서 일어난다.  
최초로 불러온 html의 내용은 비어 있고, js 파일이 비로소 로드되고 나서야 id=root인 div를 찾아 그 안에 html를 그리는 구조이기 때문에 div 안에 아무것도 없는 html이 보이는 것이다.

<img width="500" src="https://d2.naver.com/content/images/2020/06/csr.png"/>

### SSR

> Server Side Rendering

SSR은 첫 페이지 렌더링을 클라이언트 측이 아닌 서버에서 처리하는 방식이다.  
서버는 즉시 렌더링 가능한 html 파일을 만들고, 그 파일을 클라이언트에 전달한다. 사용자는 JS 파일이 로드되는 동안 페이지를 미리 확인할 수 있다.

<img width="500" src="https://d2.naver.com/content/images/2020/06/ssr.png"/>

## SEO

> Search Engine Optimization

SEO란 **검색 엔진 최적화**라는 뜻으로, 검색 엔진에서 찾기 쉽도록 사이트를 개선하는 프로세스이다.  
홈페이지나 콘텐츠를 검색 결과로 노출시키는 방법 중 하나이다.
검색 엔진은 인터넷의 정보를 미리 수집, 정리하고 유저가 관련된 내용을 검색했을 때 찾을 수 있도록 데이터베이스를 관리한다.

위에서 설명한 이유로 CRA로 만든 프로젝트는 검색 노출이 잘 되지 않는다는 단점이 있다.

### 참고

[검색엔진 최적화, SEO를 알아봅시다](https://www.bloter.net/newsView/blt201805130001)  
[어서 와, SSR은 처음이지?](https://d2.naver.com/helloworld/7804182)
