# React.lazy

## ✔️ 코드 스플리팅

### 번들링

여러 개의 JS 파일을 하나의 파일로 모으는 것을 **Bundle**이라고 함.
리액트 앱은 웹팩 같은 툴을 사용하여 파일들을 번들링한다.

### 코드 스플리팅

앱이 커질수록 번들도 무거워지고 커지므로, 번들을 스플리팅하여 나눌 필요가 있다. **코드 스플리팅**은 웹팩 같은 번들러에서 지원하는 기능이다.

코드 스플리팅을 통해 현재 필요한 항목만 lazy-load 한다면 필요하지 않은 코드 로드를 방지하고 초기 로드에 필요한 코드를 줄일 수 있으므로 앱 성능을 향상시킬 수 있다.

## ✔️ React.lazy와 Suspense

`React.lazy`로 다이나믹 Import로 사용할 컴포넌트를 가져온다.

```javascript
// before
import OtherComponent from "./OtherComponent";

// after
const OtherComponent = React.lazy(() => import("./OtherComponent")));
```

그런 다음 `Suspense` 내부에서 해당 컴포넌트를 호출하면 된다.

```javascript
import React, { Suspense } from "react";

const OtherComponent = React.lazy(() => import("./OtherComponent")));
const AnotherComponent = React.lazy(() => import("./AnotherComponent")));

function App() {
    return (
        <div>
            <Suspense fallback={<div>로딩 중...</div>}>
                <OtherComponent />
                <AnotherComponent />
            </Suspense>
        </div>
    )
}

export default App;
```

Suspense의 fallback 속성으로 해당 컴포넌트를 가져오는 데 걸리는 시간 동안 렌더할 로더를 지정할 수 있다.

Router와 Suspense를 같이 사용할 수도 있다.

```javascript
import React, { Suspense, lazy } from "react";
import { BrowserRouter as Router, Route } from "react-router-dom";

const Student = lazy(() => import("./page/Student"));
const Tutor = lazy(() => import("./page/Tutor"));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route exact path="/student" component={Student} />
        <Route path="/tutor" element={<Tutor />} />
      </Routes>
    </Suspense>
  </Router>
);
```
