# 스코프

## ✔️ 스코프란?

스코프는 참조 대상 식별자(identifier, 변수, 함수의 이름과 같이 어떤 대상을 다른 대상과 구분하여 식별할 수 있는 유일한 이름)를 찾아내기 위한 규칙이다. 쉽게 말해 변수가 어떤 위치에 있냐 찾는다고 생각하면 쉬울 것 같다.

```javascript
var x = "global";

function foo() {
  var x = "function scope";
  console.log(x);
}

foo(); // function scope
console.log(x); // global
```

위와 같이 전역 변수의 이름도 x, foo 함수 안의 변수 이름도 x지만 실행 결과는 각기 다르다.

전역에 선언된 `var x = "global"`는 어디에서나 참조할 수 있다. 하지만 foo 함수 내부에 선언된 `var x = "function scope"`는 foo 함수 내부에서만 참조할 수 있고 외부에서는 참조할 수 없다. 이런 규칙을 스코프라고 한다.

만약 스코프가 없다면 전체 프로그램 내에서 x라는 변수는 하나밖에 사용하지 못할 것이다. 컴퓨터의 폴더 구조가 없다고 생각했을 때 파일 이름은 중복되어 사용하지 못할 것이다. 스코프도 이와 비슷하다.

## ✔️ 스코프의 구분

스코프는 두 가지로 나눌 수 있다.

- 전역 스코프 (Global Scope)
  - 코드 어디에서나 참조 가능
- 지역 스코프 (Local Scope / Function-level Scope)
  - 함수 코드 블록이 만든 스코프로 함수 자신과 하위 함수에서만 참조 가능

변수의 관점에서 스코프를 구분하면 두 가지로 나눌 수 있다.

- 전역 변수 (Global variable)
  - 전역에서 선언된 변수이며 어디에서나 참조 가능
- 지역 변수 (Local variable)
  - 지역(함수) 내에서 선언된 변수이며 그 지역과 그 지역의 하위 지역에서만 참조 가능

변수는 선언 위치에 따라 스코프가 달라진다. 전역에서 선언됐으면 전역 스코프를 갖는 전역 변수이고, 지역(자바스크립트의 경우 함수 내부)에서 선언된 변수는 지역 스코프를 가지는 지역 변수가 된다.

## ✔️ 자바스크립트 스코프

자바스크립트 스코프는 **함수 레벨 스코프**를 따른다. 함수 레벨 스코프란 함수 코드 블록 내에서 선언된 변수는 함수 내부에서만 사용할 수 있고 외부에서는 참조하지 못한다는 의미이다.

ES6에서 도입된 `let` 키워드를 사용하면 **블록 레벨 스코프**를 사용할 수 있다. 블록 레벨 스코프는 코드 블록({ ... }) 내에서 유효한 스코프이다.

```javascript
var x = 0;
{
  var x = 1;
  console.log(x); // 1
}
console.log(x); // 1

let y = 0;
{
  let y = 1;
  console.log(y); // 1
}
console.log(y); // 0
```

## ✔️ 전역 스코프 (Global Scope)

```javascript
var global = "global";

function foo() {
  var local = "local";
  console.log(global); // global
  console.log(local); // local
}
foo();

console.log(global); // global
console.log(local); // Uncaught ReferenceError: local is not defined
```

전역에 변수를 선언하면 어디서나 참조할 수 있는 전역 스코프를 가지는 전역 변수가 된다. 키워드 없이 선언하면 어느 블록에서 선언하건 전역 스코프를 가지고 var 키워드로 전역에서 선언하면 전역 객체 window의 프로퍼티가 된다.

```javascript
if (true) {
  var x = 5;
}
console.log(x);
```

변수 x는 if 문 블록 내에 선언되었지만 자바스크립트는 함수 레벨 스코프를 따르므로 변수 x는 전역 스코프를 가지는 전역 변수이다.

### 암묵적 전역

```javascript
var x = 10; // 전역 변수

function foo() {
  // 선언하지 않은 식별자
  y = 20;
  console.log(x + y);
}

foo(); // 30
```

위 예제에서 y는 선언하지 않은 식별자이다. 그렇지만 `y = 20`를 실행했을 때 에러가 발생하지 않는다. 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되어 `y = 20`은 `window.y = 20`같이 전역 변수처럼 동작한다. 이를 **암묵적 전역(implicit global)**이라고 한다.

