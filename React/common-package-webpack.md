## Webpack 모듈에서 Cannot read properties of null (reading 'useState') 오류 조치

### 1. `webpack.config.js` externals 설정하기

```jsx
module.exports = {
  //...
  externals: {
    react: "react",
    "react-dom": "react-dom",
  },
};
```

### 2. Peer Dependencies 설정하기

```json
{
  "peerDependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "dependencies": {
    // dependencies에는 없어야 함
  }
}
```

공통 모듈 패키지를 사용하는 곳에서 `yarn why react` 명령어 입력했을 때 다음과 같은 결과 나오면 됨

```bash
// ⭕️
info Has been hoisted to "react"
info This module exists because it's specified in "dependencies".

// ❌
info Has been hoisted to "react"
info Reasons this module exists
    - Specified in "dependencies"
    - Hoisted from "@redbrick#package#react"
```
