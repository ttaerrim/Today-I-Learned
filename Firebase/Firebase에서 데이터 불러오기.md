# Firebase에서 데이터 불러오기

### 초기 설정

```
// firebase_config.js

import { initializeApp } from 'firebase/app';
import { getDatabase } from "firebase/database";

// TODO: Replace with your app's Firebase project configuration
const firebaseConfig = {
  apiKey: "API_KEY",
  authDomain: "PROJECT_ID.firebaseapp.com",
  // The value of `databaseURL` depends on the location of the database
  databaseURL: "https://DATABASE_NAME.firebaseio.com",
  projectId: "PROJECT_ID",
  storageBucket: "PROJECT_ID.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID",
  // For Firebase JavaScript SDK v7.20.0 and later, `measurementId` is an optional field
  measurementId: "G-MEASUREMENT_ID",
};

const app = initializeApp(firebaseConfig);

// Get a reference to the database service
const database = getDatabase(app);

export default app;
```

### 데이터 읽기

```
import { getDatabase, ref, onValue} from "firebase/database";
import app from "firebase_config"; // 1

const db = getDatabase();
const starCountRef = ref(db, 'posts/' + postId + '/starCount');
onValue(starCountRef, (snapshot) => {
  const data = snapshot.val();
  updateStarCount(postElement, data);
});
```

1. `Error: No Firebase App '[DEFAULT]' has been created - call Firebase App.initializeApp()`가 발생할 수 있으므로 `initializeApp()`을 한 코드를 import 해 준다.
