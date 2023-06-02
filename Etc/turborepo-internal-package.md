# Turborepo Internal Package

- https://turbo.build/repo/docs/handbook/sharing-code/internal-packages

```
├── apps
│   └── web
└── packages
    └── utils
```

packages/utils/package.json

```
{
  "name": "math-helpers",
  "main": "src/index.ts",
  "types": "src/index.ts",
  "dependencies": {
    "typescript": "latest"
  }
}
```

main과 types를 `src/index.ts`로 지정해 주기

apps/web 내에서 packages/utils를 import하여 사용하기

```
../../packages/utils/src/index.ts
Module parse failed: Unexpected token (1:21)
You may need an appropriate loader to handle this file type,
currently no loaders are configured to process this file.
See https://webpack.js.org/concepts#loaders

> export const add = (a: number, b: number) => {
|   return a + b;
| };
```

TS를 번들링 거치지 않고 사용할 수 없기 때문에 해당 파일을 사용하기 위한 적절한 로더가 필요하다는 에러 메시지 출력됨

`Next.js`를 사용하는 경우 `next.config.js`에서 다음과 같이 해결

```
/** @type {import('next').NextConfig} */
const nextConfig = {
  transpilePackages: ['math-helpers'],
};

module.exports = nextConfig;
```

`Vite`를 사용하는 경우 모듈 자동으로 트랜스파일링 해 줌

`CRA`와 같이 단순 리액트 앱 사용하는 경우 리서치 중

**solutions**

1. webpack을 사용하여 특정 패키지를 transpile 또는 bundle 할 수 있습니다. 이 작업을 수행하려면, **`webpack.config.js`** 파일에 **`module`** 항목에 있는 **`rules`** 설정을 추가하거나 변경해야 합니다

   ```bash
   module.exports = {
     module: {
       rules: [
         {
           test: /\.js$/,
           include: /node_modules\/my-module/,
           use: {
             loader: 'babel-loader',
             options: {
               presets: ['@babel/preset-env']
             }
           }
         }
       ]
     }
   }
   ```

   ```
   apps
   └─ web
      ├─ .yarn
      └─ node_modules
         └─ .cache
           ├─ babel-loader
           └─ default-development

   ```

   node_modules에 패키지가 있는 구조가 아님…..?

2. CRA → Vite migration
   - 이제 더 이상 신규 프로젝트를 CRA로 생성하는 것을 추천하지 않는 추세임. ([참고](https://junghan92.medium.com/%EB%B2%88%EC%97%AD-create-react-app-%EA%B6%8C%EC%9E%A5%EC%9D%84-vite%EB%A1%9C-%EB%8C%80%EC%B2%B4-pr-%EB%8C%80%ED%95%9C-dan-abramov%EC%9D%98-%EB%8B%B5%EB%B3%80-3050b5678ac8))
   - 리액트 프레임워크 사용(Next.js)으로 넘어가기 전에 중간 단계 겪는 것(Vite 사용하기?)도 나쁘지 않을지도?
     - but why not webpack? → vite가 빠르다는 이유를 댈 수 있겠음

### 참고

[Internal Packages – Turborepo](https://turbo.build/repo/docs/handbook/sharing-code/internal-packages)
