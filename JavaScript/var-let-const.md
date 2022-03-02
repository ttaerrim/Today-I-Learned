## `var`

### 1. 중복 선언 가능

```
var x = 10;
var y = 20;

var x = 100;
var y; // 초기화문 없으면 무시

console.log(x);  // 100
console.log(y);  // 20
```

### 2. 함수 레벨 스코프

함수의 코드 블록을 지역 스코프로 인정하므로 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 전역 변수로 인정됨.

### 3. 변수 호이스팅

var 키워드로 변수를 선언하면 호이스팅에 의해 스코프의 상단에 위치한 것처럼 끌어 올려져서 동작함.

```
console.log(foo); // undefined
foo = 123;
console.log(foo); // 123
var foo;
```

## `let`

`var` 키워드의 단점을 보완하기 위해 ES6에서 도입한 새로운 키워드

### 1. 중복 선언 금지

```
var x = 10;
var x = 100;

console.log(x);  // 100

let y = 20;
let y = 200; // SyntaxError: Identifier 'y' has already been declared
```

### 2. 블록 레벨 스코프

`let` 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 **블록 레벨 스코프**를 따름

```
let a = 1;

{
    let a = 10;
    let b = 20;
}

console.log(a); // 1
console.log(b); // ReferenceError: b is not defined
```

a는 전역 변수, 블록 안에 있는 a는 지역 변수, b는 지역 변수

### 3. 함수 호이스팅

`let` 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작함

```
console.log(foo); // ReferenceError: foo is not defined
let foo = 123;
```

그렇지만 호이스팅이 발생하지 않는 것은 아니다.

```
let foo = 123;

{
    console.log(foo); // ReferenceError: Cannot access 'foo' before initializtion
    let foo = 3;
}
```

호이스팅이 발생하기 때문에 ReferenceError가 발생한다.

자바스크립트는 let, const를 포함한 모든 선언(var, let, const, function, function\*, class)에서 모두 호이스팅이 발생하지만 ES6에서 도입된 let, const, class에서는 호이스팅이 발생하지 않는 것처럼 동작한다.

## `const`

### 1. 선언과 초기화

`const`로 선언된 변수는 선언과 동시에 초기화해야 한다.

```
const foo = 1; (o)
const foo;     (x)
```

`let`과 마찬가지로 블록 레벨 스코프를 가지고, 호이스팅이 발생하지 않는 것처럼 동작한다.

### 2. 재할당 금지

`const`로 선언한 변수는 재할당이 금지된다.

```
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 3. 상수

`const` 키워드와 대문자 이름을 사용해 상수임을 나타낸다.

```
const DISCOUNT_RATE = 0.1;

let price = 10000;
let discountPrice = price - (price * DISCOUNT_RATE)

console.log(discountPrice)
```

### 4. 객체

`const` 키워드로 선언한 객체의 경우 값을 변경할 수 있다.

```
const myself = {
    name: 'ttaerrim'
}

myself.name = 'leetaerim';
console.log(myself) // {name: 'leetaerim'}
```

## `var` vs `let` vs `const`

변수는 기본적으로 `const`를 사용하고 재할당이 필요한 경우에 한해 `let`을 사용하는 것이 좋다. 기본적으로는 `const` 키워드로 변수를 선언하는 것이 안전하다.

- ES6에서는 `var` 키워드를 사용하지 않는다.
- 재할당이 필요한 경우에 한해 `let` 키워드를 사용하고 이때 변수의 스코프는 최대한 좁게 만든다.
- 재할당이 필요 없는 값에는 `const` 키워드를 사용한다.

우선 `const`를 사용하고 재할당이 발생하는 경우라면 그때 `let` 키워드로 수정하는 것을 추천한다.
