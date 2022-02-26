# proxy 설정하기

CORS(Cross-Origin Resource Sharing) 에러는 현재 리소스와 다른 리소스에 접근할 때(현재 도메인에서 다른 도메인에 접근할 때) 주로 발생하는 에러이다.

React에서는 `package.json`에 `proxy`를 설정해 주어 간단하게 해결할 수 있다.

```json
{
    ...
    "proxy": "http://localhost:3000",
    ...
}
```

따위와 같이 `package.json`에 `proxy` 설정을 해 준다.

```javascript
const linkData = axios.get("/homeworks/links");
```

API를 호출할 때는 endpoint만 작성해도 된다.

## Netlify 배포할 때 proxy 설정하기

Netlify로 배포할 때는 한 번 더 설정해 줘야 한다.

```
[[redirects]]
  from = "/proxy/*"
  to = "접근할url/:splat"
  status = 200
  force = true
```

프로젝트의 root 경로에 `netlify.toml` 파일을 작성해 다음 내용을 붙여 넣는다.
to에는 `접근해야 할 url/:spalt`와 같이 작성한다.

```javascript
const PROXY = window.location.hostname === "localhost" ? "" : "/proxy";
const url = "/endpoints";

useEffect(() => {
  (async () => {
    try {
      const { data } = await axios.get(`${PROXY}${url}`);
      setLinkData(data);
    } catch (error) {
      console.log(error);
    }
  })();
}, []);
```

API를 호출하는 파일에는 다음과 같이 작성한다.

그러면 배포된 netlify 주소에 endpoint를 호출하는 게 아니라, 원하는 대로 지정해 준 proxy/endpoint 주소가 호출되는 것을 확인할 수 있다.
