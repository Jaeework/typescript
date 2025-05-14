# Getting Started and Basic Built-in Types
<details>
<summary style="cursor: pointer; font-weight: bold; font-size: 1.2em;">📑 목차</summary>

- [1. Getting Started](#1-getting-started)
    - [TypeScript 소개](#typescript-소개)
- [2. 기본 내장 타입](#2-기본-내장-타입)
    - [타입 지정 방법](#타입-지정-방법)
        - [1. 명시적 유형 할당](#1-명시적-유형-할당)
        - [2. 타입 추론](#2-타입-추론)
        - [기본 타입](#기본-타입)
        - [변수 외에 타입 할당이 가능한 곳은 어디일까?](#변수-외에-타입-할당이-가능한-곳은-어디일까)
    - [고급 타입 기능](#고급-타입-기능)
        - [유연한 타입 적용이 필요할 경우](#유연한-타입-적용이-필요할-경우)
        - [다중 유형이 아닌 다중 값을 사용할 경우](#다중-유형이-아닌-다중-값을-사용할-경우)
        - [Tuple 타입](#tuple-타입)
        - [Object 타입](#object-타입)
        - [Typescript의 이상한 기능](#typescript의-이상한-기능)
        - [Object 타입임을 명시하고 싶을 때는?](#object-타입임을-명시하고-싶을-때는)
        - [enum : 열거형 타입](#enum--열거형-타입)
        - [열거형의 사용되는 대안 : literal 타입](#열거형의-사용되는-대안--literal-타입)
        - [Type alias](#type-alias)
        - [함수의 return 타입](#함수의-return-타입)
        - [void 타입](#void-타입)
        - [never 타입](#never-타입)
        - [Function 타입](#function-타입)
        - [Special 타입 1 : null, undefined](#special-타입-1--null-undefined)
        - [Type Casting](#type-casting)
        - [Special 타입 2 : unknown type](#special-타입-2--unknown-type)
        - [optional](#optional)
        - [Nullish Coalescing](#nullish-coalescing)

</details>

# 1. Getting Started

## TypeScript 소개

### TypeScript란?

- JavaScript의 상위 집합(superset)
- JavaScript 언어의 확장 버전
- 주요 특징 : 정적 타입 지원

### TypeScript 사용 이유

- 코드 품질과 가독성 향상
- 오류 감지 및 디버깅 용이성
- 개발 도구 지원 강화

### TypeScript 단점

- 브라우저에서 직접 실행 불가능
- JavaScript로의 컴파일 과정이 필수

# 2. 기본 내장 타입

## 타입 지정 방법

### 1. 명시적 유형 할당

- 콜론과 타입을 사용한 선언 (타입 어노테이션)
- 예) `let userName: string;`

### 2. 타입 추론

- 초기값 할당 시 자동으로 타입 추론
- 예) `let userAge = 38;`(자동으로 number 타입 추론)
- 추론 가능한 경우 명시적 타입 지정은 불필요한 것이 관행

### 기본 타입

- `string` : 문자열
- `number` : 숫자
- `boolean` : 참/거짓

### 변수 외에 타입 할당이 가능한 곳은 어디일까?

함수에서의 타입 할당

```typescript
function add(a: number, b = 5) {
	return a + b;
}
```

## 고급 타입 기능

### 유연한 타입 적용이 필요할 경우

1. 명시적 유형 할당 : `any`

```typescript
let age: any = 36;

age = '37';
age = false;
age = {};
age = [];
```

- 타입스크립트의 장점을 없앰
2. **유니온 타입**

```typescript
let age: string | number = 36;

age = '37'
```

- 여러 타입 중 하나 허용

### 다중 유형이 아닌 다중 값을 사용할 경우

배열, 객체 in javascript 에도 타입이 적용된다는 것이 장점

```typescript
let hobbies = ['Sports', 'Cooking']
// 자동으로 string[] 로 타입 추론

// 유니언 타입의 배열을 만들고 싶을 때,
let users: (string | number)[];

users = [1, 'Max'];

// Generic 유형
let users: Array<string | number>;
```

### Tuple 타입

유형을 명확하게 알고 있는 고정 길이 배열을 처리하는 경우에 사용

```typescript
let possibleResults: [number, number] // [1, -1]
// 정확히 2개의 number 유형 값을 가져야 한다고 명시
```

### Object 타입

```typescript
let user: {
    name: string;
    age:number | string;
    hobbies: string[];
    role: {
        description: string;
        id: number;
    }
} = {
    name: 'Max',
    age: 38,
    hobbies: ['Sports', 'Cooking'],
    role: {
        description: 'admin',
        id: 5
    }
};
```

### Typescript의 이상한 기능

```typescript
let val: {} = 'some text';

// javascript에서의 {}는 빈 값의 객체 타입
// but typescript에서는 null, undefined(정의되지 않은)가 아닌 모든 유형을 뜻함
```

### Object 타입임을 명시하고 싶을 때는?

- Record 타입 : actually Generic type. 올바르게 작용하려면 추가 유형 정보가 필요함

```typescript
let data: Record<>;
```

JavaScript에서 허용되는 key 유형 : 기본값은 string, number도 허용. Symbol도 있지만 현재는 무시.

### enum : 열거형 타입

특정 종류의 옵션만 허용하는 유형. 일종의 사용자 정의 타입

```typescript
enum Role {
	Admin, // = 0
	Editor, // = 1
	Guest // = 2
}

let userRole: Role = Role.Admin;
// = let userRole: Role = 0;

userRole = Role.Guest;
```

```typescript
// compile 된 js 파일
var Role;
(function (Role) {
    Role[Role["Admin"] = 0] = "Admin";
    Role[Role["Editor"] = 1] = "Editor";
    Role[Role["Guest"] = 2] = "Guest";
})(Role || (Role = {}));
var userRole = Role.Admin;
userRole = Role.Guest;
```

```typescript
// 해당하는 number를 override 할 수도 있음
enum Role {
	Admin = 1, // = 1
	Editor, // = 2
	Guest // = 3
}

let userRole: Role = 0;
// 에러 남.

// 문자열을 할당할 수도 있는데, 그 경우에는 전부 할당해줘야 함
// 문자열은 그 다음 값을 예측할 수 없기 때문
enum Role {
	Admin = "Admin",
	Editor = "Editor",
	Guest = "Guest"
}
```

### 열거형의 사용되는 대안 : literal 타입

구체적인 값을 타입으로 적용할 수 있다. (유니온 타입과의 결합)

```typescript
let userRole: 'admin' | 'editor' | 'guest' = 'admin';

userRole = 'guest'
```

튜플과의 조합

```typescript
let possibleResults: [1|-1, 1|-1] // [1, -1]

possibleResults = [1, -1]
```

### Type alias

여러 곳에서 사용되는 literal, 유니언 타입에 유용한 기능

```typescript
type Role = 'admin' | 'editor' | 'guest' | 'reader';

let userRole: Role = 'admin';

function access(role: Role) {
}

// 객체 유형 정의에도 유용하게 쓰임
type User = {
	name: string;
	age: number;
	role: Role;
	permissions: string[];
}
```

### 함수의 return 타입

소괄호와 중괄호 사이에 함수의 return 타입을 :을 이용해 정의할 수 있다.

```typescript
function add(a: number, b: number): number {
    return a + b;
}
```

### void 타입

this function returns nothing

```typescript
function log(message: string) {
	console.log(message);
}
```

### never 타입

절대 반환되지 않는 타입.

```typescript
function logAndThrow(errorMessage: string): never {
    console.log(errorMessage);
    throw new Error(errorMessage);
}
```

- void와의 차이
    - void는 함수가 완료된 후, 아무것도 반환하지 않는다는 것을 의미
    - 예시의 오류 발생 함수의 경우, 함수가 완료되는 것이 아니라 오류로 인해 멈춘 것이기 때문에, 다른 곳에서 포착하지 않으면 프로그램이 충돌할 수 있다.
    - never 타입으로 명시적으로 설정하면, 다른 곳에서 변수나 상수에 해당 함수의 반환 값을 저장하려고 시도하는 일이 없도록 할 수 있다.

### Function 타입

자바스크립트에서는 함수가 값이 되기도 한다. 함수를 변수의 값으로 저장할 수 있거나 다른 함수의 매개변수로 전달할 수 있다.

타입스크립트에서는 함수를 타입 종류 중 하나로 제공한다.

```typescript
// callback 함수 : 주작업이 끝난 후, 실행되어야 할 작업들을 정의한 함수
function performJob(cb: Function) {
    // ... 주 작업
    cb();
}
// too 모호함. 정확히 지정해주는 것이 좋음

const logMsg = (msg: string) => {
	console.log(msg);
}

function performJob(cb: (msg: string) => void) { 
// 위의 arrow 함수 logMsg와 다르게, 함수를 생성한다는 의미가 아니라 함수의 유형을 정의한 것임
	// ...
	cb('Job done!');
}

performJob(log);
```

오브젝트 객체에도 유용하게 사용

```typescript
type User = {
    name: string;
    age: number;
    greet: () => string;
};

let user: User = {
    name: 'Max',
    age: 39,
    greet() {
        console.log('Hello there!');
        return this.name;
    }
}

user.greet();
```

### Special 타입 1 : null, undefined

혼자 사용되기보다는 유니언 타입으로 사용됨

```typescript
let a : null | string;

a = null;

a = 'Hi!';
```

```typescript
// form.js
const inputEl = document.getElementById('user-name'); // null 또는 HTMLElement를 반환

if(!inputEl){
    throw new Error('Element not found!');
}

console.log(inputEl.value);

```

```typescript
// form.js
const inputEl = document.getElementById('user-name')!;
// 잠재적으로 null을 생성할 수 있는 메서드에 !를 붙이면 null을 반환하지 않고 0을 반환하도록
// 타입스크립트를 설득하는 것임.
// 이럴 경우, 타입스크립트는 무시하고 작업하는 것이지만 런타임은 여전히 null로 인식해
// 아래 코드는 에러 발생할 수 있음

console.log(inputEl.value);
// console.log(inputEl?.value)도 가능

console.log(inputEl?.value);
// Optional Chaining. 타입스크립트에만 쓰였지만 지금은 자바스크립트에서도 가능
// 변수가 잠재적 null임을 알려주고, null이 아닌경우에만 다음 단계를 진행해야함을 알려줌
```

### Type Casting

Type Assertion(타입 단언) 이라고도 함.

어떤 유형을 다른 유형으로 변환하는 것.

HTMLElement는 일반적인 html 유형으로 정확히 어떤 html element인지는 불명확함
value 값을 가질 수 없는 element도 있기 때문에 inputEl.value에 에러 발생

```typescript
// form.js
const inputEl = document.getElementById('user-name') as HTMLInputElement;
// ! keyword와 마찬가지로 타입스크립트는 무시하지만 런타임 에러가 발생할 수 있음을 주의

if(!inputEl){
    throw new Error('Element not found!');
}

console.log(inputEl?.value);
```

### Special 타입 2 : unknown type

api 호출과 같이 어떤 유형의 값을 받아올 지 알 수 없는 경우, 유니언 타입을 쓸 수도 있지만 unknown 을 쓰는 게 편리

any 와 비슷하지만 다름. any는 basically vanilla javascript이기 때문에, 해당 변수에 접근하려고 할 때, 아무런 에러도 반환하지 않음. unknown은 해당 변수가 정확히 무슨 타입인지 알 수 없기 때문에 에러를 발생시킴. 개발자가 추가로 조건을 줘야함.

```typescript
function process(val: unknown ) {
    if(typeof val === 'object' 
	    && !!val
	    &&  'log' in val 
	    && typeof val.log === 'function') {
        val.log()
    }
}
```

### optional

```typescript
// 매개변수를 선택적으로 넣고 싶을 경우,
function generateError(msg?: string) {
    throw new Error(msg);
}

generateError();
```

```typescript
// object에도 적용 가능함

type User = {
    name: string,
    age: number;
    role?: 'admin' | 'guest'
};
```

### Nullish Coalescing

- `??`
- 타입스크립트 뿐만 아니라 표준 자바스크립트에서도 사용되는 문법

```typescript
let input = '';
const didProvideInput = input ?? false;
// null 또는 undefined만 감지함. 그냥 빈 값은 그 값을 반환
```
