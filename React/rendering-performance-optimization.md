# 렌더링 성능 최적화

컴포넌트가 렌더링 된다는 건 함수가 호출되어 실행된다는 것을 말한다. 그리고 그 함수가 실행될 때마다 내부에 선언되어 있는 변수나 함수 등도 매번 다시 선언되어 사용된다.

컴포넌트는 자신의 state가 변경되거나 부모의 props가 변경될 때마다 리렌더링되기 때문에 성능 최적화 설정을 하지 않는다면 리렌더링이 불필요한 컴포넌트도 함께 리렌더링되는 일이 발생한다.

## ✔️ useMemo

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

`useMemo`에서 Memo는 흔히 사용하는 Memorize의 메모라는 뜻이 아니고 **Memoization**이라는 뜻이다.
여기서 **메모이제이션**이란 주어진 입력 값에 대한 결과를 저장함으로써 같은 입력 값에 대해 함수가 한 번만 실행되는 것을 보장한다는 뜻이다. 리액트의 `useMemo`는 **메모이제이션된 값을 반환**한다.

`useMemo`를 사용하면 의존성 배열에 넘겨 준 값이 변경되었을 때만 메모이제이션된 값을 다시 계산한다.

## ✔️ useCallback

`useCallback`은 **메모이제이션된 콜백을 반환**한다. 서론에서 컴포넌트가 렌더링될 때마다 내부에 선언된 변수나 함수들도 매번 다시 선언되어 사용된다고 했다.

```javascript
import { useState, useCallback } from "react";
const [userInput, setUserInput] = useState("");

const handleChange = useCallback((e) => {
  setUserInput(e.target.value);
}, []);
```

`useCallback`은 예시와 같이 이벤트 핸들러 함수나 API를 요청하는 함수를 작성할 때 주로 사용한다.

리액트 공식 문서에 따르면 하위 컴포넌트가 React.memo() 같은 것으로 최적화 되어 있고, 그 하위 컴포넌트에게 callback 함수를 props로 넘길 때 상위 컴포넌트에서 useCallback으로 함수를 선언하는 것이 유용하다고 한다.

→ 상위 컴포넌트에서 callback 함수를 재선언한다면, 동일한 함수일지라도 props로 callback 함수를 넘겨 받는 하위 컴포넌트 입장에서는 props가 변경되었다고 인식한다. React.memo로 함수형 컴포넌트 자체를 감싼다면 넘겨 받는 props가 변경되지 않았을 때는 상위 컴포넌트가 메모이제이션된 함수형 컴포넌트를 사용, 즉 이전에 렌더링한 결과를 사용하게 된다.

<br/>

기존에 만들었던 코드를 리팩토링하며 예시를 들어 보자.

```javascript
import React from "react";

const Input = React.forwardRef((props, ref) => {
  const { type = "text", onChange, onFocus, onBlur } = props;
  return (
    <input
      type={type}
      onChange={onChange}
      onFocus={onFocus}
      onBlur={onBlur}
      ref={ref}
    />
  );
});
export default React.memo(Input);
```

사용자로부터 입력을 받을 수 있는 입력 창의 재사용성을 높이기 위해 Input 컴포넌트를 만들었다. 이 Input 컴포넌트는 필터 태그의 검색창에 사용된다. Input 컴포넌트를 React.memo로 감싸 주었다.

```javascript
import React, { useCallback, useContext, useRef } from "react";

import Input from "layout/Input";
const FilterTag = () => {
  const inputRef = useRef();
  const { setIsShow } = useContext(ItemContext);

  return (
    <SectionBody>
      // 생략
      <Input
        type="text"
        onChange={handleChange}
        onFocus={useCallback(() => setIsShow(true), [])}
        onBlur={useCallback(() => setIsShow(false), [])}
        ref={inputRef}
      />
      // 생략
    </SectionBody>
  );
};

export default FilterTag;
```

