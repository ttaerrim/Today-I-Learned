# react-three

## useProgress

`THREE.DefaultLoadingManager` 의 진행 상태를 래핑하는 hook

https://github.com/pmndrs/drei/blob/master/src/core/useProgress.tsx

- errors: string[]
  - 에러 발생했을 때 array에 값 담김
- active: boolean
  - start 될 때 true, load 됐을 때 false
- progress: number
  - item의 로드 진행률
- item: string
  - 방금 로드된 item의 URL
- loaded: number
  - 지금까지 로드된 item의 개수
- total: number
  - 로드하려는 item의 총 개수

```javascript
function Loader() {
  const { active, progress, errors, item, loaded, total } = useProgress();
  return <Html center>{progress} % loaded</Html>;
}

return (
  <Suspense fallback={<Loader />}>
    <AsyncModels />
  </Suspense>
);
```

## useState, useEffect 대신 useProgress로 모델의 로딩 상태 표시하기

### Before

```javascript
import { useEffect, useState } from "react";
import { Html, useProgress } from "@react-three/drei";
import { Canvas, useThree } from "@react-three/fiber";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

function Model() {
  const [gltf, setGltf] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const { scene } = useThree();

  const modelUrl = "./example.glb";

  useEffect(() => {
    setIsLoading(true);
    const loader = new GLTFLoader();
    loader.load(modelUrl, (loadedGltf) => {
      const model = loadedGltf.scene;
      model.position.set(0, 0, 0);
      setGltf(loadedGltf);
      setIsLoading(false);
    });
  }, []);

  useEffect(() => {
    if (gltf) {
      const model = gltf.scene;
      scene.add(model);

      return () => {
        scene.remove(model);
      };
    }
  }, [gltf, scene]);

  return (
    isLoading && (
      <Html center zIndexRange={[100, 100]}>
        <p>loading...</p>
      </Html>
    )
  );
}
export default function ReactCanvas() {
  return (
    <Canvas>
      <Model />
    </Canvas>
  );
}
```

### After

```javascript
import { useEffect, useState } from "react";
import { Html, useProgress } from "@react-three/drei";
import { Canvas, useThree } from "@react-three/fiber";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

function Model() {
  const [gltf, setGltf] = useState(null);
  const { progress } = useProgress();
  const { scene } = useThree();

  const modelUrl = "./example.glb";

  useEffect(() => {
    const loader = new GLTFLoader();
    loader.load(modelUrl, (loadedGltf) => {
      const model = loadedGltf.scene;
      model.position.set(0, 0, 0);
      setGltf(loadedGltf);
    });
  }, []);

  useEffect(() => {
    if (gltf) {
      const model = gltf.scene;
      scene.add(model);

      return () => {
        scene.remove(model);
      };
    }
  }, [gltf, scene]);

  return (
    progress !== 100 && (
      <Html center zIndexRange={[100, 100]}>
        <p>{progress}</p>
      </Html>
    )
  );
}
export default function ReactCanvas() {
  return (
    <Canvas>
      <Model />
    </Canvas>
  );
}
```
