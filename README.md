# React
> React is a JavaScript library for rendering user interfaces (UI)
* facebook에서 개발한 자바스크립트 라이브러리
## 특징
1. 컴포넌트 기반: 화면을 여러 개의 컴포넌트로 나누어서 개발 가능
2. 가상 dom: 화면이 변경 될 때 전체 html을 그리는 대신 변경된 부분만 업데이트
3. 선언형 프로그래밍: html을 직접 조작하지 않음, 원하는 ui상태를 선언하면 React가 알아서 업데이트(예: count값이 변경되면 React가 알아서 변경)
4. React hooks: 클래스 컴포넌트 없이도 상태(state) 생명주기(lifecycle)를 다룰 수 있도록 도와주는 기능(useState, useEffect 등)
5. React + SPA: 한 개의 html 페이지에서 여러 개의 화면을 동적으로 변경하는 SPA에 많이 활용됨

### 왜 프레임워크가 아닌가
1. 필수적인 기능이 부족함
    * 프레임워크는 일반적으로 라우팅, 상태 관리, API 호출, 빌드 시스템 같은 기능을 자체적으로 제공
    * React는 UI 렌더링에 집중된 라이브러리일 뿐, 라우팅(react-router), 상태 관리(Redux, Recoil, Zustand 등), API 호출(Axios, fetch) 같은 걸 직접 포함하고 있진 않다.
2. 자유도가 높음
    * React는 특정 방식으로 개발을 강요하지 않는다. 원하는 라이브러리와 조합해서 개발 스타일을 자유롭게 정할 수 있다.
    * 반면, 프레임워크(Vue, Angular 등)는 보통 자체적인 개발 방식이 정해져 있다.
3. 프레임워크는 전체적인 구조를 잡아주고, 라이브러리는 특정 기능을 제공
    * React는 UI 개발을 쉽게 해주는 라이브러리일 뿐, 전체적인 애플리케이션 구조를 관리하는건 개발자의 선택
    * Vue, Angular 같은 프레임워크는 앱의 구조까지 정해줘서 처음부터 끝까지 일관된 방식으로 개발

### 추천하는 방식
> If you want to build a new app or website with React, we recommend starting with a framework.

* React 공식 레퍼런스에서는 새로 시작하는 경우 프레임워크로 시작하는 것을 추천하고 있다 (Next.js 등)
* 레퍼런스에서 소개하는 풀스택 프레임워크는 CSR(클라이언트렌더링)), SPA(단일페이지앱), SSG(정적사이트렌더링)를 지원하므로 서버가 꼭 필요하지 않다.

## 컴포넌트
* UI를 구성하는 독립적인 조각
* 버튼, 입력 폼, 네비게이션 바 같은 것들을 각각 컴포넌트로 만들고, 이를 조합하여 화면을 구성함
### 특징
1. 재사용 가능: 여러 곳에서 컴포넌트를 재사용할 수 있음
2. 독립적: 각각의 컴포넌트는 자체적인 상태와 기능을 가짐
3. 계층 구조: 컴포넌트 안에 다른 컴포넌트를 포함할 수 있음
### 구현 방법
* 일전에는 클래스형 컴포넌트가 많이 쓰였지만 현재는 함수형 컴포넌트와 Hooks가 대체함
* 꼭 대문자로 시작해야함
```javascript
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}

export default Counter;
```
* 작성한 컴포넌트를 다른 컴포넌트에 중첩 사용 할 수 있다.
```javascript
import Counter from "../components/counter";

export default function CountPage() {
  return (
    <div>
      <Counter></Counter>
      <Counter></Counter>
    </div>
  )
}
```

## JSX(Javascript Syntax eXtension)
* React에서 UI를 만들 때 사용하는 문법
### 특징
* HTML와 비슷하거나 같아보이지만 Javascript 코드 안에서 사용됨
* class 속성 대신 className을 사용해야 함
* 태그를 반드시 닫아야함
    * `<img />`, `<br />`같은 태그도 반드시 닫아야 함
* Javascript 표현식을 `{}`안에서 사용 가능
```javascript
const name = "React";

function App() {
  return <h1>Hello, {name}!</h1>;
}

export default App;
```
### 실제 변환되는 모습
* JSX는 과거에는 내부적으로 `React.createElement()` 함수로 변환되었다.
    * https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform
```javascript
const element = React.createElement("h1", null, "Hello, ", name, "!");
```
* React 17부터는 다음과 같이 변환한다.
```javascript
// Inserted by a compiler (don't import it yourself!)
import {jsx as _jsx} from 'react/jsx-runtime';

function App() {
  return _jsx('h1', { children: 'Hello world' });
}
```
* JSX를 사용하면 코드가 훨씬 직관적이다.

### 규칙
1. 단일 루트 요소
    * `<div></div>` 또는 `<></>` 와 같은 단일 부모 태그로 요소를 래핑해야함
    ```javascript
    <div>
      <h1>Hedy Lamarr's Todos</h1>
      <img
        src="https://i.imgur.com/yXOvdOSs.jpg"
        alt="Hedy Lamarr"
        class="photo"
      >
      <ul>
        ...
      </ul>
    </div>
    ```
    * 빈 `<></>`를 `Fragment`라고 함
2. 모든 태그를 닫아야한다
3. 대부분의 것 (속성)을 camelCase로 표현해야 한다
    * 기존 javascript에는 변수 이름에 제한이 있다. 예를 들어 대시를 포함하거나`stroke-width`, 예약어를 쓴다거나`class`
    * 이러한 부분이 React에서 많은 HTML, SVG속성을 camelCase로 작성하는 이유다
    * `class`의 경우는 예약어 이므로 `className`를 사용한다.
    ```javascript
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      className="photo"
    />
    ```
### 개꿀팁
* html을 그냥 가져다 jsx에 붙히면 안되는 경우가 많다.
* 제약조건이 html보다 많기 때문
* https://transform.tools/html-to-jsx 변환기를 쓰면 보다 간편하게 사용 가능

## JSX 활용
### 따옴표로 문자열 전달
JSX에 문자열 속성은 작은따옴표나 큰따옴표로 묶는다.
```javascript
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="/sample.png"
      alt="sample"
    />
  );
}
```
* 해당 예제에서는 `"/sample.png"` 및 `"sample"`이 문자열로 전달된다.
* 동적으로 전달 시에는 `{}`로 묶으면 된다.
```javascript
export default function Avatar() {
  const src = '/sample.png'
  const desc = 'sample'
  return (
    <img
      className="avatar"
      src={src}
      alt={desc}
    />
  );
}
```

### javascript 전달
```javascript
const today = new Date();

function formatDate(date: Date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function Page() {
  return (
    <h1>Today Is {formatDate(today)}</h1>
  );
}
```
* 중괄호 안에 javascript 표현식을 사용 가능하다

### css 및 기타 객체 전달
```javascript
export default function Page() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```
* 중괄호를 2개 중첩하여 사용하여 객체 및 css 등을 전달할 수 있다
* css의 경우 인라인 스타일을 사용할 수 있지만 대부분의 경우 클래스로 적용한다
* css 인라인 스타일의 경우 camelCase로 작성해야하는 점 주의