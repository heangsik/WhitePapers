# SVELTE 셋팅

## 목차

- [SVELTE 셋팅](#svelte-셋팅)
  - [목차](#목차)
  - [프로젝트 설치](#프로젝트-설치)
  - [포트 변경](#포트-변경)
  - [tailwind (with SvelteKit)설치](#tailwind-with-sveltekit설치)
    - [Svelte](#svelte)
    - [SvelteKit](#sveltekit)
  - [Code Formatting](#code-formatting)
  - [Test](#test)
  - [Store](#store)
    - [Store를 선언 후 내부의 값을 확인 하는법](#store를-선언-후-내부의-값을-확인-하는법)
  - [SPA Router](#spa-router)
    - [설치](#설치)
    - [사용법](#사용법)
  - [vitejs root 설정](#vitejs-root-설정)
    - [vite.config.js 수정](#viteconfigjs-수정)
    - [jsconfig.json 수정](#jsconfigjson-수정)
  - [IConify](#iconify)
    - [설치](#설치-1)
    - [사용](#사용)
    - [참고](#참고)
  - [Supabase 사용](#supabase-사용)
    - [설치](#설치-2)
    - [사용](#사용-1)
  - [StoryBook](#storybook)
    - [설치](#설치-3)
    - [실행](#실행)

## 프로젝트 설치

- svelte
  ```
    npm create svelte@latest project_name
    cd procject name
    npm i
    npm run dev
  ```
- svelteKit

  ```
    npm init svelte@next project_name
    cd project_name
    npm i
    npm run dev
  ```

- vite
  ```
    npm create vite@latest project_name (enter)
    Need to install the following packages:
      create-vite@5.0.0
    Ok to proceed? (y) (enter)
    ? Select a framework: Svelte 선택
    ? Select a variant: » SvelteKit 선택(이건 알아서 해라)
  ```

## 포트 변경

```json
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack",
    "dev": "webpack serve --content-base public --port 5566"
  }
```

## tailwind (with SvelteKit)설치

### Svelte

- 설치
  ```shell
    npm install -D tailwindcss postcss autoprefixer svelte-preprocess
    npx tailwindcss init tailwind.config.cjs -p
  ```
- 설정 변경
  - tailwind.config.cjs
    ```js
    /** @type {import('tailwindcss').Config} */
    module.exports = {
      // **here**
      content: ['./src/**/*.{html,js,svelte,ts}'], <-- 설정 변경
      theme: {
        extend: {},
      },
      plugins: [],
    }
    ```
- 파일 수정
  - /src/app.css
    ```css
    /*파일 가장 위에 추가*/
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

### SvelteKit

- 설치

  - 입력

  ```
    npm i -D tailwindcss postcss autoprefixer daisyui
    npx tailwindcss init -p
  ```

  - 출력

  ```cmd
  Created Tailwind CSS config file: tailwind.config.js
  Created PostCSS config file: postcss.config.js
  ```

- path 설정
  - tailwind.config.js 수정
    ```js
    module.exports = {
      content: ["./src/**/*.{html,js,svelte,ts}"], // <-- 수정부분
      theme: {
        extend: {},
      },
      plugins: [require("daisyui")], // <--- 수정
    };
    ```
  - svelte.config.js 수정
    ```js
    import adapter from "@sveltejs/adapter-auto";
    import { vitePreprocess } from "@sveltejs/vite-plugin-svelte"; // <--- 추가
    /** @type {import('@sveltejs/kit').Config} */
    const config = {
      kit: {
        adapter: adapter(),
      },
      preprocess: vitePreprocess(), // <--- 추가
    };
    export default config;
    ```
- styles.css 수정

  ```js
    //./src/routes/styles.css 수정
    @import "@fontsource/fira-mono";
    @tailwind base; // <<- 추가
    @tailwind components; // <<- 추가
    @tailwind utilities; // <<- 추가

    :root {
      --font-body: Arial, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
        Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
      --font-mono: "Fira Mono", monospace;
      --color-bg-0: rgb(202, 216, 228);
      --color-bg-1: hsl(209, 36%, 86%);
      --color-bg-2: hsl(224, 44%, 95%);
      --color-theme-1: #ff3e00;
      --color-theme-2: #4075a6;
      --color-text: rgba(0, 0, 0, 0.7);
      --column-width: 42rem;
      --column-margin-top: 4rem;
      font-family: var(--font-body);
      color: var(--color-text);
    }
    // .......
  ```

## Code Formatting

- extention prettier-code formatter 설치
- file->preferences->setting
- Default Fomatter 검색 후 prettier-code formatter 선택
- format on save 검색 후 체크 박스 선택
- extention Svelte for VS Code 설치
  - setting.json(User) 내용추가
  ```JSON
  {
    .....~~~~~
    "editor.formatOnSave": true,
    "[svelte]": {
      "editor.defaultFormatter": "svelte.svelte-vscode"
    }
  }
  ```

## Test

- vitest 설치
  > npm i -D vitest  
  > npm i -D @testing-library/svelte jest-dom jsdom  
  > npm i -D @vitest/ui
- 실행 스크립트 추가
  ```js
  # package.json 내용 수정
    "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest" <-- 추가
  },
  ```
- vitest 설정파일 추가

  - 프로젝트 root에 파일 추가

  ```js
  // vite.config.js
  import { defineConfig } from "vite";
  import { svelte } from "@sveltejs/vite-plugin-svelte";

  export default defineConfig({
    plugins: [svelte({ hot: !process.env.VITEST })],
    test: {
      globals: true,
      environment: "jsdom",
    },
  });
  ```

## Store

### Store를 선언 후 내부의 값을 확인 하는법

> 간단한 store인 경우 선언 후 바로 값을 읽어 들일 수 있지만 store내부에 로직이 있는 경우는 불가능 하다.
> 이때 선언한 값 앞에 $를 붙이면 저장된 값을 직접 접근 할수 있다

## SPA Router

### 설치

- npm install svelte-spa-router

### 사용법

- Router 생성

```js
<script>
  import Home from "./lib/components/Home.svelte";
  import About from "./lib/components/About.svelte";
  import News from "./lib/components/News.svelte";
  import Router from "svelte-spa-router";
  import Detail from "./lib/components/Detail.svelte";

  const routes = {
    "/": Home,
    "/about": About,
    "/news": News,
    "/detail/:id": Detail,
  };
</script>
```

- 링크 연결

```js
<script>
  import { link } from "svelte-spa-router";
</script>
  <ul>
    <li><a use:link href="/">Home</a></li>
    <li><a use:link href="/about">About</a></li>
    <li><a use:link href="/news">News</a></li>
    <li><a use:link href="/contact">Contact</a></li>
  </ul>
  -- 또는
  <ul>
    <li><a href="#/">Home</a></li>
    <li><a href="#/about">About</a></li>
    <li><a href="#/news">News</a></li>
    <li><a href="#/contact">Contact</a></li>
  </ul>
```

## vitejs root 설정

### vite.config.js 수정

```JS
import { defineConfig } from "vite";
import { svelte } from "@sveltejs/vite-plugin-svelte";
import { configDefaults } from "vitest/config";

export default defineConfig({
  plugins: [svelte()],
  // 아래 내용 추가
  resolve: {
    alias: {
      $: "/src",
      $lib: "/src/lib", // 만약 더 추가 하고 싶으면 추가
      },
  },
});

```

### jsconfig.json 수정

```JSON

  "compilerOptions": {
    // 아래 내용 추가
    "baseUrl": ".",
    "paths": {
      "$/*": ["src/*"]
    }
  },
```

## IConify

### 설치

```bash
npm i -D @iconify/svelte
```

### 사용

```html
<script>
  import Icon from "@iconify/svelte";
</script>

<div>
  <Icon icon="mdi:home" style="font-size: 24px;" />
</div>
```

### 참고

[iconify svelte](https://iconify.design/docs/icon-components/svelte/#installation)

## Supabase 사용

### 설치

```bash
npm i @supabase/supabase-js
```

### 사용

## StoryBook

### 설치

```bash
npx storybook@latest init

```

### 실행

```bash
npm run storybook
```

[참고사이트](https://storybook.js.org/docs/get-started/frameworks/sveltekit?renderer=svelte)
