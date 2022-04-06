# FLUX 패턴

## ✔️ Flux란?

Flux는 페이스북에서 발표한 클라이언트-사이드 웹 애플리케이션을 만들기 위해 사용하는 애플리케이션 아키텍쳐이다.

전통적인 MVC 패턴을 버리고 개발이 오래되고 커져도 유지 보수가 쉽고 reliable하고 high performance를 유지할 수 있는 새로운 패턴을 제시했다!

단방향 데이터 흐름을 활용하여 뷰 컴포넌트를 구성하는 React를 보완하는 역할을 한다.

## ✔️ MVC의 문제점

Flux 패턴에 대해 자세히 알아보기 전에, MVC의 문제점이 무엇인지 알아야 flux 패턴에 대한 이해가 더 쉬울 것 같다.

우선 프로젝트 규모가 커질수록 복잡해진다는 점이 있다.

<img src="https://blog.kakaocdn.net/dn/ALrHe/btqBTMSuHfN/ZlW9i9ET34e90APgCRChk1/img.png"/>

MVC 패턴은 양방향 데이터 흐름 구조를 가지고 있다.

Controller는 Model의 데이터를 조회하거나 업데이트하는 역할을 한다. Model이 업데이트되면 View가 이를 화면에 반영한다. View가 Model을 업데이트할 수도 있다. 이 과정에서 Model이 업데이트되면 View가 업데이트되고, 업데이트된 View가 또 다른 Model을 업데이트하면 또 다른 View가 업데이트되어 위 사진과 같은 구조를 낳는 것이다.

이러한 데이터 흐름이 프로젝트가 커질수록 기하급수적으로 증가하는 복잡도를 낳을 수 있고 예측 불가능한 코드가 만들어지게 된다.

페이스북의 알림 버그 중 메시지가 있다는 알림을 보고 메시지 창에 들어가면 정작 받은 메시지는 없는 버그가 있었다고 한다. 페이스북 측에서는 이 버그의 원인을 양방향 데이터 흐름으로 보고, 단방향 데이터 흐름인 Flux를 도입하게 되었다고 한다.

## ✔️ Flux

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlmfPW%2FbtqBQnTPgIs%2FZ1jmHHdNcOTNiu93kQ9gMk%2Fimg.png"/>

단방향 데이터 흐름을 갖는 Flux는 dispatcher에서 store, store에서 view, view는 action을 통해 dispatcher로 데이터가 흐르는 구조를 가지고 있다. 이런 단방향 데이터 흐름은 데이터 변화를 예측하기 쉽게 만든다. Flux는 이렇게 크게 Dispatcher, Store, View(React 컴포넌트) 세 부분으로 구성된다.

React View에서는 사용자가 상호작용 할 때 View가 새로운 action을 생성해 Dispatcher에게 action을 넘겨 주게 된다. 모든 데이터는 중앙의 dispatcher를 통해 흐른다. dispatcher가 받은 action을 store에게 전달하면 store는 이 action을 받아 action과 관련 있는 모든 View를 갱신한다.

### 🚩 참고

[Flux](https://haruair.github.io/flux/docs/overview.html)  
[[디자인패턴] Flux, MVC 비교](https://beomy.tistory.com/44)
