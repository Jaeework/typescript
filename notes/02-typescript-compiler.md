# TypeScript Compiler
<details>
<summary style="cursor: pointer; font-weight: bold; font-size: 1.2em;">📑 목차</summary>

- [1. TypeScript 컴파일 기본](#1-typescript-컴파일-기본)
    - [개별 파일 컴파일](#개별-파일-컴파일)
    - [프로젝트 단위 컴파일 설정](#프로젝트-단위-컴파일-설정)
- [2. tsconfig.json의 주요 옵션](#2-tsconfigjson의-주요-옵션)
    - [기본 설정](#기본-설정)
    - [파일 관리 설정](#파일-관리-설정)
    - [JavaScript 통합](#javascript-통합)
    - [디버깅 및 출력 옵션](#디버깅-및-출력-옵션)
    - [모듈 호환성](#모듈-호환성)
    - [타입 체크 설정](#타입-체크-설정)
    - [Code Quality Checks](#code-quality-checks)
- [3. 권장 기본 설정](#3-권장-기본-설정)
- [4. 컴파일러 설정 적용하기](#4-컴파일러-설정-적용하기)
    - [watch 모드](#watch-모드)
    - [외부 라이브러리 타입 패키지 설치](#외부-라이브러리-타입-패키지-설치)
    - [package.json 설정](#packagejson-설정)
- [5. 일반적인 컴파일 오류 해결](#5-일반적인-컴파일-오류-해결)
    - [모듈 임포트 오류](#모듈-임포트-오류)
    - [타입 선언 파일 오류](#타입-선언-파일-오류)
    - [컴파일 성능 개선](#컴파일-성능-개선)

</details>

## 1. TypeScript 컴파일 기본

### 개별 파일 컴파일

- `tsc 파일명.ts`를 사용하여 단일 TypeScript 파일을 JavaScript 파일로 컴파일
- 프로젝트 규모가 커질수록 개별 파일 컴파일 방식은 비효율적

### 프로젝트 단위 컴파일 설정

- TypeScript는 일반적으로 프로젝트 단위로 설정 구성
- `tsconfig.json` 파일을 통해 프로젝트 폴더와 하위 폴더의 컴파일 설정 관리
- `tsc --init` 명령어를 이용하여 간단하게 기본 설정 파일을 자동 생성
- 설정 파일이 있는 디렉토리에서 `tsc` 명령어만 실행하면 전체 프로젝트 컴파일

## 2. tsconfig.json의 주요 옵션

- 꽤 긴 설정 목록이 있으며, 목록의 오른편에 설정에 대한 자세한 설명이 있어 편리하다.
- 옵션에 대해 자세히 알아보려면 공식 페이지의 TSConfig 페이지를 통해 알 수 있음

### 기본 설정

1. disableReferencedProjectLoad
- 타입스크립트가 관련 프로젝트를 모두 메모리에 로드하지 않도록 하여 성능 향상
- 대규모 프로젝트에서 빌드/로드 속도 개선에 유용
2. target
- 컴파일 될 자바스크립트 버전 지정 (예: es2016, es2022)
- 최신 기능을 사용하면서도 특정 환경의 호환성 보장 가능
3. lib
- 타입스크립트가 인식하는 일부 표준 유형 라이브러리 지정
- 따로 설정하지 않으면 기본 표준 라이브러리가 포함됨. (DOM, HTML 라이브러리들 예시) HTMLElement)
4. jsx
- JSX 구문을 어떻게 처리할지 결정
- React 등 JSX 사용 시 필요한 설정
- 참조 : JSX는 JavaScript XML의 약자로, React 등의 라이브러리에서 UI 구성 요소를 선언적으로 작성할 수 있게 해주는 문법 확장
5. module
- 모듈 시스템 지정 (CommonJS, ES Modules 등)
- `NodeNext` : 최신 Node.js의 import/export 모듈 시스템 지원
- `preserve` : 웹팩이나 Vite 같은 번들러가 처리할 수 있도록 원본 형태 유지

### 파일 관리 설정

6. rootDir
- 소스파일이 포함된 폴더 지정
- 컴파일러가 TypeScript 파일을 찾는 기준 경로
7. outDir
- 컴파일 된 JavaScript 파일이 저장될 폴더 지정
- 소스 코드와 컴파일 된 코드 분리 관리
8. outFile
- 여러 파일을 하나의 파일로 묶어서 출력
- 일반적으로는 Webpack, Vite 등의 번들러를 사용한다.

### JavaScript 통합

9. allowJs
- 프로젝트에 js파일과 ts파일을 동시에 가질 수 있는지 true/false 설정
- 점진적인 TypeScript 도입 시 유용
10. checkJs
- 자바스크립트 파일에서도 기본적인 타입 검사 설정
- JS 파일을 TS로 마이그레이션하는 과정에 유용

### 디버깅 및 출력 옵션

11. sourceMap
- 디버깅에 유용한 소스맵 파일 생성
- 브라우저 개발 도구에서 컴파일된 JS가 아닌 원본 TS 코드 확인 가능
12. removeComments
- 컴파일 시 주석 제거 여부 설정
- 출력 파일 크기 감소에 도움
13. noEmitOnError
- 에러 발생 시 출력 파일 생성 여부
- true : 컴파일 오류 발생 시 JS 파일을 생성하지 않음. 컴파일이 정상적으로 완료된 경우에만 파일 생성
- false : 기본 값. 오류가 있어도 JS 파일 생성

### 모듈 호환성

14. esModuleInterop
- CommonJS와 ES 모듈 간 상호 운용성 개선
- 다양한 모듈 시스템 사용 시 호환성 문제 해결
15. forceConsistentCasingInFileNames
- 파일 이름의 대소문자 일관성 강제
- 대소문자를 구분하는 운영체제와 구분하지 않는 운영체제 간의 문제 방지

### 타입 체크 설정

1. strict
- 거의 모든 타입을 엄격하게 체크
- 나머지 모든 설정이 자동으로 true로 설정됨
2. noImplicitAny
- 암시적 any 타입 허용 여부
- true : 타입이 명시되지 않은 변수에 any 타입 자동 부여 금지. any 타입은 반드시 명시해줘야 함
3. strictNullChecks
- null과 undefined 값 처리 엄격화
- 잠재적인 null/undefined 오류 사전 방지
4. strictFunctionTypes
- 함수 매개변수 타입 검사 엄격화
- 함수 타입의 매개변수 처리를 더 엄격하게 검사

### Code Quality Checks

타입 관련은 아니지만, tsconfig.json에 같이 설정함으로써 코드의 퀄리티를 높일 수 있음

```json
{
  "noUnusedLocals": true, // 사용되지 않는 지역 변수 감지
  "noUnusedParameters": true, // 사용되지 않는 함수 매개변수 감지
  "noFallthroughCasesInSwitch": true // switch 문에서 break/return 없는 case 감지
}
```

## 3. 권장 기본 설정

vite 같은 build 도구를 사용하면, tsconfig.json은 이미 설정되어 있다. 하지만, 그러한 도구 없이 직접 설정하고 싶다면 다음 설정을 이용하면 좋을 것임

```json
{
  "target": "ES2022", // Good for modern browsers or Node.js
  "compilerOptions": {
    "esModuleInterop": true, // Ensures ESM and CJS imports work together well
    "skipLibCheck": true, // Ensures .d.ts files from 3rd libraries are not type-checked
    "target": "es2022", // Sets a relatively modern ECMAScript version as compilation target
    "allowJs": true, // Allows importing .js files into .ts (helpful when migrating projects)
    "strict": true, // Ensures strict type checking (i.e., noImplicitAny etc)
    "noUncheckedIndexedAccess": true, // Adds undefined as a value when accessing by index
    // "noImplicitOverride": true, // Enable this when working with classes & inheritance
    "noUnusedLocals": true, // to avoid unused variables
    "module": "NodeNext", // Supports both ESM & CJS modules / imports
    "outDir": "dist", // Store compiled files in "dist" folder
    "sourceMap": true, // Enables source maps for easier debugging
    "lib": ["es2022", "dom", "dom.iterable"] // Or without "dom" libs when building for Node
  }
}
```

## 4. 컴파일러 설정 적용하기

- 이전처럼, 개별 파일 하나하나 컴파일하면 이러한 설정이 적용되지 않는다. `tsc` 만 사용하여 프로젝트 전체에 설정 적용

### watch 모드

- `tsc --watch` 또는 `tsc -w` 명령어로 감시 모드 실행
- 파일 변경을 감지하여 자동으로 재컴파일

### 외부 라이브러리 타입 패키지 설치

- 기본적으로 Node.js나 외부 라이브러리는 TypeScript 타입 정의가 포함되어 있지 않음
- `@types` 패키지를 통해 타입 정의 설치 가능
    - `npm install -D @types/node` (Node.js 타입 정의)
- `--save-dev` 또는 `-D` 옵션 : 개발 의존성으로 패키지 설치 (프로덕션 빌드에 포함되지 않음)

### package.json 설정

- `type: "module"` 설정 : ESM(ES Modules) 사용 시 필요
- `"scripts"` 섹션에 유용한 자주 사용하는 명령어를 단축 키워드로 등록 가능

```json
"scripts": {
  "build": "tsc",
  "dev": "tsc --watch",
  "start": "node dist/index.js"
}
```

- scripts 사용 예시
  - `"build": "tsc"` : `npm run build` 명령어로 컴파일러를 실행해 TS파일을 JS로 반환
  - `"dev": "tsc --watch"` : `npm run dev` 명령어로 파일 변경 감지 모드로 컴파일러를 실행
  - `"start": "node dist/index.js"` : `npm start` 명령어로 컴파일된 JS 파일을 실행

## 5. 일반적인 컴파일 오류 해결

### 모듈 임포트 오류

- 모듈 시스템 호환성 문제는 `esModuleInterop: true`로 해결 가능. CommonJS 모듈을 ES Modules 스타일로 가져올 수 있게 해줌
- Node.js 관련 오류는 `@types/node` 패키지 설치로 해결

### 타입 선언 파일 오류

- 서드파티 라이브러리 사용 시 타입 선언 파일 부재:
    1. 해당 라이브러리의 `@types` 패키지 찾아 설치
    2. 또는 직접 타입 선언 파일(`.d.ts`)작성
  
```typescript
// 예: my-library.d.ts
declare module 'my-library' {
  export function someFunction(): void;
  export const someValue: string;
}
```

### 컴파일 성능 개선

- `skipLibCheck: true`로 설정하여 외부 라이브러리 타입 검사 생략
- 대규모 프로젝트에서 `incremental: true` 설정으로 증분 컴파일 활성화. 이전 컴파일 정보를 `.tsbuildinfo` 파일에 저장하고, 변경된 파일만 다시 컴파일
