# 모노레포 툴링과 패키지 매니저

## lerna

- 2022년 5월 Nx의 개발사인 [Nrwl](https://nrwl.io/)이 [프로젝트 관리 권한을 인수](https://github.com/lerna/lerna/issues/3121)

## nx

- 구글 개발자들이 만든 오픈소스 프로젝트
- Nx는 모노레포 구성을 위한 다양한 개발 도구를 제공
  - Angular, React와 같은 프런트엔드 프레임워크 기반의 개발 환경 구성
  - Express, [Nest.js](https://nestjs.com/)와 같은 백엔드 기술 기반의 개발까지 폭넓게 지원
  - workspace 생성 시 Cypress, Jest 등을 기반으로 한 테스트 환경까지 설정 가능해 초기 모노레포 개발 환경 구축 비용 줄여 줌

## turborepo

- Vercel에서 개발 및 운영하고 있는 JavaScript/TypeScript를 위한 모노레포 빌드 시스템
- 특징

  - 증분 빌드(incremental build)
  - 원격 캐싱
  - 병렬 처리 기법을 통해
    → 빌드 성능을 끌어올리고
  - `Pipeline`의 쉬운 설정
  - profiling, trace 등 다양한 시각화 기능을 제공해 관리 편의성을 높인 것

- 기존의 모노레포 프로젝트에 점진적으로 Turborepo를 적용할 수 있으며, 쉽게 Turborepo로 구성된 새 모노레포 프로젝트를 생성할 수 있도록 스캐폴딩 기능을 제공
- 패키지 매니저로 `yarn`, `npm`, `pnpm`와 함께 잘 동작하므로 프로젝트 상황에 맞게 선택해 사용할 수 있음
- 참고
  [Turborepo로 모노레포 개발 경험 향상하기](https://engineering.linecorp.com/ko/blog/monorepo-with-turborepo)

## 결론

![Untitled](https://d2.naver.com/content/images/2022/04/0a710dbd-7f96-14ab-817f-aa65aea54d30.png)

표에서 보이는 것처럼 Yarn은 다른 모노레포 도구에 비해 지원하는 것들이 많지는 않지만, 모노레포 사용의 목적이 **단순히 공통 요소를 공유**하는 것이라면 Yarn으로 workspace을 구성해서 개발을 진행하는 것을 추천한다.

하지만 단순한 코드의 공유 외에 **패키지 간 의존성 관리 및 테스트, 배포** 등의 작업에 대한 더 나은 무언가를 필요로 한다면 Lerna를 함께 사용해보는 것도 좋은 선택지가 될 것이다.

그리고 프로젝트가 성장하면서 개발, 관리에 유용한 더 많은 기능이 필요하다면 Nx와 Turborepo를 고려해보면 좋을 것 같다. Nx와 Turborepo는 모두 **캐싱** 기능을 지원해서 빌드 측면에서 우수한 속도를 자랑하고, **의존성 그래프 시각화** 기능을 통해 프로젝트의 구성 요소들이 서로 어떤 의존관계를 갖고있는 지 한눈에 파악이 가능하도록 해준다.

둘 중 좀더 가벼운 시작을 원한다면, **Zero Config**를 지향해 초기 설정이 상대적으로 쉬운 Turborepo를 시도하는 것도 좋을 것이다. 상대적으로 출시된 지 오래되어 관련 레퍼런스, 지원하는 IDE 플러그인 등의 **풍부한 생태계**가 형성되어 있는 Nx 또한 좋은 선택지가 될 수 있다.

위의 네 가지 도구 모두 상황에 따라 좋은 선택지가 될 수 있으니, 현재 프로젝트 상황에 맞는 좋은 도구를 선택하여 즐겁게 개발하길 바란다.

- 참고
  [모던 프론트엔드 프로젝트 구성 기법 - 모노레포 도구 편](https://d2.naver.com/helloworld/7553804)

# package manager

## npm

- npm workspace 기능 지원

## yarn

- yarn v1 / yarn v2 (yarn berry)
- 현재 사용 중인 패키지 매니저
- yarn workspace 기능 지원
- reference
  [](https://blog.mathpresso.com/팀워크-향상을-위한-모노레포-monorepo-시스템-구축-3ae1b0112f1b)

## pnpm

- pnpm workspace
- turborepo와의 궁합이 좋다
- reference
  [Yarn 대신 pnpm으로 넘어간 3가지 이유](https://engineering.ab180.co/stories/yarn-to-pnpm)
