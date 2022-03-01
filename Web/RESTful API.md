# RESTful API

<s>REST, REST 하는데 그러니까 그게 대체 뭘까?  
나도 졸업 작품 발표할 때 질문을 받은 적 있었는데...... 너무 기본이라고 생각했지만 제대로 대답 못 했다. 거두절미하고 알아보자.</s>

## REST

**REST**: **Re**presentational **S**tate **T**ransfer

- REST는 **네트워크 아키텍처 원리**의 모음 중 하나라고 할 수 있다.  
  네트워크 아키텍처 원리? -> 자원을 정의하고 자원에 대한 주소를 지정하는 방법

- REST는 자원을 이름으로 구분하고 자원의 상태를 주고받는다.
- 웹의 기존 기술과 HTTP 프로토콜을 활용하기 때문에 웹의 장점을 활용할 수 있는 아키텍처 스타일

## API

**API**: Application Programming Interface

- 두 응용 프로그램이 통신할 수 있도록 하는 소프트웨어 중개자
- 서로 정보를 교환하게 해 줌  
  ex) 날씨 앱에서 날씨 확인할 때마다 API를 사용

### 🔵 REST API -> REST 기반으로 API를 구현한 것

## REST 장단점

**장점**

- HTTP 프로토콜의 규칙을 그대로 사용하므로 별도로 인프라 구축할 필요 없음
- HTTP 프로토콜 따르는 모든 플랫폼에서 사용 가능
- REST API 메시지가 의도하는 바를 쉽게 파악할 수 있음
- 서버와 클라이언트의 역할을 명확하게 분리

**단점**

- 표준이 없음
- HTTP Method(GET, POST, PUT, DELETE)를 따르므로 사용할 수 있는 메소드가 제한적임
- 구형 브라우저 지원 못 하는 부분 존재

## REST 구성

1. 자원(Resource)
2. 행위(Verb)
3. 표현(Representations)

### ✔️ 자원(Resource)

URI: `/blog/post` 따위 같은 것들

- URI는 정보의 자원을 표현해야 함
- 동사보다 명사 사용하기

### ✔️ 행위(Verb)

행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

❌ DELETE `/blog/delete/1`  
⭕️ DELETE `/blog/1`

delete와 같은 표현은 들어가지 않아도 된다.

## REST 특징

1. `Uniform interface (인터페이스 일관성)`

- Resource의 조작은 지정된 URI로만 할 수 있다.
- 모든 리소스는 HTTP GET과 같은 공통적인 접근 방식으로 액세스할 수 있어야 하고, 일관된 접근 방식을 사용해야 한다.

2. `Client-Server`

- 클라이언트와 서버는 각각에 종속되지 않아야 한다. -> **의존성 줄어듦**
- 클라이언트는 리소스 URI만 알면 된다.

3. `Stateless`

- REST는 HTTP의 규칙을 그대로 반영하므로, 모든 클라이언트와 서버의 상호작용은 `stateless`하다.
- 서버는 클라이언트가 만든 HTTP 리퀘스트를 저장하지 않는다.

  - 세션과 쿠키 신경 쓸 필요 없다.

- 클라이언트가 애플리케이션의 상태를 관리해야 한다.
- 서버의 처리 방식은 일관적이어야 하므로 부담이 줄어든다.

4. `Cacheable (캐시 가능)`

- 부하를 감소시켜 클라이언트의 성능 향상과 서버의 확장성 범위를 넓힌다.
- 적용 가능하다면 리소스에 적용시키고, 적용된 리소스는 캐시 가능하다고 선언해야 한다.
- 서버나 클라이언트 측에서 구현할 수 있다.

5. `Layered System (계층화 시스템)`

- 서버 A에 API 배포하고 서버 B에 데이터 저장하고 서버 C에서 요청 인증하는 계층화된 시스템을 사용할 수 있다.
- 클라이언트는 어느 서버에 연결되어 있는지 모른다.

6. `Code on demand (주문형 코드)` - optional

- 보통 XML이나 JSON 형태로 데이터가 전달되는데, 필요하다면 실행가능한 코드를 리턴할 수도 있다.

## REST 설계 시 주의점

1. 슬래시 구분자는 계층 관계를 나타낼 때 사용

```
http://www.myapi.com/university/major/philosophy
http://www.myapi.com/calculation/add
```

2. URI 마지막 문자에 슬래시 포함하지 않기

```
http://www.myapi.com/university/major/philosophy/ ❌
http://www.myapi.com/university/major/philosophy  ⭕️
```

3. 가독성을 위한다면 언더스코어(\_) 대신 하이픈(-) 사용

```
http://www.myapi.com/university/major/liberal_arts/philosophy/ ❌
http://www.myapi.com/university/major/liberal-arts/philosophy  ⭕️
```

### 참고

[restfulapi](https://restfulapi.net/)  
[URI - 위키피디아](https://ko.wikipedia.org/wiki/%ED%86%B5%ED%95%A9_%EC%9E%90%EC%9B%90_%EC%8B%9D%EB%B3%84%EC%9E%90)  
[What is an API](https://www.mulesoft.com/resources/api/what-is-an-api)  
[REST란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)
