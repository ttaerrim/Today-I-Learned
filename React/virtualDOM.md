# Virtual DOM

Virtual DOM이란 말 그대로 가상 돔이다. 가상 돔은 왜 필요할까.  
 알아보기 전에 브라우저 작동 방식에 대해 간단히 알아보고 넘어가자.

## ✔️ 브라우저 작동 방식

![](https://media.vlpt.us/images/kim-jaemin420/post/ed6d061e-acb8-4cae-ad79-429205f2c7db/image.png)

**브라우저 엔진**은 사용자 인터페이스와 렌더링 엔진 사이 동작을 제어해 주는 엔진이다.  
**렌더링 엔진**은 HTML documents와 웹 페이지의 리소스들을 브라우저 시각적으로 출력해 준다.

### 브라우저가 화면에 그리는 과정

1. 요청과 응답  
   URL에 주소를 입력하고 엔터를 누르면 HTTP 요청이 전송되고 해당 서버에서 HTML document를 응답으로 보낸다.
2. HTML 파싱과 DOM 생성  
   브라우저는 HTML 소스코드를 파싱하고 돔 트리를 생성한다. 돔 트리는 데이터를 표현한 것인데, 여기에는 HTML 태그에 해당하는 노드와 태그 사이 텍스트 청크에 해당하는 텍스트 노트가 들어가 있다. 돔 트리의 루트 노트는 `<html>`이다.
3. CSS 파싱과 CSSOM 생성
   브라우저 렌더링 엔진은 HTML을 처음부터 한 줄씩 순차적으로 파싱해 DOM을 생성한다. 그러나 CSS를 로드하는 link나 style 태그를 만나면 DOM 생성을 일시 중단하고 CSS 파일을 서버에 요청해 CSS를 해석하여 CSSOM을 생성한다. 이후 CSS 파싱이 완료되면 HTML 파싱이 중단된 시점부터 다시 파싱을 시작해 DOM을 생성한다.
4. 렌더 트리 생성
   렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱해 DOM과 CSSOM을 생성한다. 그리고 DOM과 CSSOM은 렌더링을 위해 렌더 트리로 결합된다. 만약 `display: none`으로 숨긴 `div`가 있다면 이는 렌더 트리에 표현되지 않는다.  
   완성된 렌더 트리는 HTML 요소의 레이아웃을 계싼하는 데 사용되고 브라우저 화면에 픽셀을 렌더링하는 페인팅 처리에 입력된다.

## ✔️ Virtual DOM이 필요한 이유

DOM이 변경될 때마다 브라우저는 CSS를 다시 계산하고 웹 페이지를 다시 렌더링한다. 이 상황이 반복되면 그만큼 브라우저의 렌더링은 잦아지고, 자원을 많이 소모하게 된다. SPA에서는 DOM 조작이 많이 발생하므로 브라우저의 연산량이 많아지고 전체적인 프로세스는 비효율적으로 동작한다. 이 문제를 해결하기 위해 Virtual DOM 기술이 도입되었다.

## ✔️ Virtual DOM 동작 방식

Virtual DOM은 브라우저의 불필요한 렌더링을 줄이기 위해 도입되었다고 했다. 그럼 어떻게 문제를 해결했을까?

Virtual DOM은 UI 가상 표현을 메모리상에 두고 [재조정(Reconciliation)](https://reactjs.org/docs/reconciliation.html) 과정을 통해 실제 DOM과 동기화하는 것을 말한다. 재조정 과정은 크게 3단계로 구성된다.

Virtual DOM은 DOM을 추상화한 가상의 객체이다. 이 가상의 객체를 메모리에 만들어 놓는다. 그리고 변경 사항을 DOM에 직접 수정하는 것이 아니라 Virtual DOM을 수정하고 Virtual DOM을 통해 DOM을 수정한다.
Virtual DOM에 변경된 부분을 모으고, 실제 DOM과 Virtual DOM의 차이를 파악한 뒤 변경된 부분만 바꾸는 형식이다. 이로써 브라우저 내에 발생하는 연산량을 줄이면서 성능을 개선시키는 것이다.

![](https://miro.medium.com/max/700/1*8OCCATi8_5HmWI1QpjrRNA.png)

새로운 엘리먼트 트리가 그려질 때,

1. 서로 다른 타입의 두 엘리먼트 트리를 만든다.
2. key props를 통해 여러 렌더링 사이에서 어떤 엘리먼트가 변경되고 변경되지 않는지 표시해 줄 수 있다.

여기서 서로 다른 타입이라는 말은 Root 엘리먼트의 타입을 말한다. <a>에서 <img>로, <Article>에서 <Comment>로, 혹은 <Button>에서 <div>로 바뀌는 것 모두 트리 전체를 다시 만드는 경우이다.

예를 들어 아래와 같은 변화가 일어난다고 해 보자.

```html
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

root 엘리먼트가 다른 경우이므로 이전 `Counter`는 사라지고 새롭게 렌더링될 것이다.

#### DOM 엘리먼트 타입이 같은 경우

같은 타입의 React DOM 요소를 비교할 때는 어떨까. 두 엘리먼트의 속성을 비교하고 같은 속성은 유지, 달라진 속성만 변경한다.

```html
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

두 엘리먼트를 비교할 때는 className만 변경한다.

`style`이 변경될 때도, React는 또한 변경된 속성만을 갱신한다.. 예를 들어,

```
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />
```

위 두 엘리먼트 사이에서 변경될 때, React는 `fontWeight`는 수정하지 않고 `color` 속성만을 수정한다.

### 🚩 참고

[Virtual DOM 동작 원리와 이해 (with 브라우저의 렌더링 과정)](https://jeong-pro.tistory.com/210)  
[ 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?](https://velopert.com/3236)  
[재조정](https://ko.reactjs.org/docs/reconciliation.html#gatsby-focus-wrapper)
