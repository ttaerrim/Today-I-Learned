# `connect()`

## ✔️ connect란?

`connect()` 함수는 리액트 컴포넌트와 리덕스 스토어를 연결해 준다. 스토어에 필요한 데이터와 액션을 디스패치하는 함수들을 연결하도록 도와준다.

```javascript
function connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)
```

`mapStateToProps`는 state를 설정할 수 있고, `mapDispatchToProps`는 dispatch할 action을 설정할 수 있다. 설정해 주면 다음과 같이 props를 통해 받아와 사용할 수 있다.

예시 1

```javascript
import React from "react";
import { connect } from "react-redux";
import Counter from "../components/Counter";
import { increase, decrease, setDiff } from "../modules/counter";

function CounterContainer({ number, diff, increase, decrease, setDiff }) {
  return (
    <Counter
      number={number}
      diff={diff}
      onIncrease={increase}
      onDecrease={decrease}
      onSetDiff={setDiff}
    />
  );
}

const mapStateToProps = (state) => ({
  number: state.counter.number,
  diff: state.counter.diff,
});

const mapDispatchToProps = {
  increase,
  decrease,
  setDiff,
};

export default connect(mapStateToProps, mapDispatchToProps)(CounterContainer);
```

<br/>

예시2

```javascript
function View(props) {
  const userinfo = props.userinfo;

  return (
    <div className={"layout"}>
      <p>{userinfo}</p>
    </div>
  );
}

export default connect((state) => ({ userinfo: state.userinfo }))(View);
```

## useSelector vs connect

이는 React hooks의 `useSelector()`와 같은 결과를 만들 수 있다.

예시 1을 useSelector와 useDispatch를 사용하면 다음과 같이 코드를 간소화할 수 있다.

```javascript
import React from "react";
import { useDispatch, useSelector } from "react-redux";
import Counter from "../components/Counter";

function CounterContainer({ number, diff, increase, decrease, setDiff }) {
  const number = useSelector((state) => state.counter.number);
  const diff = useSelector((state) => state.counter.diff);
  const dispatch = useDispatch();
  return (
    <Counter
      number={number}
      diff={diff}
      onIncrease={dispatch(increase)}
      onDecrease={dispatch(decrease)}
      onSetDiff={dispatch(setDiff)}
    />
  );
}

export default CounterContainer;
```
