# Svelte(5) 개발환경 설치
<details>
<summary>접기/펼치기</summary>
<br>

## 요구 환경
- NodeJS (LTS 18버전 이상)
- svelte kit  
  svelte를 기반으로 하는 SSR 프레임워크 (React의 NextJS와 유사한 기능 제공)
  
## 설치 순서  
1. vite 초기화  
  Svelte에서 메인으로 사용하는 번들러    
    ```bash
    npm init vite ./
    ```

2. svelte 선택 후 Enter
    ```bash
    │
    ◆  Select a framework:
    │  ○ Vanilla
    │  ○ Vue
    │  ○ React
    │  ○ Preact
    │  ○ Lit
    │  ● Svelte
    │  ○ Solid
    │  ○ Qwik
    │  ○ Angular
    │  ○ Marko
    │  ○ Others
    ```
3. JavaScript 선택 후 Enter
    ```
      ◇  Select a framework:
    │  Svelte
    │
    ◆  Select a variant:
    │  ○ TypeScript
    │  ● JavaScript
    │  ○ SvelteKit ↗
    └
    ```
4. - rolldown vite 번들러 사용여부 - No 선택 후 Enter
    ```
    ◆  Use rolldown-vite (Experimental)?:
    │  ○ Yes
    │  ● No
    └
    ```
5. npm 설치 후 실행 - Yes 선택 후 Enter
    ```
    ◆  Install with npm and start now?
    │  ● Yes / ○ No
    └
    ```

</details>

<br>

# Svelte3 다운그레이드
<details>
<summary>접기/펼치기</summary>
<br>

## vite 버전 호환성 이슈

- 오류 로그
  ```
  You are using Node.js 18.15.0. Vite requires Node.js version 
  20.19+ or 22.12+. Please upgrade your Node.js version.       
  failed to load config from C:\Users\dq\diquest\study\inflearn\indiecoder-svelte-basic\vite.config.js
  error when starting dev server:
  file:///C:/Users/dq/diquest/study/inflearn/indiecoder-svelte-basic/node_modules/@sveltejs/vite-plugin-svelte/src/utils/log.js:4
  import { styleText } from 'node:util';
  		 ^^^^^^^^^
  SyntaxError: The requested module 'node:util' does not provide an export named 'styleText'
  	at ModuleJob._instantiate (node:internal/modules/esm/module_job:124:21)
  	at async ModuleJob.run (node:internal/modules/esm/module_job:190:5)
  PS C:\Users\dq\diquest\study\inflearn\indiecoder-svelte-basic> npm init vite@4.0.4 ./
  npm ERR! code ETARGET
  npm ERR! notarget No matching version found for create-vite@4.0.4.
  npm ERR! notarget In most cases you or one of your dependencies are requesting
  npm ERR! notarget a package version that doesn't exist.      
  
  npm ERR! A complete log of this run can be found in:
  npm ERR!     C:\Users\dq\AppData\Local\npm-cache\_logs\2025-11-22T07_57_35_418Z-debug-0.log
  ```

강의에서 나오는 설치방법은 vite 최신기준으로 svelte가 설치되기 때문에 
node 18.15.0에서 지원하지 않는 라이브러리인 styleText가 설치되지 않으므로 오류가 발생한다.
강의에서 사용중인 svelte 버전3이다.
### 의존성 버전 ([인프런 Indie Coder Svelte REST-API 프로젝트 강의 깃허브 참조](https://github.com/freeseamew/SLOG-FRONTEND-SVELTE/commit/71772eeeaab9e3c1363e0045fb6029fb8204b9d6#diff-7ae45ad102eab3b6d7e7896acd08c427a9b25b346470d7bc6507b6481575d519))
```json
{
  "devDependencies": {
    "@sveltejs/vite-plugin-svelte": "^2.0.0",
    "svelte": "^3.54.0",
    "tinro": "^0.6.12",
    "vite": "^4.0.0"
  }
}
```


## 설치 순서
1. 기존 svelte devDependencies 제거
    ```
    npm remove vite @sveltejs/vite-plugin-svelte svelte
    ```
    혹은 마지막 옵션인 Install with npm and start now?를 No로 설치한다
    ```
    ◆  Install with npm and start now?
    │  ○ Yes / ● No
    └
    ```


2. 강의 버전으로 재설치
    ```
    npm install -D vite@4.0.0 @sveltejs/vite-plugin-svelte@2.0.0 svelte@3.54.0
    ```

## vite3 기준 설정 및 코드수정
1. svelte.config.js 파일 제거
2. jsconfig.json 수정
    ```json
    {
      "compilerOptions": {
        "moduleResolution": "Node",
        "target": "ESNext",
        "module": "ESNext",
        /**
        * svelte-preprocess cannot figure out whether you have
        * a value or a type, so tell TypeScript to enforce using
        * `import type` instead of `import` for Types.
        */
        "importsNotUsedAsValues": "error",
        "isolatedModules": true,
        "resolveJsonModule": true,
        /**
        * To have warnings / errors of the Svelte compiler at the
        * correct position, enable source maps by default.
        */
        "sourceMap": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true,
        /**
        * Typecheck JS in `.svelte` and `.js` files by default.
        * Disable this if you'd like to use dynamic types.
        */
        "checkJs": true
      },
      /**
      * Use global.d.ts instead of compilerOptions.types
      * to avoid limiting type declarations.
      */
      "include": ["src/**/*.d.ts", "src/**/*.js", "src/**/*.svelte"]
    }
    ```
3. Counter.svelte 컴포넌트 수정
    - `let count = $state(0)`을 `let count = 0` 으로 수정
    - `<button onclick={increment}>`를 `<button on:click={increment}>` 으로 수정
4. main.js 코드 수정  
  mount()를 통해 루트 컴포넌트인 App을 마운트 하는것은 svelte5 버전에서 사용한다.  
  따라서 svelte3에서는 사용하지 않으므로 svelte3 방식으로 수정한다. 
  - AS-IS
    ```js
	import { mount } from 'svelte'
    import './app.css'
    import App from './App.svelte'
    
    const app = mount(App, {
      target: document.getElementById('app'),
    })
    
    export default app
	```
  - TO-BE
    ```js
    import './app.css'
    import App from './App.svelte'
    
    const app = new App({
      target: document.getElementById('app'),
    })
    
    export default app
    ```

  </details>