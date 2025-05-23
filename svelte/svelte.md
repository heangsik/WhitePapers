# SVELTE 셋팅

## 목차

- [SVELTE 셋팅](#svelte-셋팅)
  - [목차](#목차)
  - [프로젝트 설치](#프로젝트-설치)
    - [svelte](#svelte)
      - [svelteKit](#sveltekit)
  - [포트 변경](#포트-변경)
  - [bootstrap](#bootstrap)
    - [설치](#설치)
    - [사용](#사용)
      - [CSS](#css)
      - [SCSS](#scss)
  - [BUILD경로 변경](#build경로-변경)
  - [배포시 주의점](#배포시-주의점)
  - [구동 모드](#구동-모드)
    - [env 분리](#env-분리)
  - [tailwind (with SvelteKit)설치](#tailwind-with-sveltekit설치)
    - [Svelte](#svelte-1)
    - [SvelteKit](#sveltekit-1)
  - [Flowbite](#flowbite)
    - [설치](#설치-1)
    - [설정](#설정)
  - [Code Formatting](#code-formatting)
  - [Test](#test)
  - [Store](#store)
    - [Store를 선언 후 내부의 값을 확인 하는법](#store를-선언-후-내부의-값을-확인-하는법)
  - [SPA Router](#spa-router)
    - [설치](#설치-2)
    - [사용법](#사용법)
  - [vitejs root 설정](#vitejs-root-설정)
    - [vite.config.js 수정](#viteconfigjs-수정)
    - [jsconfig.json 수정](#jsconfigjson-수정)
  - [경로 별칭 설정](#경로-별칭-설정)
  - [비동기 처리](#비동기-처리)
  - [IConify](#iconify)
    - [설치](#설치-3)
    - [사용](#사용-1)
    - [참고](#참고)
  - [Supabase 사용](#supabase-사용)
    - [설치](#설치-4)
    - [사용](#사용-2)
  - [StoryBook](#storybook)
    - [설치](#설치-5)
    - [실행](#실행)

## 프로젝트 설치

### svelte

```
  npm create svelte@latest project_name
  cd procject name
  npm i
  npm run dev
```

#### svelteKit

- npm

  ```
    npx sv create  project_name
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

## bootstrap

### 설치

> npm i bootstrap

### 사용

#### CSS

- +layout.svelte 수정

  ```js
    <script>
      import { onMount } from 'svelte';
      import 'bootstrap/dist/js/bootstrap.bundle.min.js';
    </script>

    <slot />
  ```

#### SCSS

- app.scss 수정
  ```css
  @import "bootstrap/scss/bootstrap";
  ```
- 만약 Deprecation Warning 발생시 sass를 1.49.0으로 설치한다.
  > npm i sass@1.49.0

## BUILD경로 변경

- svelte.config.js 수정

  ```js
  import adapter from "@sveltejs/adapter-auto";

  /** @type {import('@sveltejs/kit').Config} */
  const config = {
    kit: {
    + adapter: adapter({ out: "build" }), // <-- 수정
    },
  };

  export default config;
  ```

## 배포시 주의점

- 배포 후 구동시 프로젝트가 구동이 안되는경우가 있다.
- 이경우 설정 파일의 adapter를 설치 후 변경해줘야 한다.

  - adapter 설치

  ```
    npm i -D @sveltejs/adapter-node` 또는
    npm i -D @sveltejs/adapter-vercel 등 더 있다.
  ```

  - svelte.config.js 수정

  ```js
  // import adapter from '@sveltejs/adapter-auto';
  import adapter from "@sveltejs/adapter-node"; //<-- 수정

  /** @type {import('@sveltejs/kit').Config} */
  const config = {
    kit: {
      adapter: adapter(),
    },
  };

  export default config;
  ```

## 구동 모드

```json
    "dev": "vite dev --mode prod",
    "build": "vite build --mode prod",
```

### env 분리

| 파일      | 모드             | 구동법              |
| --------- | ---------------- | ------------------- |
| .env      | 일반 구동시      | npm run dev         |
| .env.prod | prod 모드 구동시 | npm run --mode prod |

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

## Flowbite

### 설치

- npm i -D flowbite-svelte flowbite flowbite-svelte-icons

### 설정

- tailwind.config.cjs 수정

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{html,js,svelte,ts,scss,css}",
    "./node_modules/flowbite-svelte/**/*.{html,js,svelte,ts}" < --추가,
  ],
  theme: {
    extend: {},
  },
  plugins: [require('flowbite/plugin')], <-- 수정
};
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

## 경로 별칭 설정

- 안해봤음

```js
import { sveltekit } from "@sveltejs/kit/vite";
import path from "path";

/** @type {import('vite').UserConfig} */
const config = {
  plugins: [sveltekit()],
  // 아래 내용 추가
  resolve: {
    alias: {
      $components: path.resolve("./src/components"),
      $assets: path.resolve("./src/assets"),
    },
  },
};

export default config;
```

## 비동기 처리

- #await를 사용하여 처리 한다.

```js
<script>
import Button from './Components/Button.svelte'
let promiseValue = new Promise((res)=>{
	setTimeout(()=>{
		res("DONE!")
	},1500)
});
</script>

{#await promiseValue}
	Loading...
{:then value}
	{value}
{/await}

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
or
npx sb@latest init

```

### 실행

```bash
npm run storybook
```

[참고사이트](https://storybook.js.org/docs/get-started/frameworks/sveltekit?renderer=svelte)

- MockDate
  - [link](https://github.com/boblauer/MockDate)