```javascript
// 전역 변수 x는 호이스팅이 발생한다.
console.log(x); // undefined
// 전역 변수가 아니라 단지 전역 프로퍼티인 y는 호이스팅이 발생하지 않는다.
console.log(y); // ReferenceError: y is not defined

var x = 10; // 전역 변수

function foo() {
  // 선언하지 않은 변수
  y = 20;
  console.log(x + y);
}

foo(); // 30
```

하지만 y는 선언 없이 전역 객체의 프로퍼티가 된 것이고 전역 변수가 된 것은 아니기 때문에 변수 호이스팅이 발생하지 않는다.

## ✔️ 함수 레벨 스코프 (Function-level Scope)

```javascript
var a = 10; // 전역변수

(function () {
  var b = 20; // 지역변수
})();

console.log(a); // 10
console.log(b); // "b" is not defined
```

자바스크립트는 함수 레벨 스코프를 사용한다. 함수 내에 선언된 매개변수와 변수는 함수 외부에서 접근할 수 없다. 따라서 변수 b는 지역 변수이다.

```javascript
var x = "global";

function foo() {
  var x = "local";
  console.log(x);
}

foo(); // local
console.log(x); // global
```

전역 변수 x와 지역 변수 x가 중복 선언되었다. 전역 스코프에서는 전역 변수만 참조할 수 있고, 지역 스코프에서는 전역 변수와 지역 변수 모두 참조 가능하다. 이럴 때에는 지역 변수를 우선해 참조한다.

```javascript
var x = "global";

function foo() {
  var x = "local";
  console.log(x); // local

  function bar() {
    // 내부함수
    console.log(x); // local
  }

  bar();
}
foo();
console.log(x); // global
```

내부 함수는 자신을 포함하는 외부 함수에 선언된 변수에도 접근할 수 있다.
위의 코드는 클로저가 포함된 예시이다.

```javascript
var x = 10;

function foo() {
  var x = 100;
  console.log(x); // 100

  function bar() {
    // 내부함수
    x = 1000;
    console.log(x); // 1000
  }

  bar();
}
foo();
console.log(x); // 10
```

변수의 이름이 같은 경우 가장 인접한 지역을 우선해서 참조한다.

## ✔️ 렉시컬 스코프 (Lexical Scope)

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```

위 코드는 어떻게 동작할까. 위 코드의 실행 결과는 함수 bar의 상위 스코프가 무엇인지에 따라 결정된다. 예측되는 두 가지 패턴은 다음과 같다.

1. **동적 스코프**: 함수를 어디서 호출하였는지에 따라 상위 스코프를 결정
   - bar의 상위 스코프 차례대로 foo와 전역
2. **렉시컬 스코프 or 정적 스코프**: 함수를 어디서 선언하였는지에 따라 상위 스코프를 결정
   - bar의 상위 스코프 차례대로 전역과 전역

#### 자바스크립트는 렉시컬 스코프를 따른다.

렉시컬 스코프는 함수를 어디서 호출하는지가 아닌 어디에 선언하였는지에 따라 결정된다. 렉시컬 스코프를 따르므로 함수를 선언한 시점에 상위 스코프가 결정된다. 위 예제에서 함수 bar는 전역에 선언되었으므로 함수 bar의 상위 스코프는 전역 스코프이고, 위 예제는 전역 변수 x의 값 1을 두 번 출력한다.

## ✔️ 최소한의 전역 변수 사용하기

### 전역 변수 객체 만들기

전역 변수 사용을 최소화하는 방법 중 하나는 다음과 같이 전역 변수 객체를 하나 만들어 사용하는 것이다.

```javascript
var MYAPP = {};

MYAPP.student = {
  name: "Lee",
  gender: "male",
};

console.log(MYAPP.student.name);
```

### 즉시 실행 함수를 이용한 전역 변수 사용 억제

전역변수 사용을 억제하기 위해, 즉시 실행 함수(IIFE, Immediately-Invoked Function Expression)를 사용할 수 있다. 이 방법을 사용하면 전역변수를 만들지 않으므로 라이브러리 등에 자주 사용된다. 즉시 실행 함수는 즉시 실행되고 그 후 전역에서 바로 사라진다.

```javascript
(function () {
  var MYAPP = {};

  MYAPP.student = { name: "Lee", gender: "male" };

  console.log(MYAPP.student.name); // Lee
})();

console.log(MYAPP.student.name); // Uncaught ReferenceError: MYAPP is not defined
```

### 🚩 참고

[스코프](https://poiemaweb.com/js-scope)
