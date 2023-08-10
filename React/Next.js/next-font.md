# next.js google font

```bash
npm i @next/font

# or

yarn add @next/font
```

@next/font와 next의 버전을 일치시켜 사용해야 함

폰트 이름이 `Zilla Slab` 이라면 띄어쓰기를 언더바(\_)로 바꿔서 사용

```typescript
import { Zilla_Slab } from "@next/font/google";

const zilla = Zilla_Slab({
  weight: ["300"],
  style: ["italic"],
  display: "swap",
});
```

## 새로고침 시 폰트 적용되지 않는 이슈

```typescript
const zilla = Zilla_Slab({
  weight: ["300"],
  style: ["italic"],
  display: "swap",
});
```

`display: "swap"` 옵션을 추가하면 해결됨
