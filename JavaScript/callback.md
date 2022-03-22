# callback

## ✔️ 콜백 함수

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수(callback function)라고 한다.
매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수는 고차 함수(Higher-Order Function, HOF)라고 한다.

특정 작업을 n 번 반복시키는 함수가 있다고 하자.

```javascript
function repeat(n) {
  for (let i = 0; i < n; i++) {
    console.log(i);
  }
}

repeat(5); // 0 1 2 3 4
```

`console.log(i)` 대신 다른 작업을 n번 반복시키고 싶을 수도 있을 것이다. 그럴 때 함수로 n과 같이 콜백 함수를 넘겨 준다면 우리가 원하는 대로 특정 작업을 반복시킬 수 있을 것이다.

```javascript
function repeat(n, callback) {
  for (let i = 0; i < n; i++) {
    callback(i);
  }
}

const logAll = (i) => {
  console.log(i);
};

repeat(5, logAll); // 0 1 2 3 4

const logOdds = (i) => {
  if (i % 2) console.log(i);
};

repeat(5, logOdds); // 1 3
```

## ✔️ callback hell

비동기 처리를 위해 콜백 함수를 연속해서 사용할 때 중첩되면서 생기는 문제를 보통 콜백 지옥(callback hell)이라고 부른다.

```javascript
$.get("https://api.test.com/proudcts", function (response) {
  var firstProductId = response.products[0].id;
  $.get(
    "https://api.test.com/proudct/comments?id=" + firstProductId,
    function (response) {
      var firstCommentId = response.comments[0].id;
      $.get(
        "https://api.test.com/proudct/comment/likes?id=" + firstCommentId,
        function (response) {
          var likes = response.likes;
          var likesCount = likes.length;
        }
      );
    }
  );
});
```

이렇게 콜백이 중첩되어 있는 코드는 가독성도 좋지 않을뿐더러 유지 보수 하기도 어렵다는 단점이 있다.

### ❕ 콜백 지옥 개선

콜백 지옥은 `Promise 사용`, `Async / Await 사용`, `코딩 패턴 개선`으로 해결할 수 있다.

코딩 패턴으로 개선하는 방법은 다음과 같이 중첩되어 있는 콜백을 따로 분리하여 작성하는 방법이다.

```javascript
function fetchCommentLikes(commentId) {
  $.get(
    "https://api.test.com/proudct/comment/likes?id=" + commentId,
    function (response) {
      var likes = response.likes;
      var likesCount = likes.length;
    }
  );
}

function fetchComments(productId) {
  $.get(
    "https://api.test.com/proudct/comments?id=" + productId,
    function (response) {
      var firstCommentId = response.comments[0].id;

      fetchCommentLikes(firstCommentId);
    }
  );
}

$.get("https://api.test.com/proudcts", function (response) {
  var firstProductId = response.products[0].id;

  fetchComments(firstProductId);
});
```

그러나 이마저도 흐름을 쫓아가며 읽어야 하기 때문에 눈에 썩 잘 들어오지는 않는다.
