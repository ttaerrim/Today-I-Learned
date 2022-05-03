# URLSearchParams

## ✔️ URLSearchParams란?
URL의 파라미터 값을 확인하고 싶을 때 `URLSearchParams`를 사용할 수 있다.

```javascript
const params = new URLSearchParams('?a=aaa&b=bbb');

console.log(params.get('a')) // aaa
console.log(params.get('b')) // bbb

params.append('c', 'ccc')
console.log(params.get('c')) // ccc

params.delete('c')
console.log(params.get('c')) // null

console.log(params.has('a')) // true

console.log(params.toString()) // ?a=aaa&b=bbb
```