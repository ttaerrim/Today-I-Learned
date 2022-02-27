### setInterval

```
const intervalID = setInterval(myCallback, 2000);
const myCallback = () => {
    console.log('2초 지났어요!');
}
```

```
const intervalID = setInterval(()=>{
    console.log('2초 지났어요!');
}), 2000);
```

`setInterval` 메소드는 인자로 함수와 시간(ms)을 받는다. 시간이 지날 때마다 첫 번째 인자로 받은 함수가 실행된다.

```
const myCallback = (value1, value2) =>{
    console.log(value1)
    console.log(value2)
}

const intervalID = setInterval(myCallback, 2000, 'a', 'b');
```

첫 번째 인자의 함수에 전달하고 싶은 인자가 있다면 시간 뒤에 작성해 주면 된다.

### clearInterval

`clearInterval` 메소드는 `setInterval`로 반복 실행했던 함수를 취소해준다. 취소하고 싶은 동작을 인자로 전달하면 된다.

```
const intervalID = setInterval(()=>{
    console.log('2초 지났어요!');
}), 2000);

clearInterval(intervalID)
```
