# React version 18

SSR 사용 시 기존에 동기적으로 코드를 불러 오기 때문에 API fetch 작업이 시간이 걸릴 시 초기 렌더링 속도가 느리다는 단점이 있었음.

react 18에서는 초기 렌더링을 할 때 빠르게 준비되는 부분부터 렌더링할 수 있는 기능을 도입하였음. 바로 `Streaming HTML`과 `Selective Hydration`!

```
<Suspense fallback={<Loading />}>
    <LongTimeComponent />
</Suspense>
```

다음과 같이 `Suspense`로 시간이 오래 소요되는 컴포넌트를 감싸준다. fallback props로 넘긴 `<Loading/>`은 렌더링이 완료되기 전까지 대신 보여지게 된다.

그 후, 서버에서 `<LongTimeComponent />`를 렌더링할 준비가 끝나면 리액트는 추가적으로 html 코드를 스트리밍한다.

`<Suspense>` 태그는 `React.lazy`와 함께 큰 번들의 자바스크립트 코드들을 작은 청크들로 나누어 로드될 수 있도록 해 주는 코드 스플리팅 기법에서 사용된다.

그러나 React18 이전에는 SSR을 구현할 때 사용되는 `renderToString`과 함께 사용할 수는 없었고, loadable-component와 같은 서드파티 라이브러리를 함께 사용해야 했다. React18부터는 서드파티 라이브러리 없이 `<Suspense>`를 SSR 환경에서도 이용할 수 있게 되었다.

### Selective Hydrating (선택적 수화)

렌더링하는 데 오래 걸리는 컴포넌트들을 `<Suspense>`로 감싸고, 해당 부분이 fallback 엘리먼트를 내보내고 있는 경우에도 페이지의 다른 부분은 hydrating을 시작할 수 있다.

기존에는 모든 페이지가 완전하게 인터렉션할 수 있는 상태가 될 때까지 기다려야 했지만