다른 코드가 아닌 Input 내의 코드에 집중하자. Input 컴포넌트로 전달되는 props 중 onFocus와 onBlur의 콜백 함수에 useCallback 함수를 적용시켜 보았다.

![화면 기록 2022-03-26 00 10 49](https://user-images.githubusercontent.com/43867711/160148157-eaffebe6-0d03-47ee-9712-3e8924ef87eb.gif)

React Developer Tools 크롬 익스텐션으로 확인했을 때 다른 컴포넌트들이 리렌더링 되어도 Input 창은 리렌더링되지 않는 것을 확인할 수 있다.

## ✔️ useMemo와 useCallback의 차이

위에서 useMemo는 메모이제이션된 값을 반환한다고 했고, useCallback은 메모이제이션된 콜백 함수를 반환한다고 했다. 무슨 소리인지 잘 와닿지 않으므로 더 자세히 알아보자.

```javascript
import { useState, useCallback, useMemo } from "react";

export default function App() {
  const [ex, setEx] = useState(0);
  const [why, setWhy] = useState(0);

  useMemo(() => {
    console.log("ex: ", ex);
  }, [ex]);

  useMemo(() => {
    console.log("why: ", why);
  }, [why]);

  return (
    <>
      <button onClick={() => setEx((curr) => curr + 1)}>X</button>
      <button onClick={() => setWhy((curr2) => curr2 + 1)}>Y</button>
      <h1>this is ex: {ex}</h1>
      <h1>this is why: {why}</h1>
    </>
  );
}
```

useMemo 내부에 작성한 ex와 why를 출력하는 함수는 의존성 배열을 보고 의존성 배열 안의 값이 바뀌었을 경우에만 값을 리턴한다. 위 코드 예시에서는 값이 변경될 때마다 return 값으로 `console.log()`를 실행한다. ex이 바뀌었을 때는 `console.log("ex: ", ex)`를 실행하고, why가 바뀌었을 때는 `console.log("why: ", why)`만을 실행한다. 만일 useMemo를 사용하지 않았더라면 ex만 바뀌었을 때도 ex와 why 둘 다 출력되었을 것이다.

```javascript
import "./styles.css";
import { useState, useCallback } from "react";

export default function App() {
  const [ex, setEx] = useState(0);
  const [why, setWhy] = useState(0);

  const useCallbackReturn = useCallback(() => {
    console.log(why);
  }, [ex]);

  useCallbackReturn();

  return (
    <>
      <button onClick={() => setEx((curr) => curr + 1)}>X</button>

      <button onClick={() => setWhy((curr2) => curr2 + 1)}>Y</button>
      <h1>this is ex: {ex}</h1>
      <h1>this is why: {why}</h1>
    </>
  );
}
```

![화면 기록 2022-03-26 00 25 40](https://user-images.githubusercontent.com/43867711/160151354-f2ff9a30-8395-46b2-9c49-78b7f273e58e.gif)

---

useCallback은 어떨까. useCallback은 함수를 반환한다고 했다. 따라서 위 예제에서 `() => console.log(why)`가 반환되어 `useCallbackReturn`에 할당된다. 의존성 배열에는 ex가 있으므로, ex가 바뀔 때 새로운 함수가 반환된다.

![화면 기록 2022-03-26 00 35 58](https://user-images.githubusercontent.com/43867711/160152798-f1312c78-f972-4e49-88d4-b9f264b2ee10.gif)

따라서 Y 버튼을 눌러 why의 값을 변경시켜도 콘솔 창에는 0만 출력되는 이유가 그것이다. X 버튼을 눌렀을 때 그제서야 `() => console.log(why)`가 다시 반환되어 콘솔 창에 why의 값인 4가 출력된다.

### 🚩 참고

[이제는 사용해보자 useMemo & useCallback](https://leehwarang.github.io/2020/05/02/useMemo&useCallback.html)  
[useCallback 과 useMemo 의 차이](https://basemenks.tistory.com/238)
