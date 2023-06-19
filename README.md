# vite-ts-settings
Vite 를 사용한 ts 전용 기본 세팅

## 세팅 순서

### 1-1. 기본 포맷팅 플러그인 설치
```
npm i --save-dev eslint eslint-config-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react prettier
```

### 1-2. ts 템플릿일 때 추가 설치
```
npm i --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

### 2-1. tsconfig.json 설정
```
{
  "compilerOptions": {
    "types": ["node"],
     // es버젼을 사용
    "target": "ESNext",
     // class field를 사용
    "useDefineForClassFields": true,
     // 사용할 라이브러리를 설정
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
     // js를 사용하지 않겠다.
    "allowJs": false, 
     // 모든 선언 파일(*.d.ts)의 타입 검사를 건너뛰기
    "skipLibCheck": true,
    "esModuleInterop": false,
    // 런타임 바벨 생태계 호환성을 위한 __importStar와 __importDefault 헬퍼를 내보내고 타입 시스템 호환성을 위해 --allowSyntheticDefaultImports를 활성화합니다.
    "allowSyntheticDefaultImports": true, // default export가 없는 모듈에서 default imports를 허용. 코드 방출에는 영향을 주지 않으며, 타입 검사만 수행
     // strict mode에서 파싱하고 각 소스 파일에 대해 "use strict"를 내보냄
    "strict": true, 
     // 동일 파일 참조에 대해 일관성 없는 대소문자를 비활성화
    "forceConsistentCasingInFileNames": true, 
     // 모듈 코드 생성 지정: "ESNext"
    "module": "ESNext", 
     // 모듈 해석 방법 결정
    "moduleResolution": "Node", 
     // .json 확장자로 import된 모듈을 포함합니다
    "resolveJsonModule": true, 
     // 추가 검사를 수행하여 별도의 컴파일 (예를 들어 트랜스파일된 모듈 혹은 @babel/plugin-transform-typescript) 이 안전한지 확인
    "isolatedModules": true, 
    "noEmit": true,
    "jsx": "react-jsx",
     // baseUrl을 기준으로 관련된 위치에 모듈 이름의 경로 매핑 목록을 나열
    "baseUrl": ".", 
    "paths": {
      "@src/*": [
        //@src로 시작하면 아래 줄의 디렉토리
        "src/*" //baseUrl을 기준으로 src/ 하위 디렉토리를 @src로 표현
      ],
      "@components/*": ["src/components/*"] //@components로 시작하면 components/ 하위 디렉토리를 의미
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### 3-1. 절대 경로에 필요한 플러그인 설치
```
npm i -D vite-tsconfig-paths @types/node
```

### 3-2. 절대 경로 플러그인 수동 연결
```
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import tsconfigPaths from 'vite-tsconfig-paths';
import { resolve } from 'path';

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: [
      { find: '@src', replacement: resolve(__dirname, 'src') },
      {
        find: '@components',
        replacement: resolve(__dirname, 'src/components'),
      },
    ],
  },

  plugins: [react(), tsconfigPaths()],
});
```
혹, 위 과정이 아닌 자동으로 절대 경로를 지정하고 싶다면 *vite-tsconfig-paths* 모듈을 받아준다.

### 4-1. .eslintrc.js 설정
```
module.exports = {
  extends: [
    // By extending from a plugin config, we can get recommended rules without having to add them manually.
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:import/recommended',
    'plugin:jsx-a11y/recommended',
    // This disables the formatting rules in ESLint that Prettier is going to be responsible for handling.
    // Make sure it's always the last config, so it gets the chance to override other configs.
    'eslint-config-prettier',
  ],
  settings: {
    react: {
      // Tells eslint-plugin-react to automatically detect the version of React to use.
      version: 'detect',
    },
    // Tells eslint how to resolve imports
    'import/resolver': {
      node: {
        paths: ['src'],
        extensions: ['.js', '.jsx', '.ts', '.tsx'],
      },
    },
  },
  rules: {
    // Add your own rules here to override ones from the extended configs.
  },
};
```

### 4-2. .prettierrc 설정
```
{
  "semi": false,
  "trailingComma": "all",
  "singleQuote": true,
  "printWidth": 200,
  "endOfLine": "auto"
}
```
<hr />

