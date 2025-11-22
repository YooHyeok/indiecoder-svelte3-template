# Svelte 개발환경 설치
- 요구 환경
  - NodeJS (LTS 18버전 이상)
  - svelte kit  
    svelte를 기반으로 하는 SSR 프레임워크 (React의 NextJS와 유사한 기능 제공)
  
  
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