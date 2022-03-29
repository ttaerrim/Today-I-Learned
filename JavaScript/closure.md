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

### 1. 상태 유지

클로저가 가장 유용하게 사용되는 상황은 **현재 상태를 기억하고 변경된 최신 상태를 유지**하는 것이다.

```javascript
<!DOCTYPE html>
<html>
<body>
  <button class="toggle">toggle</button>
  <div class="box" style="width: 100px; height: 100px; background: red;"></div>

  <script>
    var box = document.querySelector('.box');
    var toggleBtn = document.querySelector('.toggle');

    var toggle = (function () {
      var isShow = false;

      // ① 클로저를 반환
      return function () {
        box.style.display = isShow ? 'block' : 'none';
        // ③ 상태 변경
        isShow = !isShow;
      };
    })();

    // ② 이벤트 프로퍼티에 클로저를 할당
    toggleBtn.onclick = toggle;
  </script>
</body>
</html>
```

![화면 기록 2022-03-29 16 52 46](https://user-images.githubusercontent.com/43867711/160561892-4b65266e-90ff-445a-9b98-38736a0ba1df.gif)

① 즉시실행함수는 함수를 반환하고 즉시 소멸한다. 즉시실행함수가 반환한 함수는 자신이 생성됐을 때의 렉시컬 환경에 속한 변수 isShow를 기억하는 클로저이다. 클로저가 기억하는 변수 isShow는 box 요소의 display 상태를 나타낸다.

② 클로저를 이벤트 핸들러로서 이벤트 프로퍼티에 할당한다. 이벤트 프로퍼티에서 이벤트 핸들러인 클로저를 제거하지 않는 한 클로저가 기억하는 렉시컬 환경의 변수 isShow는 사라지지 않는다. 다시 말해 현재 상태를 기억하고 있다.

③ 버튼을 클릭하면 이벤트 프로퍼티에 할당한 이벤트 핸들러인 클로저가 호출된다. 이때 .box 요소의 표시 상태를 나타내는 변수 isShow의 값이 변경된다. 변수 isShow는 클로저에 의해 참조되기 때문에 유효하고 자신의 변경된 최신 상태를 계속해서 유지한다.

이처럼 클로저는 현재 상태를 기억하고 이 상태가 변경되어도 최신 상태를 유지해야 하는 상황에 매우 유용하다. 클로저 기능이 없다면 상태를 유지하기 위해 전역 변수를 사용할 수밖에 없는데 전역 변수의 사용은 많은 사이드 이펙트를 유발하기 때문에 오류의 원인이 되므로 사용을 억제해야 한다.

### 2. 전역 변수의 사용 억제

```javascript
<!DOCTYPE html>
<html>
<body>
  <p>전역 변수를 사용한 Counting</p>
  <button id="inclease">+</button>
  <p id="count">0</p>
  <script>
    var incleaseBtn = document.getElementById('inclease');
    var count = document.getElementById('count');

    // 카운트 상태를 유지하기 위한 전역 변수
    var counter = 0;

    function increase() {
      return ++counter;
    }

    incleaseBtn.onclick = function () {
      count.innerHTML = increase();
    };
  </script>
</body>
</html>
```

![화면 기록 2022-03-29 17 00 48](https://user-images.githubusercontent.com/43867711/160563133-ee9d313b-4c8a-4942-8afd-079cedf88f43.gif)

위 코드는 자라 동작하지만 전역 변수를 사용했기 때문에 오류가 발생할 가능성이 있다. 전역 변수이기 때문에 언제든지 누구나 접근해 변경할 수 있기 때문이다. 이 전역 변수 counter를 increase 함수의 지역 변수로 바꿔 의도치 않은 상태 변경을 방지해 보자.

```javascript
// script만 작성
var incleaseBtn = document.getElementById("inclease");
var count = document.getElementById("count");

function increase() {
  // 카운트 상태를 유지하기 위한 지역 변수
  var counter = 0;
  return ++counter;
}

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

위 코드는 전역 변수를 지역 변수로 바꿨지만 increase가 호출될 때마다 counter가 0으로 초기화되기 때문에 **변경된 이전 상태를 기억하지 못한다**.

```javascript
// script만 작성
var incleaseBtn = document.getElementById("inclease");
var count = document.getElementById("count");

var increase = (function () {
  var counter = 0;
  return function () {
    return ++counter;
  };
})();

incleaseBtn.onclick = function () {
  count.innerHTML = increase();
};
```

스크립트가 실행되면 즉시실행함수가 호출되고 변수 increase에는 함수 `function () { return ++counter; };`이 할당된다. 이 함수는 자신이 생성되었을 때의 렉시컬 환경을 기억하는 클로저이다. 즉시실행함수는 반환 후 소멸되지만 즉시실행함수가 반환한 함수는 increase에 할당되어 + 버튼을 클릭하면 클릭 이벤트 핸들러 내부에서 호출된다. 이때 클로저인 이 함수는 렉시컬 환경에 속한 지역변수 counter를 기억하고 자신을 참조하는 함수가 소멸될 때까지 counter는 유지된다.

> 변수의 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있다. 상태 변경이나 가변(mutable) 데이터를 피하고 불변성(Immutability)을 지향하는 함수형 프로그래밍에서 부수 효과(Side effect)를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용된다.

### 3. 정보의 은닉

```javascript
function Counter() {
  // 카운트를 유지하기 위한 자유 변수
  var counter = 0;

  // 클로저
  this.increase = function () {
    return ++counter;
  };

  // 클로저
  this.decrease = function () {
    return --counter;
  };
}

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.decrease()); // 0
```

생성자 함수 Counter를 만들고 이를 통해 counter 객체를 만들었다.

생성자 함수 Counter는 increase, decrease 메소드를 가지는 인스턴스를 생성한다. 이 메소드들은 모두 자신이 생성됐을 때의 렉시컬 환경인 생성자 함수 Counter의 스코프에 속한 변수 counter를 기억하는 클로저이고 렉시컬 환경을 공유한다. 생성자 함수가 생성한 객체의 메소드는 객체의 프로퍼티에만 접근할 수 있는 것이 아니고 자신이 기억하는 렉시컬 환경의 변수에도 접근할 수 있다.

이때 생성자 함수 Counter의 변수 counter는 this에 바인딩된 프로퍼티가 아니고 변수이다. counter가 this에 바인딩된 프로퍼티라면 생성자 함수 Counter는 생성한 인스턴스를 통해 외부에서 접근이 가능한 `public` 프로퍼티가 된다. 하지만 Counter 내의 변수 counter는 생성자 함수 Counter 외부에서 접근할 수 없다. 하지만 인스턴스의 메소드인 increase, decrase는 클로저라서 자신이 생성됐을 때의 렉시컬 환경인 생성자 함수 Counter의 변수 counter에 접근할 수 있다. **이러한 클로저의 특징을 사용해 클래스 기반 언어의 `private` 키워드를 흉내낼 수 있다**.

### 🚩 참고

[클로저](https://poiemaweb.com/js-closure)
