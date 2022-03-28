# 클로저

## ✔️ 클로저란?

**만들어진 시점의 실행 환경을 기억하는 함수**를 클로저라고 한다.

클로저 함수를 호출한 함수가 종료되더라도 호출한 함수의 환경(변수 등)을 클로저 함수가 기억하고 있는 것이다.

```javascript
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  innerFunc();
}

outerFunc(); // 10
```

함수 outerFunc 내에서 내부 함수 innerFunc이 선언되고 호출되었다.
innerFunc은 outerFunc의 내부에 선언된 변수 x에 접근할 수 있다.

내부 함수 innerFunc이 호출되면 콜 스택에 innerFunc 실행 컨텍스트가 쌓이고 변수 객체(Variable Object)와 스코프 체인(Scope Chain), this에 바인딩할 객체가 결정된다. 스코프 체인은 전역 객체, outerFunc 함수의 활성 객체, 자신인 innerFunc 함수의 활성 객체를 바인딩한다. 바인딩한 객체가 렉시컬 스코프의 실체이다.

이 스코프 체인을 따라 자바스크립트 엔진이 변수 x를 탐색했기 때문에 innerFunc 함수가 outerFunc 내부에 있는 변수 x를 접근할 수 있다.

innerFunc을 반환할 경우는 어떨까?

```javascript
function outerFunc() {
  var x = 10;
  var innerFunc = function () {
    console.log(x);
  };
  return innerFunc;
}

var inner = outerFunc();
inner(); // 10
```

outerFunc 함수는 내부 함수 innerFunc를 반환하고 소멸된다. 실행 컨텍스트가 동작하는 방식대로 살펴보면 outerFunc이 호출되고 난 뒤에는 콜 스택에서 outerFunc 실행 컨텍스트가 사라져 변수 x에 접근할 수 없을 것이다. 그렇지만 `inner()`를 실행했을 때 변수 x의 값이 출력된다.

이처럼 외부 함수 밖에서 내부 함수가 호출되더라도 외부 함수의 지역 변수에 접근할 수 있는데 이런 함수를 **클로저(Closure)**라고 부른다.

클로저는 반환된 내부 함수가 자신이 선언됐을 때의 환경(렉시컬 환경)의 스코프를 기억해 자신이 선언됐을 때 환경(스코프) 밖에서 호출되어도 그 환경(스코프)에 접근할 수 있는 함수를 말한다.

### → 클로저란 `자신이 생성될 때의 환경(렉시컬 환경)을 기억하는 함수`

실행 컨텍스트의 관점에서 설명하자면, 외부 함수가 종료되어 외부 함수의 실행 컨텍스트가 반환되어도 내부 함수가 유효하다면 외부 함수 실행 컨텍스트 내의 활성 객체(변수, 함수 선언 등의 정보를 가지고 있음)는 내부 함수에 의해 참조되는 한 유효하여 내부 함수가 스코프 체인을 통해 참조할 수 있는 것을 의미한다.

즉, 외부 함수가 반환되었어도 외부 함수 내의 변수는 이를 사용하는 내부 함수가 있을 경우 계속 유지된다. 이 변수는 복사본도 아니고 실제 변수이다.

## ✔️ 클로저의 활용

### 🚩 참고

[클로저](https://poiemaweb.com/js-closure)
