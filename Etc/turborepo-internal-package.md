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
