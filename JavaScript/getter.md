# getter

`get` 문법은 객체의 속성에 접근할 때 호출할 함수를 바인딩해 준다.  
이는 접근자 프로퍼티(accessor property)로서 값을 **획득(get)**하고 설정(set)하는 역할을 담당한다. getter니까 값을 획득하게 해 주는 역할을 한다.  
객체 리터럴 안에서 getter와 setter 메소드는 `get`과 `set`으로 작성하여 사용하면 된다.

```javascript
const obj = {
    log: ['a', 'b', 'c'],
    get latest() {
        if (this.log.length === 0) {
            return undefined;
        }
        return this.log[this.log.length - 1];
    }
}

console.log(obj.latest) // "c"
```

```javascript
get isEditMode() {
    return !!this.applianceId;
}

console.log(this.isEditMode)
```