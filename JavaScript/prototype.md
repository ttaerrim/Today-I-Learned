# 프로토타입

자바스크립트는 클래스라는 개념이 없음. 대신 기존의 객체를 복사하여 새로운 객체를 생성하는 프로토타입 기반 언어이다. 이 프로토타입에 대해 알아보자.

## ✔️ 함수 객체 내부 구조

함수를 만들면 함수 객체가 생기고, 함수의 프로토타입 객체도 생긴다. 함수의 프로토타입 속성이 프로토타입 객체를 참조한다. 프로토타입 객체의 constructor 속성은 함수 객체를 참조한다.

![](https://user-images.githubusercontent.com/43867711/160235440-0067659e-c84f-49ab-a8e2-83f493603d41.png)

프로토타입 객체는 `new Person()` 형식으로 만들어진 모든 객체의 원형이 되는 객체이다.

![image](https://user-images.githubusercontent.com/43867711/160235463-62eede66-62b0-4f97-9e2b-afc6bd3a3732.png)

```jsx
function Person() {}

var joon = new Person();
var jisoo = new Person();
```

객체 안에는 proto라는 속성이 있는데 프로토타입 객체를 숨은 참조로 참조하는 역할을 한다.

## ✔️ 프로토타입 객체

모든 객체는 프로토타입에 접근할 수 있다. 그리고 프로토타입 객체에 동적으로 속성을 추가할 수 있다.

```jsx
function Person() {}

var ttaerrim = new Person();
var taylor = new Person();

Person.prototype.getType = function () {
  return "인간";
};

console.log(ttaerrim.getType()); // 인간
console.log(taylor.getType()); // 인간
```

동적으로 getType을 추가했다. 멤버 추가하기 전에 선언한 객체에서도 사용할 수 있다.

프로토타입 객체에 멤버를 추가, 수정, 삭제할 때는 `Person.prototype`와 같이 `함수명.prototype`으로 접근해야 한다.

프로토타입 객체에 추가한 멤버를 읽을 때는 `Person.prototype.getType` 이나 `ttaerrim.getType`와 같이 `함수명.prototype`이나 `객체명.getType`으로 접근해야 한다.

```jsx
ttaerrim.getType = function () {
  return "사람";
};

console.log(ttaerrim.getType()); // 사람
console.log(taylor.getType()); // 인간

taylor.age = 25;

console.log(ttaerrim.age); // undefined
console.log(taylor.age); // 25
```

ttaerrim.getType을 사람으로 수정했다. ttaerrim과 taylor의 getType()을 호출해 출력해 보면 수정한 ttaerrim은 사람, taylor는 인간이 출력된다.

생성된 객체의 멤버를 수정하면 프로토타입 객체가 수정되는 게 아니라 객체에 멤버가 추가된다. ttaerrim 객체의 getType() 메소드는 프로토타입 객체의 멤버가 아니라 ttaerrim 객체의 멤버인 것이다.

만약 프로토타입 객체의 멤버를 수정하고 싶다면 함수명.prototype에 접근해서 다음과 같이 수정한다.

```jsx
Person.prototype.getType = function () {
  return "사람";
};

console.log(taylor.getType()); // 사람
```

프로토타입 객체는 새로운 객체가 생성되기 위한 원형이 되는 객체이다. 같은 원형으로 생성된 객체가 공통으로 참조하는 공간이다. 프로토타입 객체의 멤버를 읽는 경우에는 객체 또는 함수의 prototype 속성을 통해 접근할 수 있다. 하지만 추가, 수정, 삭제는 함수의 prototype 속성을 통해 접근해야 한다.

## ✔️ 프로토타입이란?

`var ttaerrim = new Person()`과 같이 객체를 만들 때는 프로토타입 객체를 이용해 객체가 만들어진다. 이렇게 만들어진 `ttaerrim` 객체 안에는 `__proto__` 속성이 있는데, 이 proto 속성이 프로토타입 객체를 숨은 링크로 참조한다.

이 숨겨진 링크로 프로토타입 객체 멤버에 접근할 수 있고, 이 링크를 사용해 코드를 재사용하고 객체 지향적인 프로그래밍을 할 수 있다.

## ✔️ 코드의 재사용 - 프로토타입 상속

수많은 객체가 공통으로 사용하는 속성과 메소드가 있을 때 프로토타입을 이용해 효율적으로 사용할 수 있다. 어떤 객체에 프로토타입을 지정하고, 프로토타입의 속성을 해당 객체에서 재사용할 수 있다.

부모에 해당하는 함수를 이용해 객체를 생성할 수 있다. 자식 함수의 프로토타입 속성을 부모 함수를 이용해 생성한 객체로 참조한다.

```jsx
function Person(name) {
  this.name = name || "태림";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {}
Korean.prototype = new Person();

var korean1 = new Korean();
console.log(korean1.getName());

var korean2 = new Korean("민지");
console.log(korean2.getName());
```

`Korean.prototype = new Person()`에서 Korean 함수의 prototype 속성을 부모 함수인 Person으로 생성된 객체로 변경했다. 따라서 Korean 함수와 new 연산자로 생성한 korean1 객체의 **proto** 속성은 Person 객체를 참조한다. korean1 객체에 name과 getName() 속성은 없지만 부모 프로토타입 객체에 name과 getName() 속성이 있으므로 korean1 객체에서 사용할 수 있다.

하지만 부모 객체와 부모 객체의 프로토타입 속성을 모두 상속받아 사용하므로 객체 자신의 속성은 특정 인스턴스에 한정되어 재사용할 수 없어지므로 필요가 없어진다. 또한 korean2 객체처럼 인자를 넘겨도 부모 객체를 생성할 때 이를 사용하지 못한다.

이 방법은 `apply` 함수로 해결할 수 있다.

```jsx
function Person(name) {
  this.name = name || "태림";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  Person.apply(this, arguments);
}

var korean1 = new Korean("민지");
console.log(korean1.name); // 민지
console.log(korean1.getName()); // error
```

Korean 함수 내부에서 부모 함수인 Person 영역의 this를 Korean 함수 내의 this로 바인딩한다. 이렇게 하면 부모의 속성을 자식 함수 내에 모두 복사할 수 있다. 그렇지만 이렇게 생성자를 빌려 쓰는 방법은 부모 객체의 this로 된 멤버들만 물려받을 수 있다. 그래서 부모 객체의 프로토타입 객체 멤버들은 상속받을 수 없다. `korean1.name`은 민지를 출력하지만 `korean1.getName()`을 호출했을 땐 에러를 출력하는 것을 확인할 수 있다.

그렇다면 프로토타입도 지정해 주어 이 문제를 해결해 보자.

```jsx
function Person(name) {
  this.name = name || "태림";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  Person.apply(this, arguments);
}

Korean.prototype = new Person();

var korean1 = new Korean("민지");
console.log(korean1.name); // 민지
console.log(korean1.getName()); // 민지
```

`apply` 함수를 사용해 부모 함수의 this를 자식 함수의 this로 바인딩한다.그리고 `Korean.prototype = new Person()`로 자식 함수의 프로토타입 속성을 부모 함수를 사용해 지정해 준다. 부모 객체 속성을 참조하는 것이 아니라 복사본을 만드는 것이다. 이런 방식을 사용하면 this로 된 멤버도 상속받을 수 있고 프로토타입의 멤버도 사용할 수 있다.

그렇지만 이 방법 또한 부모 생성자를 두 번 호출하기 때문에 부모 함수를 이용해 `new Person()`으로 생성한 객체에도 name 멤버가 할당되어 있다는 문제가 있다.

```jsx
function Person(name) {
  this.name = name || "태림";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  this.name = name;
}

Korean.prototype = Person.prototype;

var korean1 = new Korean("민지");
console.log(korean1.name); // 민지
console.log(korean1.getName()); // 민지
```

그렇다면 Korean.prototype을 부모 함수의 프로토타입 속성이 참조하는 객체로 설정해 보자. 자식 함수로 생성된 객체는 부모 함수로 생성된 객체를 거치지 않고도 부모 함수의 프로토타입 객체를 부모로 지정해 객체를 생성한다. 부모 함수의 내용을 상속받지 못하므로 상속받으려는 부분을 자식 함수의 내부에 직접 작성해 주어야 원하는 결과를 얻을 수 있다.

`Object.create` 함수를 사용해 객체의 프로토타입을 지정해 보자. 이 방법은 객체를 생성함과 동시에 프로토타입 객체를 지정한다. 첫 번째 매개변수로 부모 객체로 사용할 객체를 넘겨준다. 두 번째 매개변수는 옵션으로, 반환되는 자식 객체의 속성에 추가되는 부분이다.

```jsx
const preson = {
  introduce: function () {
    return `안녕하세요 ${this.name}입니다`;
  },
};

const ttaerrim = Object.create(preson);
ttaerrim.name = "이태림";

const taylor = Object.create(preson);
taylor.name = "테일러";

console.log(ttaerrim.introduce()); // 안녕하세요 이태림입니다
console.log(taylor.introduce()); // 안녕하세요 테일러입니다

ttaerrim.introduce === taylor.introduce; // true
```

부모 객체에 해당하는 person을 객체 리터럴 방식으로 생성한다. 그리고 `Object.create(preson)`로 person을 넘겨받아 객체를 생성한다. 한 줄로 객체를 생성함과 동시에 부모 객체의 속성도 물려받았다.

이런 식으로 프로토타입을 이용해 다른 객체의 기능을 가져와 사용하는 것을 **프로토타입 상속**이라고 한다. 위와 같은 경우는 `personPrototype은 ttaerrim의 프로토타입이다`, `ttaerrim 객체는 personPrototype을 상속받았다`라고 표현한다.

프로토타입 상속은 다른 언어와 차별화되는 자바스크립트의 특징적인 기능이다.

## ✔️ 프로토타입 체인

프로토타입 상속을 받은 객체가 실제로 어떻게 생겼는지 확인해 보자.
앞서, `Object.setPrototypeOf` 함수를 사용하면 이미 생성된 객체의 프로토타입을 변경할 수 있다. (하지만 작업 속도가 느리므로 사용을 지양함)

```jsx
const parent = {
  a: 1,
};

const child = {
  b: 2,
};

Object.setPrototypeOf(child, parent);
console.log(child); // {b: 2}
console.log(child.a); // 1
```

`child` 객체를 출력해 보면 a 속성이 없지만 `child.a`를 출력해 보면 1이라는 결과가 나오는 것을 확인할 수 있다.

이런 결과가 나오는 이유는 JavaScript 엔진이 `child` 객체의 속성만 확인하는 것이 아니라 **프로토타입 객체의 속성까지 확인**하기 때문이다.

프로토타입 객체는 또 부모 객체를 가질 수 있다. 프로토타입 객체의 프로토타입 객체가 있을 수 있다는 뜻이다. 이런 식으로 이어져 있는 프로토타입의 연쇄를 **프로토타입 체인**이라고 한다.

만약 프로토타입 객체에도 속성이 없다면 프로토타입의 프로토타입까지 확인을 거치고 거쳐서 남아 있는 프로토타입이 없을 때까지 확인한다. 모두 확인했을 때에도 속성 값이 없다면 그제서야 `undefined`를 반환한다. 즉, JavaScript 엔진은 **속성 접근자를 통해 어떤 객체의 속성을 확인할 때 프로토타입 체인을 전부 확인**한다.

```jsx
const obj1 = {
  a: 1,
};

const obj2 = {
  b: 2,
};

const obj3 = {
  c: 3,
};

// `obj3 -> obj2 -> obj1` 과 같이 상속
Object.setPrototypeOf(obj2, obj1);
Object.setPrototypeOf(obj3, obj2);

console.log(obj3.a); // `obj3`의 프로토타입의 프로토타입에 존재하는 속성 `a`의 값을 출력
console.log(obj3.b); // `obj3`의 프로토타입에 존재하는 속성 `b`의 값을 출력
console.log(obj3.c); // `obj3`에 존재하는 속성 `c`의 값을 출력
```

프로토타입 체인은 눈에 명확하게 보이지는 않지만 객체의 속성에 접근할 때마다 탐색된다. 따라서 프로토타입 체인의 깊이가 너무 깊으면 읽기 속도에 영향을 미칠 수 있다.

```jsx
obj1.isPrototypeOf(obj3); // true
obj2.isPrototypeOf(obj3); // true
```

어떤 객체가 다른 객체의 프로토타입에 존재하는지 확인하기 위해 `isPrototypeOf` 메소드를 사용하여 확인할 수 있다.

### ❔ 프로토타입 체인의 끝

프로토타입의 끝은 대체 어디일까? JavaScript에서 객체의 프로토타입으로 객체 또는 null 이외의 값은 지정할 수 없다. 지정하려고 하면 에러가 나거나, 무시된다.

`Object.getPrototypeOf`를 사용해 프로토타입을 확인할 수 있는데, 이를 사용하면 null이 나온다.

```jsx
Object.getPrototypeOf(Object.prototype); // null
```

프로토타입 체인을 따라가다 보면 결국에는 null에 도달한다는 결론을 얻을 수 있다. 이때 프로토타입 체인을 확인하는 과정이 끝난다.

### 🚩 참고

[JavaScript: 프로토타입(prototype) 이해](https://www.nextree.co.kr/p7323/)  
[Hello World JavaScript - 프로토타입](https://helloworldjavascript.net/pages/180-object.html#%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85-prototype)
