# 🌻throttling & debouncing🌻
>짧은 시간 간격으로 연속해서 이벤트가 발생했을 때 과도한 이벤트 핸들러 호출을 방지하는 기법

<br/>

## 🌼react-router-dom 설치🌼
```
yarn add react-router-dom
```

<br/>

## 🌼App.jsx🌼
![image](https://github.com/limhyerin/StudyNote/assets/70150896/79fca85f-041a-4144-b0c1-89fee6e54453)
```js
import './App.css';
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from './pages/Home';
import Company from './pages/Company';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />}/>
        <Route path="/company" element={<Company />}/>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

<br/>

## 🌼Home.jsx🌼
### 쓰로틀링 버튼 클릭시
![image](https://github.com/limhyerin/StudyNote/assets/70150896/1337ae8c-f086-4b91-b4e2-e27eaca7e903)

### 바운싱 버튼 클릭시
![image](https://github.com/limhyerin/StudyNote/assets/70150896/d959a694-3966-4c4a-bfcd-738c774a18fe)

```js
import { useEffect, useState } from "react";
import { useNavigate } from "react-router-dom";

export default function Home() {
  // const [state, setState] = useState(false);
  const navigate = useNavigate();
  let timerId = null;

  // Leading Edge Throttling
  const throttle = (delay) => {
    if (timerId) {
      // timerId가 있으면 바로 함수 종료
      return;
    }
    // setState(!state);
    console.log(`API요청 실행! ${delay}ms 동안 추가요청 안받음`);
    timerId = setTimeout(() => {
      console.log(`${delay}ms 지남 추가요청 받음`);
      // alert("Home / 쓰로틀링 쪽 API호출!");
      timerId = null;
    }, delay);
  };

  // Trailing Edge Debouncing
  const debounce = (delay) => {
    if (timerId) {
      // 할당되어 있는 timerId에 해당하는 타이머 제거
      clearTimeout(timerId);
    }
    timerId = setTimeout(() => {
      // timerId에 새로운 타이머 할당
      console.log(`마지막 요청으로부터 ${delay}ms지났으므로 API요청 실행!`);
      timerId = null;
    }, delay);
  };

  useEffect(() => {
    return () => {
      // 페이지 이동 시 실행
      if (timerId) {
        // 메모리 누수 방지
        clearTimeout(timerId);
      }
    };
  }, [timerId]);

  return (
    <div style={{ paddingLeft: 20, paddingRight: 20 }}>
      <h1>Button 이벤트 예제</h1>
      <button onClick={() => throttle(2000)}>쓰로틀링 버튼</button>
      <button onClick={() => debounce(2000)}>디바운싱 버튼</button>
			<div>
        <button onClick={() => navigate("/company")}>페이지 이동</button>
      </div>
    </div>
  );
}
```

<br/>

## 🌼Company.jsx🌼
### 페이지 이동 버튼 클릭시
![image](https://github.com/limhyerin/StudyNote/assets/70150896/fd45c161-7f2e-4340-b776-940adc59b462)

```js
import React from 'react';

export default function Company() {
  return (
    <div>
	    Test Page
    </div>
  );
}
```
