# JavaScript의 this

## ✔️ 기본 바인딩 - 전역 객체

JavaScript에서의 `this`는 일반적으로 window를 가리킨다.

크롬 콘솔 창에서 `console.log(this)`를 입력하고 결과를 확인했을 때 window가 출력되는 것을 확인할 수 있다.

<img width="642" alt="스크린샷 2022-03-25 14 23 21" src="https://user-images.githubusercontent.com/43867711/160059755-06c5e28b-bd87-414e-9d7f-294537e1b8cd.png">  
  
<br/>
<br/>

터미널을 열고 node 쉘에서 this를 입력했을 경우에는 Object가 출력된다.

<img width="682" alt="스크린샷 2022-03-25 16 28 08" src="https://user-images.githubusercontent.com/43867711/160074462-0e1fc1b6-dcca-41ba-adc4-422d0a288963.png">

크롬 브라우저에서의 Window와 node 쉘에서의 Object는 **자바스크립트 실행 환경의 전역 객체**이다.

### → 기본적으로 this는 `전역 객체`를 가리킨다는 것을 알 수 있다.

```javascript
var num = 100;
console.log(this.num); // 100

function callName() {
  this.name2 = "태림";
  console.log(this);
}

console.log(this.name1); // undefined
console.log(this.name2); // undefined

this.name1 = "ttaerrim";
console.log(this.name1); // ttaerrim
console.log(this.name2); // undefined

callName(); // Window
console.log(this.name1); // ttaerrim
console.log(this.name2); // 태림
```

상단 코드에서 var 키워드로 정의한 name은 this.name과 같고, this가 window를 가리키므로 window.name와도 같다.  
`var name` = `this.name` = `window.name`

this 객체에 name1 프로퍼티를 등록하는 것은 전역 객체를 정의하는 것과 같다는 의미이다.
`this.name1 = 'ttaerrim'`을 하든 `var name1 = 'ttaerrim'`을 하든 동작은 같다.

## ✔️ 암시적 바인딩

객체의 메서드에서 this를 호출할 경우 this는 객체를 가리킨다.

```javascript
var num = 100;

function foo() {
  console.log(this.num);
}

var obj = {
  num: 40,
  func: function () {
    console.log(this.num);
  },
  foo: foo,
};

obj.func(); // 40
obj.foo(); // 40

foo(); // 100

var callNum = obj.foo;
callNum(); // 100
```

obj 객체에서 호출된 func은 `this.num`으로 `obj.num`인 40을 출력한다.  
obj 객체에서 호출된 foo에서 `this`는 obj 객체를 가리키므로 `this.num`은 `obj.num`인 40을 출력한다.

전역으로 호출된 foo에서 this는 window를 가리키므로 전역 변수 num의 값 100을 출력한다.  
전역 객체로 선언된 callNum은 `window.callNum`과 같고, callNum에서 `this`는 다시 window 객체가 되기 때문에 callNum을 호출했을 때는 `window.num`인 100이 출력된다.

### → this를 호출한 위치에서 this가 암시적으로 바인딩된다.

## ✔️ 명시적 바인딩

함수 객체는 `call`, `apply`, `bind` 메서드를 가지고 있다. 이 메소드들을 사용하면 첫 번째 인자로 넘겨지는 것이 this 객체가 된다. 이 메소드들을 사용해 객체를 `this`로 바인딩 하는 것이 명시적 바인딩이다.

```javascript
function foo() {
  console.log(this);
}

var obj = {
  num: 10,
};

foo.call(obj); // {num: 10}
foo.call("안녕하세요"); // String {'안녕하세요'}
```

### ➕ 하드 바인딩

`bind`의 경우 함수를 정의할 때 바인딩할 수 있다. 이를 **하드 바인딩**이라고 한다.

```javascript
function hello() {
  console.log(this.name);
}

var obj = {
  name: "ttaerrim",
};

setTimeout(hello.bind(obj), 1000);

name = "global context!";
```

```
global context
ttaerrim
```

hello를 호출했을 때는 전역 변수 name의 값인 global context를 출력한다.

## ✔️ `new` 바인딩

new 바인딩은 언뜻 보면 클래스 디자인 패턴의 형태를 띄고 있다.

```javascript
function foo(num) {
  this.num = num;
}

var obj = new foo(10);
console.log(obj.num);
```

new를 사용하면 새로운 객체가 만들어진다. 그리고 그 객체의 this에 obj가 바인딩된다.

## ✔️ 바인딩의 우선순위는?

암시적 바인딩과 명시적 바인딩의 우선순위를 비교해 보자.

```javascript
function hello() {
  console.log(this.name);
}

var obj = {
  name: "ttaerrim",
  hello: hello,
};

obj.hello(); // ttaerrim
obj.hello.call({ name: "taylor" }); // taylor
```

