# useReducer

`useReducer`는 컴포넌트에 리듀서를 추가할 수 있도록 하는 리액트 훅이다.

```
import { useReducer } from 'react';

function reducer(state, action) {
  // ...
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
}
```

리덕스를 사용해 본 사람이라면 리덕스의 리듀서, 액션과 같은 형태를 가지고 있기 때문에 익숙할 것임.

## Parameters

#### `useReducer(reducer, initialArg, init?)`

- `reducer`: 현재 상태와 액션을 받아 새로운 상태를 반환하는 함수
- `initialArg`: 초기 상태로 사용할 값
- `optional init`: 선택적으로 사용할 수 있는 초기화 함수. 이 함수를 사용하면 초기 상태를 계산하는 데 비동기 로직을 수행할 수 있음.

`useReducer`를 호출하면 배열이 반환되는데, 이 배열의 첫 번째 요소는 현재 상태이고, 두 번째 요소는 상태를 업데이트하는 `dispatch` 함수이다. `dispatch` 함수를 사용하여 액션을 전달하면 `reducer` 함수가 호출되고 새로운 상태를 반환한다.

`useState`의 사용이 많아지는 경우 `useReducer`를 사용하면 액션에 기반하여 상태를 업데이트할 수 있으며, 복잡한 상태 관리 로직을 더 쉽게 구현할 수 있다.

### useState를 useReducer로 개선하기

주 목적은 너무 많아진 props의 개수를 줄이는 것임.

Before

```
export default function Container() {
  const [userStatus, setUserStatus] = useState(USER_STATUS.ALL);
  const [dateSearchType, setDateSearchType] = useState(undefined);
  const [startDate, setStartDate] = useState(null);
  const [endDate, setEndDate] = useState(null);

  const [searchType, setSearchType] = useState(undefined);
  const [searchValue, setSearchValue] = useState("");

  const [startAge, setStartAge] = useState(null);
  const [endAge, setEndAge] = useState(null);

  const [gender, setGender] = useState("전체");

  return (
    <View
      {...생략}
      userStatus={userStatus}
      setUserStatus={setUserStatus}
      dateSearchType={dateSearchType}
      setDateSearchType={setDateSearchType}
      startDate={startDate}
      setStartDate={setStartDate}
      endDate={endDate}
      setEndDate={setEndDate}
      searchType={searchType}
      setSearchType={setSearchType}
      searchValue={searchValue}
      setSearchValue={setSearchValue}
      startAge={startAge}
      setStartAge={setStartAge}
      endAge={endAge}
      setEndAge={setEndAge}
      gender={gender}
      setGender={setGender}
    />
  )
}


```

After

```
export default function Container() {

  const initialState = {
    userStatus: USER_STATUS.ALL,
    dateSearchType: undefined,
    startDate: null,
    endDate: null,
    searchType: undefined,
    searchValue: "",
    startAge: null,
    endAge: null,
    gender: GENDER.ALL,
    filter: "",
  };

  const setUserStatus = (userStatus) => ({
    type: "SET_USER_STATUS",
    payload: userStatus,
  });
  const setDateSearchType = (dateSearchType) => ({
    type: "SET_DATE_SEARCH_TYPE",
    payload: dateSearchType,
  });
  const setStartDate = (startDate) => ({
    type: "SET_START_DATE",
    payload: startDate,
  });
  const setEndDate = (endDate) => ({
    type: "SET_END_DATE",
    payload: endDate,
  });
  const setSearchType = (searchType) => ({
    type: "SET_SEARCH_TYPE",
    payload: searchType,
  });
  const setSearchValue = (searchValue) => ({
    type: "SET_SEARCH_VALUE",
    payload: searchValue,
  });
  const setStartAge = (payload) => ({
    type: "SET_START_AGE",
    payload,
  });
  const setEndAge = (payload) => ({
    type: "SET_END_AGE",
    payload,
  });
  const setGender = (payload) => ({
    type: "SET_GENDER",
    payload,
  });
  const setFilter = (payload) => ({
    type: "SET_FILTER",
    payload,
  });

  const setInitial = () => ({ type: "SET_INITIAL" });

  const [searchState, searchDispatch] = useReducer(searchReducer, initialState);

  function searchReducer(state, action) {
    switch (action.type) {
      case "SET_USER_STATUS":
        return { ...state, userStatus: action.payload };
      case "SET_DATE_SEARCH_TYPE":
        return { ...state, dateSearchType: action.payload };
      case "SET_START_DATE":
        return { ...state, startDate: action.payload };
      case "SET_END_DATE":
        return { ...state, endDate: action.payload };
      case "SET_SEARCH_TYPE":
        return { ...state, searchType: action.payload };
      case "SET_SEARCH_VALUE":
        return { ...state, searchValue: action.payload };
      case "SET_START_AGE":
        return { ...state, startAge: action.payload };
      case "SET_END_AGE":
        return { ...state, endAge: action.payload };
      case "SET_GENDER":
        return { ...state, gender: action.payload };
      case "SET_FILTER":
        return { ...state, filter: action.payload };
      case "SET_INITIAL":
        return initialState;
      default:
        return state;
    }
  }

  const {
    userStatus,
    dateSearchType,
    startDate,
    endDate,
    searchType,
    searchValue,
    startAge,
    endAge,
    gender,
  } = searchState;

  return (
    <View
      {...생략}
      searchState={searchState}
      searchDispatch={searchDispatch}
    />
  );
}
```

props를 `searchState`와 `searchDispatch`만 전달해 18개 -> 2개로 줄이고, actions들은 컴포넌트 내에 폴더 분리해 선언해 필요한 action만 import하여 사용하는 방식으로 변경함.

### 참고

- https://react.dev/reference/react/useReducer
- https://www.zigae.com/state-vs-reducer/
