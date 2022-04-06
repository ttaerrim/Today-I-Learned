# Context API

React에서 컴포넌트가 데이터를 다루는 방법은 Props, State, **Context**가 있습니다.

Context API는 리액트 내에서 제공하는 전역 상태 관리 방법입니다.

보통 Props와 State는 한 컴포넌트 내에서 데이터를 다룰 수 있고, 단방향으로 데이터를 전달하기 때문에 상위에서 하위 컴포넌트로 데이터가 흐르게 됩니다.

<img src="https://dev-yakuza.posstree.com/assets/images/category/react/2021/context-api/global-data-use-props.png"/>

G 컴포넌트에서 J 컴포넌트처럼 어떤 한 컴포넌트에서 복잡하게 연결되어 있는 컴포넌트로 데이터를 전달해서 사용하고 싶다면 꽤나 까다롭겠죠. 부모 컴포넌트에서 이를 거치는 모든 하위 컴포넌트에 props를 전달하는 형태로 데이터 구조를 설계해야 할 테고 props drilling을 피할 수 없을 것입니다. 이와 같은 문제를 해결하기 위해서 리액트에서는 Flux라는 개념을 도입하였고, 그에 걸맞는 Context API를 제공합니다.

하지만 전역 상태 관리는 전체 코드가 전역 상태 관리로 인해 복잡해진다는 단점이 반드시 따르게 됩니다.

단위 테스트를 할 때 단위 테스트에 필요한 요소들을 Context Provider로 래핑해야 하는 등 까다로워지는 부분은 언제나 있으니 Context를 사용하기 전에 정말로 전역 상태 관리를 하는 것이 효율적인지 따져보고 사용해야 할 필요가 있겠습니다.

Context API는 위와 같은 흐름을 따를 필요 없이 전역적으로 데이터를 관리하고 다룰 수 있습니다. 전역 데이터를 Context에 저장하고, 데이터가 필요한 컴포넌트에서 데이터를 불러와 사용할 수 있는 방식입니다.

<img src="https://dev-yakuza.posstree.com/assets/images/category/react/2021/context-api/context.png"/>

## ✔️ Context API 사용 방법

React에서 제공하는 Context를 사용하려면 Context API를 사용해야 하고, Context의 Provider와 Consumer를 사용해야 합니다.

### 1. context 생성하기

우선적으로 Context를 생성해서 Context 인스턴스를 생성합니다. 이는 `createContext` 메소드를 통해 만들 수 있습니다.

```javascript
import { createContext } from "react";
const Context = createContext("Default Value");
```

Context를 만들 때 기본값을 설정할 수도 있습니다.

### 2. context 제공하기

Context에 저장된 데이터를 사용하려면 공통된 부모 컴포넌트에 Context의 Provider를 사용해서 데이터를 제공해야 합니다.

Context 값을 설정하려면 value를 통해 전달해 줄 Context를 설정해 줄 수 있습니다.

```javascript
function Main() {
  const value = "My Context Value";
  return (
    <Context.Provider value={value}>
      <MyComponent />
    </Context.Provider>
  );
}
```

그리고 Context를 사용하고 싶은 컴포넌트는 반드시 이 Provider로 감싸 주어야 합니다.

Context를 변경하려면 value를 업데이트해서 변경할 수 있습니다.

### 3. context 사용하기

데이터를 사용하려는 컴포넌트에서 Consumer나 useContext 훅을 사용하여 데이터를 불러와 사용할 수 있습니다.

개인적으로는 useContext 훅을 사용하는 것이 더 편리하다고 느껴졌습니다.

```javascript
import { useContext } from "react";
function MyComponent() {
  const { value } = useContext(Context);
  return <span>{value}</span>;
}
```

useContext 훅은 Context 생성 시 전달해 줬던 value 값을 반환합니다. 또한 Context 값이 변경됐을 때 state나 props가 변경됐을 때와 마찬가지로 컴포넌트를 리렌더링합니다.

Context.Consumer를 사용하여 데이터를 사용할 수도 있습니다.

```javascript
function MyComponent() {
  return <Context.Consumer>{(value) => <span>{value}</span>}</Context.Consumer>;
}
```

### 🚩 참고

[Context](https://ko.reactjs.org/docs/context.html)

[A Guide to React Context and useContext() Hook](https://dmitripavlutin.com/react-context-and-usecontext/)