`obj.hello()`는 암시적 바인딩이 적용되어 obj가 바인딩 되고 obj.name을 출력한다.  
`obj.hello.call()`을 통해 명시적으로 `{ name: "taylor" }`을 바인딩하면 obj 객체가 아닌 call을 통해 명시적으로 바인딩한 `{ name: "taylor" }` 객체가 this로 바인딩 된다.

### → `명시적 바인딩 > 암시적 바인딩`

암시적 바인딩과 new 바인딩의 우선순위를 알아보자.

```javascript
function hello(name) {
  this.name = name;
}

var obj1 = {
  hello: hello,
};

obj1.hello("ttaerrim");
console.log(obj1.name); // ttaerrim

var obj2 = new obj1.hello("taylor");
console.log(obj1.name); // ttaerrim
console.log(obj2.name); // taylor
```

`obj1.hello("ttaerrim")`을 실행하면 암시적 바인딩 규칙에 따라 hello의 this는 obj1로 바인딩된다. 따라서 `obj1.name`에 ttaerrim이 할당된다.  
반면, 객체의 메소드를 사용하더라도 new 키워드를 사용한다면 `obj2.name`으로 taylor가 할당된 것을 보아 new 바인딩이 암시적 바인딩보다 우선한다는 것을 알 수 있다.

### → `new 바인딩 > 암시적 바인딩`

명시적 바인딩과 new 바인딩의 우선순위를 알아보자.  
명시적 바인딩 중 bind 함수를 이용한 하드 바인딩과 new 바인딩을 비교해 두 우선순위를 알아보도록 할 것이다.

```javascript
function hello(name) {
  this.name = name;
}

var obj1 = {};
var helloFunc = hello.bind(obj1);
helloFunc("ttaerrim");
console.log(obj1.name); // ttaerrim

var obj2 = new helloFunc("taylor");
console.log(obj1.name); // ttaerrim
console.log(obj2.name); // taylor
```

`hello.bind(obj1)`를 통해 hello 함수의 this가 obj1을 가리키도록 하드 바인딩했다. 그 결과로 `helloFunc`를 만들고, `helloFunc("ttaerrim")`을 실행시키면 `obj1.name`이 "ttaerrim"으로 할당된다.

obj1로 하드 바인딩된 helloFunc 함수를 new 키워드를 사용해 호출하면 새로운 객체를 반환하는데, 새로운 객체는 obj2에 바인딩된다. 그 결과로 `obj2.name`에는 "taylor"가 할당된다.

즉, new 바인딩이 명시적 바인딩보다 우선순위가 높다는 것을 알 수 있다.

### → `new 바인딩 > 명시적 바인딩`

### ∴ `new 바인딩 > 명시적 바인딩 > 암시적 바인딩 > 기본 바인딩`

## ✔️ 화살표 함수

화살표 함수는 기존 컨텍스트 바인딩 규칙을 따르지 않는다. 기존 컨텍스트는 실행 시점에 바인딩 규칙이 적용되는 **동적 바인딩**인데 반해, 화살표 함수는 실행하지 않고도 바인딩 규칙을 알 수 있는 이미 정해진 **정적 바인딩**이다.

#### 화살표 함수에서 this가 가리키는 곳은 선언된 시점의 상위 스코프이다.

```javascript
var num = 10;

var obj = {
  num: 2,
  func: () => console.log(this.num),
};

obj.func(); // 10
```

`obj.func`는 원래대로라면 암시적 바인딩이 이루어져야 하지만 obj 객체의 func을 호출한 곳은 전역 스코프이므로 `obj.num`인 2가 아닌 `window.num`인 10이 `this.num`이 된다.

```javascript
var num = 10;

function testArrow() {
  console.log(this);
  return () => console.log(this.num);
}

function testFunc() {
  console.log(this);
  return function () {
    console.log(this.num);
  };
}

var test1 = testArrow(); // Window
test1(); // 10

var context = { num: 999 };
var test2 = testArrow.call(context); // {num: 999}
test2(); // 999

var test3 = testFunc(); // Window
test3(); // 10

var test4 = testFunc.call(context); // {num: 999}
test4(); // 10
```

화살표 함수를 사용한 test2에서 바인딩된 this는 {num: 999} 객체이고, this.num이 이 객체의 num인 999를 가리킨다.  
반면, 일반 함수를 사용한 test4에서 바인딩 된 this는 {num: 999} 객체이지만, this.num은 전역 변수인 10을 가리킨다.

### 🚩 참고

[자바스크립트의 this는 무엇인가?](https://www.zerocho.com/category/JavaScript/post/5b0645cc7e3e36001bf676eb)  
[자바스크립트 this의 4가지 동작 방식](https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-this%EC%9D%98-4%EA%B0%80%EC%A7%80-%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D)  
[자바스크립트 this 바인딩 우선순위](https://jeonghwan-kim.github.io/2017/10/22/js-context-binding.html)  
[화살표 함수와 this 바인딩](https://velog.io/@padoling/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98%EC%99%80-this-%EB%B0%94%EC%9D%B8%EB%94%A9)
