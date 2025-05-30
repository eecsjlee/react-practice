# CRA + Yarn → Vite + npm 마이그레이션 가이드

이 문서는 Create React App(CRA)과 Yarn으로 생성된 기존 프로젝트를 Vite와 npm을 사용하는 환경으로 마이그레이션하는 절차를 안내합니다. 빠른 번들링 속도와 현대적인 개발 경험을 위해 Vite 전환을 권장합니다.

## ✅ 1. 기존 프로젝트 백업

먼저 기존 CRA 프로젝트를 백업하거나 새 브랜치를 만들어 두세요.

```bash
git clone https://github.com/your-username/your-project
cd your-project
````

## 2. 기존 CRA 구성 제거

아래 항목들을 삭제하거나 초기화합니다.

```bash
rm -rf node_modules yarn.lock public src
```

필요하다면 `src`와 `public`은 Vite 구성에 맞게 재사용할 수 있습니다.

## 3. Vite 프로젝트 생성

Vite로 새 프로젝트를 초기화합니다.

```bash
npm create vite@latest
```

* 프로젝트 이름: 기존과 동일하게 유지
* 프레임워크: **React**
* Variant: **JavaScript** 또는 **TypeScript**

이후 종속성 설치:

```bash
cd your-project
npm install
```

## 4. 기존 코드 이전

CRA 프로젝트의 `src/` 디렉토리를 Vite 프로젝트로 복사하고, 필요한 경우 `public/`도 함께 이전합니다.

## 5. 주요 변경 사항 정리

### index.html

Vite는 루트 디렉토리에 `index.html`을 둡니다. 다음처럼 수정하세요:

```html
<body>
  <div id="root"></div>
  <script type="module" src="/src/main.jsx"></script>
</body>
```

### main.jsx

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

### 환경변수 접두사 변경

* CRA: `REACT_APP_`
* Vite: `VITE_`

예시:

```env
VITE_API_URL=https://api.example.com
```

### Vite 플러그인 설치 (필요 시)

```bash
npm install @vitejs/plugin-react
```

`vite.config.js`에 다음을 추가:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
})
```

## 6. 개발 서버 실행

```bash
npm run dev
```

## 7. 패키지 매니저 변경 요약

| 항목      | 변경 전 (CRA + Yarn) | 변경 후 (Vite + npm) |
| ------- | ----------------- | ----------------- |
| 빌드 시스템  | Webpack           | ESBuild (초고속)     |
| 패키지 매니저 | Yarn              | npm               |
| 실행 속도   | 보통                | 빠름                |
| 설정 유연성  | 낮음 (숨김 설정)        | 높음 (직접 설정)        |

## 참고 링크

* [Vite 공식 문서](https://vitejs.dev/)
* [Create React App → Vite 마이그레이션 참고](https://vitejs.dev/guide/)
* [환경변수 다루기 (Vite)](https://vitejs.dev/guide/env-and-mode.html)
