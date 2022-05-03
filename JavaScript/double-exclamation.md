## 느낌표 두 개 연산자 (Double Exclamation Marks)

주로 Boolean으로 형 변환을 하고자 할 때 사용한다. 0을 false, 1을 true로 만들 때 사용할 수 있다.
undefined, null, "", 0일 경우 false를 반환, 그 외에는 true를 반환한다.

```javascript
const a = undefined
const b = ""
const c = 0
const d = true
const e = 1
const f = null

console.log(!!a, !!b, !!c, !!d, !!e, !!f)
// false false false true true false
```