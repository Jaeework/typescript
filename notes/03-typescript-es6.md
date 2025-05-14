# Next-generation JavaScript & TypeScript

<details>
<summary style="cursor: pointer; font-weight: bold; font-size: 1.2em;">📑 목차</summary>

- [1. `let`과 `const`](#1-let과-const)
    - [스코프 차이점](#스코프-차이점)
    - [호이스팅(Hoisting)](#호이스팅hoisting)
- [2. 화살표 함수(Arrow function)](#2-화살표-함수arrow-function)
    - [장점](#장점)
    - [화살표 함수의 변형](#화살표-함수의-변형)
- [3. 함수 매개변수의 기본 값(Default Function Parameters)](#3-함수-매개변수의-기본-값default-function-parameters)
- [4. 스프레드 연산자(Spread Operator)](#4-스프레드-연산자spread-operator)
    - [배열](#배열)
    - [객체](#객체)
- [5. 레스트 매개변수(Rest parameters)](#5-레스트-매개변수rest-parameters)
- [6. 배열 및 객체 구조 분해(Destructuring)](#6-배열-및-객체-구조-분해destructuring)
    - [배열](#배열-1)
    - [객체](#객체-1)

</details>

## 1. `let` and `const`

```typescript
const userName = 'Max';
let age = 30;

age = 29;

function add(a: number, b:number) {
    let result;
    result = a + b;
    return result;
}

if(age > 20) {
    let isOld = true;
    // var isOld = true; // var로 선언하면 블록 밖에서도 접근 가능
}

// console.log(isOld);   // 오류: isOld는 if 블록 내에서만 접근 가능
// console.log(result);  // 오류: result는 함수 내에서만 접근 가능
```

### 스코프 차이점

- `var` : 함수 스코프 - 선언된 함수 내에서만 접근 가능. 함수 외부나 if/for 등 블록 내부에서 선언되면 전역 스코프
- `let` / `const` : 블록 스코프 - 선언된 블록(`{}`)과 그 하위 레벨에서만 접근 가능

### 호이스팅(Hoisting)

- `var` : 선언 위치와 상관없이 해당 스코프의 최상단으로 끌어올려짐(호이스팅). 선언 전에 접근해도 에러가 나지 않고 `undefined` 반환
- `let` / `const` 도 호이스팅되지만, 선언 전에 접근할 수 없는 `일시적 사각지대(Temporal Dead Zone)`가 존재. 선언 전에 접근하면 `ReferenceError` 발생

## 2. 화살표 함수(Arrow function)

### 장점

1. 일반 함수 선언식보다 짧게 표현 가능
2. 한 가지 식만 있다면, 중괄호 와 return 키워드 생략 가능

```typescript
// 기존 함수 표현식
const add1 = function(a: number, b: number) {
	return a + b;
}

// 화살표 함수 - 간결한 표현
const add2 = (a: number, b: number) => a + b;
```

### 화살표 함수의 변형

1. 타입스크립트에서의 매개변수 표현

```typescript
const add = (a: number, b: number) => a + b;

//const printOutput = (output:string | number) => {
//    console.log(output);
//}

// 함수 타입 정의와 구현을 분리하여, javascript에서처럼 매개변수가 한 개일 때, 괄호 생략 가능
const printOutput: (a: number | string) => void = output => console.log(output);

printOutput(add(5, 2));
```

2. 이벤트 처리와 같은 익명 함수로 사용 시 간결함

```typescript
const button = document.querySelector('button');

if(button) {
	button.addEventListener('click', event => console.log(event));
}
```

## 3. 함수 매개변수의 기본 값(Default Function Parameters)

함수 매개변수에 기본 값을 설정하여, 인자가 전달되지 않았을 때 사용할 값을 지정할 수 있다.

```typescript
const add = (a: number, b: number = 1) => a + b;

console.log(add(5)); // 출력 : 6 (b에 기본 값 1 사용)
console.log(add(5, 3)); // 출력 : 8 (b에 3 전달)
```

**주의 사항 :** 기본 값이 설정된 매개변수는 오른쪽에 위치해야 한다. why? 매개변수를 건너뛸 방법이 없기 때문

```typescript
const add = (a: number = 1, b: number) => a + b;

console.log(add(5)); // 오류 : b에 값이 전달되지 않음
// 5는 a에 할당되고, b는 undefined
```

## 4. 스프레드 연산자(Spread Operator(`…`))

스프레드 연산자(`…`)는 배열이나 객체의 각 요소를 하나씩 분리해서(펼쳐서) 개별 인자로 전달하거나 전개

### 배열

```typescript
const hobbies = ['Sports', 'Cooking'];
const activeHobbies = ['Hiking'];

// 기존 방식
// push 메서드는 배열 전체를 하나의 요소로 추가할 뿐, 각 요소를 따로 추가하지 않는다
activeHobbies.push(hobbies); // ['Hiking', ['Sports', 'Cooking']]

// push 메서드에 여러 값을 나열하면 각각의 요소로 추가 가능
activeHobbies.push(hobbies[0], hobbies[1]);

// 스프레드 연산자를 사용하면 배열의 각 요소를 개별 인자로 한 번에 전달 가능
activeHobbies.push(...hobbies);

// 새 배열 생성 시에도 사용 가능
const allHobbies = [...activeHobbies, ...hobbies];
```

### 객체

```typescript
const person = {
    name: 'Max',
    age: 30
}

// 단순히 person 객체의 포인터만 복사
// const copiedPerson = person;

// 새 객체를 생성하기 위해서 spread 연산자 사용
const copiedPerson = {...person};

// 추가 속성도 포함 가능
const extendedPerson = {...person, hobbies: ['Sports']};
```

## 5. 레스트 매개변수(Rest parameters)

스프레드 연산자와 유사하지만 반대 방향으로 작동한다. 여러 개의 인자를 하나의 배열로 모아준다.

매개변수를 flexible 하게 받고 싶을 때 사용.

```typescript
const add = (...numbers: number[]) => {
	return numbers.reduce((curResult, curValue) => {
		return curResult + curValue;
	}, 0);
}

const addedNumbers = add(5, 10, 2, 3.7);
console.log(addedNumbers); // 출력 : 20.7
```

매개변수를 몇 개 받을 것인지 정확히 알고 있다면 튜플 타입과도 결합 가능하다.

```typescript
const add = (...numbers: [number, number, number]) => {
	return numbers.reduce((curResult, curValue) => {
		return curResult + curValue;
	}, 0);
}

const addedNumbers = add(5, 10, 2);
console.log(addedNumbers);
```

## 6. 배열 및 객체 구조 분해(Destructuring)

배열이나 객체의 값을 추출하기

### 배열

```typescript
const hobbies = ['Sports', 'Cooking', 'Reading'];

// 기존 방식
// const hobby1 = hobbies[0];
// const hobby2 = hobbies[1];

// 구조 분해 사용
const [hobby1, hobby2, ...remainingHobbies] = hobbies;

console.log(hobby1, hobby2, remainingHobbies); // 'Sports', 'Cooking', ['Reading']
console.log(hobbies); // 원본 배열은 변경되지 않음: ['Sports', 'Cooking', 'Reading']
```

### 객체

```typescript
const person = {
    firstName: 'Max',
    age: 30
}

//const { firstName, age } = person;
// 객체는 배열과 달리 순서가 아닌 키(key)로 매핑.
// 키 이름은 원본 객체의 키 이름과 동일해야 함.

// 다른 이름으로 복사하고 싶을 경우
const { firstName: userName, age} = person;

console.log(userName, age, person);
```
