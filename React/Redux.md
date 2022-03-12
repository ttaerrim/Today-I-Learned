# Redux

## ✔️ Redux란?

Redux는 자바스크립트 상태 관리 라이브러리이다.  
상태(state)를 효율적으로 관리할 수 있도록 도와준다.

## ✔️ Redux 기본 개념

![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa12ec593-6cc1-4d76-b823-b37c33942037%2FUntitled.png?table=block&id=6db8bae1-d99b-46ae-a76b-a6636c9125fc&spaceId=4b97eaca-7938-4c43-b27c-a0c55795a841&width=1270&userId=14572033-9587-4bc1-9c98-d7b204dd577f&cache=v2)

### 1. 액션(Action)

상태를 변경하기 위해서는 액션을 통해 변경해야 한다. 액션은 객체 형식으로 표현되고, `type`을 **필수**로 가지고 있어야 한다. 옵션으로 다른 프로퍼티를 가질 수 있고, 액션에 필요한 부가적인 데이터를 전달할 때 주로 사용한다.

```javascript
{
    type: 'ADD_MESSAGE',
    data: {
        id: 1
        message: "리덕스 배우기"
    }
}
```

메시지를 새로 추가하는 액션이고, 새로운 메시지에 대한 부가적인 data가 전달된다.

### 2. 액션 생성 함수(Action Creator)

액션은 객체 형식으로 바로 만드는 것이 아니라, 액션을 만들어 주는 **액션 생성 함수**를 통해 만든다.

```javascript
function addMessage(data) {
  return {
    type: "ADD_MESSAGE",
    data,
  };
}

// arrow function
const addMessage = (data) => ({ type: "ADD_MESSAGE", data });
```

액션 생성은 액션 생성 함수를 호출함으로써 이루어진다.

```javascript
addMessage({ id: 1, message: "리덕스 배우기" });
```

### 3. 리듀서(Reducer)

리듀서는 액션이 발생했을 때 상태(state)를 변경시키기 위한 함수이다. 리듀서는 현재 상태와 액션 객체를 받고, 새로운 상태를 반환한다.

```javascript
function message(state, action) {
    switch(action.type) {
        case 'ADD_MESSAGE':
            return {
                ...state,
                action.data
            }
        default:
            return state
    }
}
```

여기서 액션에 전달한 부가적인 데이터인 `data`는 `action.data`와 같은 형식으로 사용할 수 있다.

### 4. 스토어(Store)

하나의 앱에는 단 하나의 스토어만 존재하고, 유일한 스토어를 사용하여 앱의 전체 상태를 관리한다.
스토어 생성은 다음과 같이 한다.

```javascript
import { createStore } from "redux";
import message from "./messageReducer";

const store = createStore(message);
```

redux에서 제공하는 createStore 함수를 사용하고, 인자로 리듀서를 전달해 준다.

### 5. 디스패치(Dispatch)

디스패치는 스토어의 내장 함수 중 하나이다. 디스패치는 상태를 업데이트할 수 있는 유일한 방법이다. 액션을 리듀서에게 전달해 상태를 변경시킨다.

### 6. 구독(Subscribe)

구독은 스토어의 내장 함수 중 하나이다. 리스너 함수를 subscribe에 전달하여 호출하면 상태가 업데이트될 때마다 호출된다.

```javascript
const listener = () => {
  console.log("Update!");
};

const unsubscribe = store.subscribe(listener);
unsubscribe(); // 구독을 비활성화할 때 함수를 호출한다
```

### 7. 셀렉터(Selector)

react-redux에서 상태 값을 가져올 때 사용한다.

```javascript
const message = useSelector((state) => state.message);
```

## ✔️ Redux의 장점은 무엇이 있나요?

redux를 사용하지 않을 때는 부모 컴포넌트에서 자식 컴포넌트에 props를 전달해 주는데, 그 경우에 생기는 props drilling을 방지할 수 있다는 점.

## ✔️ Redux에서 준수해야할 3가지 원칙은?

### 1. Single source of truth

**전체 상태값이 하나의 객체로 표현된다.**  
동일한 데이터는 항상 같은 곳에서 가지고 온다 → 단 하나의 스토어가 있다는 의미

### 2. State is read-only

**state는 읽기 전용이다.**  
읽기 전용 상태를 변경하는 방법은 액션 객체뿐이다

### 3. Changes are made with pure functions

**상태의 변경은 순수 함수로 이루어져야 한다.**  
이 순수 함수\*를 reducer(리듀서)라고 한다. 리듀서는 이전 상태와 액션을 받아 다음 상태를 반환하는 순수 함수이다.  
\*순수 함수: 같은 인자가 들어오면 항상 같은 결과를 반환하는 함수

## ✔️ Redux와 비슷한 다른 것들은 써보았는지?

React의 Context API를 사용해 보았다.  
Context API와 Redux의 차이점은 아래와 같다.

### **Context API**

리액트에서 제공하는 자체 전역상태 관리 API이다.

Context API로 관리하는 상태를 사용하기 위해서는 Provider로 컴포넌트를 감싸서 사용해야 한다. 여기서 각기 다른 Context를 사용하려고 할 때 Provider를 따로 여러 번 쓰는 경우나, Provider를 중첩해서 여러 번 사용해야 하는 경우가 생긴다.

```javascript
<ItemContext.Provider value={{ items, setItems }}>
  <ShowContext.Provider value={{ isShow, setIsShow }}>
    {children}
  </ShowContext.Provider>
</ItemContext.Provider>
```

전역적으로 관리하는 상태는 `useContext`를 사용해서 꺼내 쓸 수 있다.

```javascript
const { item } = useContext(ItemContext);
```

### **Redux**

Redux로 관리하는 상태를 사용하기 위해서는 App 전체를 Provider로 감싸서 사용하는데, 여러 상태가 있더라도 store를 사용해 한 데 모을 수 있기 때문에 Provider를 한 번만 사용하면 어떤 컴포넌트에서든 모든 상태를 useSelector를 사용해 꺼내 쓸 수 있다.

## ✔️ 어떤 상황에서 Redux를 쓰면 좋고 왜 그런지?

하나의 state가 여러 컴포넌트에서 필요한 경우가 있을 때 redux를 사용하면 좋다.

redux를 사용하지 않을 경우에, 모든 컴포넌트에서 사용한다면 최상단의 컴포넌트, 예를 들어 App 컴포넌트에서 변수를 정의하고 그 변수를 모든 컴포넌트에 전달해 주어야겠지만, redux를 사용한다면 일단 그 코드에서 전달해 주는 게 줄어들고 useSelector를 사용해서 전역적으로 관리하는 변수를 사용할 수 있다.

비슷하게 규모가 큰 앱에서는 Redux를 사용하는 것이 편했다.
