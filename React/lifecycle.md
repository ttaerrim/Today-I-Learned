# React의 Life Cycle

## ✔️ 라이프 사이클이란?

Life Cycle Method는 한국어로 생명 주기 메서드라고 부른다. 브라우저에 컴포넌트가 나타나고, 업데이트되고 사라질 때 호출되는 메서드들이며 클래스형 컴포넌트에서만 사용된다. 함수형 컴포넌트에서는 useEffect 같은 Hooks가 사용된다.

## ❓ 라이프 사이클 종류

<img width="600" alt="스크린샷 2022-03-23 02 47 27" src="https://user-images.githubusercontent.com/43867711/159543512-9bdff3f8-3f6a-44d8-b63a-ad9ec59992eb.png"/>

### 마운트될 때 발생하는 생명 주기 메서드들

- ### constructor
  컴포넌트의 생성자 메서드로, 컴포넌트가 만들어지면 가장 먼저 실행된다.
  ```javascript
  constructor(props) {
      super(props);
      this.state = {}
  }
  ```
- ### getDerivedStateFromProps
  props로 받아온 것을 state에 넣을 때 사용한다.
  ```javascript
  static getDerivedStateFromProps(nextProps, prevState) {
      console.log("getDerivedStateFromProps")
      if (nextProps.color !== prevState.color) {
          return { color: nextProps.color }
      }
      return null
  }
  ```
  맨 앞의 `static` 키워드를 필요로 하고, `getDerivedStateFromProps` 내에서는 `this`를 조회할 수 없다. 특정 객체를 반환하게 되면 해당 객체 안의 내용이 컴포넌트의 state로 설정된다. `null`을 반환한다면 아무 일도 일어나지 않는다.
  컴포넌트가 처음 렌더링 되기 전에도 호출되고, 이후에 리렌더링 되기 전에도 매번 실행된다.
- ### render
  컴포넌트를 렌더링하는 메서드이다.
- ### componentDidMount
  컴포넌트의 첫 번째 렌더링을 마치고 나면 호출되는 메서드이다. 이 메서드가 호출되는 시점은 우리가 만든 컴포넌트가 화면에 나타난 상태이다. 여기서는 DOM을 사용하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch 등을 통해 ajax 요청을 하거나, DOM의 속성을 읽거나 직접 변경하는 작업을 수행한다.

### 컴포넌트가 업데이트될 때 호출되는 생명 주기 메서드들

- ### getDerivedStateFromProps
  `getDerivedStateFromProps`는 컴포넌트의 `props`나 `state`가 바뀌었을 때에도 호출된다.
- ### shouldComponentUpdate
  `shouldComponentUpdate` 메서드는 컴포넌트가 리렌더링을 할 것인지 말 것인지를 결정하는 메서드이다.
  ```javascript
  shouldComponentUpdate(nextProps, nextState) {
      console.log('shouldComponentUpdate', nextProps, nextState);
      return nextState.number % 10 !== 4;
  }
  ```
  주로 최적화할 때 사용한다. 함수형 컴포넌트에서 사용하는 `React.memo`와 비슷하다.
- ### render
- ### getSnapshotBeforeUpdate
  `getSnapshotBeforeUpdate`는 컴포넌트에 변화가 일어나기 직전 DOM 상태를 가져와 특정 값을 반환하면 그 다음 발생하게 되는 `componentDidUpdate` 함수에서 받아와 사용할 수 있다.
  ```javascript
  getSnapshotBeforeUpdate(prevProps, prevState) {
      console.log('getSnapshotBeforeUpdate')
      if (prevProps.color !== this.props.color) {
          return this.myRef.style.color;
      }
      return null;
  }
  ```
  함수형 컴포넌트와 Hooks를 사용할 때는 `getSnapshotBeforeUpdate`를 대체할 수 없는 것이 아직 없다. DOM에 변화가 반영되기 직전 DOM의 속성을 확인하고 싶을 때 사용하면 된다는 것만 기억해 두자.
- ### componentDidUpdate

  `componentDidUpdate`는 리렌더링을 마치고 화면에 변화가 모두 반영되고 난 뒤 호출되는 메서드이다. 세 번째 파라미터로 `getSnapshotBeforeUpdate`에서 반환한 값을 조회할 수 있다.

  ```javascript
  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log("componentDidUpdate", prevProps, prevState);
    if (snapshot) {
      console.log('업데이트 되기 직전 색상: ', snapshot);
    }
  }
  ```

### 컴포넌트가 언마운트(화면에서 사라짐)될 때 호출되는 생명 주기 메서드

- ### componentWillUnmount
  `componentWillUnmount`는 화면에서 컴포넌트가 사라지기 직전에 호출된다. 주로 DOM에 직접 등록했었던 이벤트를 제거한다. 예를 들어 setTimeout을 등록했다면 clearTimeout을 통해 제거한다. 외부 라이브러리를 사용했을 때, 해당 라이브러리에 dispose 기능이 있다면 `componentWillUnmount`에서 호출하면 된다.
  ```javascript
  componentWillUnmount() {
    console.log("componentWillUnmount")
  }
  ```

### 참고

[react lifecycle method diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)  
[25. LifeCycle Method](https://react.vlpt.us/basic/25-lifecycle.html)
