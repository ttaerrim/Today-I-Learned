# Hydration (수화)

## ✔️ Hydrate란

Server Side 단에서 렌더링된 정적 페이지와 번들링된 JS 파일을 클라이언트에게 보낸 뒤, 클라이언트 측에서 HTML 코드와 JS 코드를 서로 매칭시키는 과정.

`Next.js` 웹 페이지의 구성 원리를 생각해 보자.

Next.js는 클라이언트에게 웹 페이지를 보내기 전 Server Side Rendering을 수행한다.  
Pre-Rendering으로 생성된 HTML Document를 클라이언트에게 전송하는데, 클라이언트가 받은 이 웹 페이지는 자바스크립트 요소가 없는 단순 HTML 파일이다.

> 이후 리액트가 번들링된 자바스크립트 코드들을 클라이언트에게 전송하면, 전송된 자바스크립트 코드들이 이전에 보내진 HTML DOM 요소 위에 한번더 렌더링이 되면서 매칭된다.

이 과정을 `Hydrate`라고 부른다.

→ `Hydrate`: `클라이언트 측 자바스크립트가 정적 호스팅 또는 SSR을 통해 전달되는 정적 HTML 요소에 이벤트 핸들러를 첨부하여 동적 웹 페이지로 변환하는 기술`

## `render` vs `hydrate`

```
ReactDOM.render(element, container, [callback])
```

`render` 함수는 두 번째 파라미터인 지정된 DOM 요소에 하위 요소를 넣어 렌더링하는 방식이며, 렌더링이 완료되면 특정 이벤트를 처리할 콜백 함수를 세 번째 인자로 넣어 줄 수 있다.

```
ReactDOM.hydrate(element, container, [callback])
```

`hydrate` 함수는 특정 컴포넌트를 두 번째 파라미터인 지정된 DOM 요소에 하위로 Hydrate 처리만 한다. 렌더링을 통해 새 웹 페이지를 구성하는 것이 아니라 기존 DOM Tree에서 해당되는 DOM 요소를 찾아 정해진 자바스크립트 속성들만 적용시킨다는 뜻이다.
