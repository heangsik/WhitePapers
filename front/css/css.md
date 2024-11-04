# CSS

- [CSS](#css)
  - [SCSS 설정](#scss-설정)
    - [설치](#설치)
    - [사용](#사용)
  - [Tailwind SCSS 동시사용](#tailwind-scss-동시사용)
    - [설치](#설치-1)
    - [설정](#설정)
  - [TIPS](#tips)
    - [하단에 베너 고정 시키기](#하단에-베너-고정-시키기)
    - [화명 중앙에 배치 하기](#화명-중앙에-배치-하기)

## SCSS 설정

### 설치

> npm install -D sass

### 사용

- svelte.config.js 수정

```js
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

/** @type {import('@sveltejs/kit').Config} */
const config = {
	preprocess: vitePreprocess({ scss: true }), <-- 수정

	kit: {
		adapter: adapter()
	}
};

export default config;

```

- src/app.scss파일 생성

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

- +layout.svelte 수정

  ```js
  ....
  <style lang="scss" global>
    @use "../app.scss"; <-- import에서 use로 변경됨
  </style>

  ```

- +page.svelte 수정

  ```css
  <style lang="scss">
    header {
      text-align: center;
      padding: 25px;
      margin-bottom: 30px;
      .logo {
        margin-bottom: 30px;
      }
      nav {
        ul {
          list-style: none;
          li {
            display: inline-block;
            margin: 0 1em;
            a {
              text-decoration: none;
              color: #333;
              &:hover {
                opacity: 0.7;
              }
            }
          }
        }
      }
    }
  </style>
  ```

## Tailwind SCSS 동시사용

### 설치

```bash
npm install -D tailwindcss postcss autoprefixer sass@1.79.6
npx tailwindcss init -p
```

- sass@1.79.6를 사용하는 이유는 1.80부터 사용법이 변경되기 시작한다.

### 설정

- tailwind.config.js 수정

  ```js
  /** @type {import('tailwindcss').Config} */
  export default {
    content: ['./src/**/*.{html,js,svelte,ts,scss,css}'], <-- 수정
    theme: {
      extend: {}
    },
    plugins: []
  };
  ```

- app.scss 수정

  - app.css -> app.scss로 변경

  ```css
  @tailwind base;       <-- 추가
  @tailwind components; <-- 추가
  @tailwind utilities;  <-- 추가

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  ```

- svelte.config.js 수정

  ```js
  import adapter from '@sveltejs/adapter-auto';
  import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

  /** @type {import('@sveltejs/kit').Config} */
  const config = {
    preprocess: vitePreprocess({
      scss: true,     <-- 추가
    }),

    kit: {
      adapter: adapter()
    }
  };

  export default config;

  ```

- layout.svelte tnwjd

  ```css
  ....
  <style>
    @import '../app.scss';<-- 추가

   // 사용 예시
    .custom-component {
    // Tailwind의 믹스인 사용
    @apply text-lg font-bold p-4;

    // SCSS 문법과 혼합 사용
    &:hover {
      @apply bg-blue-500;
      transform: scale(1.1);
    }

    // 미디어 쿼리에서도 활용 가능
    @screen md {
      @apply text-xl p-6;
    }
  }
  </style>
  ```

## TIPS

### 하단에 베너 고정 시키기

```html
<footer>
  <small>Copyright B-Boy Brother. All Right reserved</small>
</footer>

<style lang="scss">
  footer {
    background-color: #444; // 배경을 검은색으로 한다.
    color: #fff; // 폰트 색은 흰색
    text-align: center; // 문자열을 가로 기준으로 가운데
    align-items: center; // 문자열을 세로 기준으로 가운데
    padding: 1rem; // 문자열에 페딩을 준다.
    position: fixed; // 아래에 고정 시킨다.
    bottom: 0; // 고정시키는 위치
    width: 100%; // 넓이
  }
</style>
```

### 화명 중앙에 배치 하기

```html
<body>
  <h2>테스트 문구</h2>
</body>
<style>
  body {
    display: grid;
    place-items: center;
    height: 100vh;
  }

  or body {
    display: grid;
    align-items: center;
    justify-items: center;
  }
</style>
```

- 둘다 같은 결과가 나온다.
