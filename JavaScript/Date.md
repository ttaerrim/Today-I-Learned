# Date()

`1995-12-17T03:24:00`와 같은 형태는 ISO 형식.  
`new Date('1995-12-17T03:24:00')`로 날짜 객체를 얻을 수 있음.

### toLocaleString

```
const event = new Date('1995-12-17T03:24:00');

// British English uses day-month-year order and 24-hour time without AM/PM
console.log(event.toLocaleString('en-GB', { timeZone: 'UTC' }));
// expected output: 17/12/1995, 03:24:00

// Korean uses year-month-day order and 12-hour time with AM/PM
console.log(event.toLocaleString('ko-KR', { timeZone: 'UTC' }));
// expected output: 1995. 12. 17. 오전 3:24:00
```

ISO 형식의 Date 객체를 `toLocaleString` 메소드를 사용하여 원하는 포맷대로 변환할 수 있음.
