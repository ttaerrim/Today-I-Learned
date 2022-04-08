# React Hook 동작 원리

기본적으로 Hooks는 UI의 상태 관련 동작이나 부수 효과(side effect)를 캡슐화하는 가장 간단한 방법이다. Hook은 함수 기반으로 설계되어 있는데, 이를 이해하려면 JavaScript의 클로저에 대한 선행 지식이 있어야 한다.

Hook을 쓰게 된다면 초심자가 JavaScript의 this에 대해 걱정하지 않아도 된다고 [Mark Dalgleis](https://twitter.com/markdalgleish/status/1095025468367990784)는 말한다. 그러나 우리는 Closer에 대해 알아야 한다....

> _클로저는 함수가 속한 렉시컬 스코프를 기억하여 함수가 렉시컬 스코프 밖에서 실행될 때에도 이 스코프에 접근할 수 있게 하는 기능을 뜻한다._

## ✔️ useState

```javascript
function useState(initialValue) {
  var _val = initialValue; // _val은 useState에 의해 만들어진 지역 변수
  function state() {
    // state는 내부 함수이자 클로저입니다.
    return _val; // state()는 부모 함수에 정의된 _val을 참조합니다.
  }
  function setState(newVal) {
    // 마찬가지
    _val = newVal; // _val를 노출하지 않고 _val를 변경합니다.
  }
  return [state, setState]; // 외부에서 사용하기 위해 함수들을 노출
}
var [foo, setFoo] = useState(0); // 배열 구조 분해 사용
console.log(foo()); // 0 출력 - 위에서 넘긴 initialValue
setFoo(1); // useState의 스코프 내부에 있는 _val를 변경합니다.
console.log(foo()); // 1 출력 - 동일한 호출하지만 새로운 initialValue
```

다음과 같이 리액트의 setState의 형태를 클로저의 개념을 사용해 구현해 볼 수 있다.

여기서 중요한 것은 우리가 `foo`와 `setFoo`를 사용해 내부 변수인 `_val`에 접근하고 조작할 수 있다는 것이다. 이 둘이 클로저라는 것을 기억하면 된다.

```javascript
// 예제 0, 다시보기 - 버그 있음 주의!
function useState(initialValue) {
  var _val = initialValue;
  // state() 함수 없음
  function setState(newVal) {
    _val = newVal;
  }
  return [_val, setState]; // _val를 그대로 노출
}
var [foo, setFoo] = useState(0);
console.log(foo); // 함수 호출 할 필요 없이 0 출력
setFoo(1); // useState의 스코프 내부에 있는 _val를 변경합니다.
console.log(foo); // 0 출력 - 헐!!
```

리액트에서의 setState를 생각해 보면 return 된 state는 함수가 아닌 변수여야 한다. 이를 위해 위처럼 수정하였으나, useState에서 불러온 `foo`는 `_val`을 참조한 뒤 다시 변경되지 않는다!

이는 [모듈 패턴](https://www.patterns.dev/posts/classic-design-patterns/#modulepatternjavascript)을 사용하여 해결할 수 있다.

```javascript
// 예제 2
const MyReact = (function () {
  let _val; // 모듈 스코프 안에 state를 잡아놓습니다.
  return {
    render(Component) {
      const Comp = Component();
      Comp.render();
      return Comp;
    },
    useState(initialValue) {
      _val = _val || initialValue; // 매 실행마다 새로 할당됩니다.
      function setState(newVal) {
        _val = newVal;
      }
      return [_val, setState];
    },
  };
})();
```

이렇게 함으로써 `MyReact`는 함수형 컴포넌트를 render 할 수 있고, 클로저를 통해 얻고자 했던 내부의 변수 `_val` 값을 올바르게 얻어낼 수 있다.

```javascript
// 예제 2로 부터 이어짐
function Counter() {
  const [count, setCount] = MyReact.useState(0);
  return {
    click: () => setCount(count + 1),
    render: () => console.log("render:", { count }),
  };
}
let App;
App = MyReact.render(Counter); // render: { count: 0 }
App.click();
App = MyReact.render(Counter); // render: { count: 1 }
```
