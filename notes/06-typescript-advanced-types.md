# Advanced Types

<details>
<summary style="cursor: pointer; font-weight: bold; font-size: 1.2em;">📑 목차</summary>

- [교차 타입(Intersection Types)](#intersection-types-교차타입)
- [타입 가드(Type Guards)](#type-guards)
- [함수 오버로드(Function Overloads)](#function-overloads)
- [인덱스 타입(Index Types)](#index-types)
- [상수 타입 어서션(Constant Type)](#constant-type-as-const)
- [Record 타입](#record-타입)
- [satisfies 연산자](#satisfies)
</details>

## Intersection Types 교차타입

`type` 키워드를 이용해 만든 두 개 이상의 커스텀 타입을 합친 새로운 타입을 만들고 싶을 때,  `&` 연산자를 이용해 손쉽게 합칠 수 있다. 교차 타입은 모든 타입의 속성을 포함한다.

```tsx
type FileData = {
    path: string;
    content: string;
};

type DatabaseData = {
		connectionUrl: string;
		credentials: string;
};

type Status = {
    isOpen: boolean;
    errorMessage?: string;
};

type AccessedFileData = FileData & Status;
type AccessedDatabaseData = DatabaseData & Status;
```

interface를 사용하는 대안

```tsx
interface FileData {
    path: string;
    content: string;
}

interface DatabaseData {
    connectionUrl: string;
    credentials: string;
}

interface Status {
    isOpen: boolean;
    errorMessage?: string;
}

interface AccessedFileData extends FileData , Status {}
interface AccessedDatabaseData extends DatabaseData, Status {}
```

## Type Guards

유니언 타입으로 전혀 호환되지 않는 두 타입을 다룰 때, 런타임에서 타입을 안전하게 확인하는 방법들

### 1. `in` 연산자를 사용하여 객체 안에 있는 특정 속성의 존재 유무 확인

```tsx
type FileSource = { path: string };
const fileSource: FileSource = {
    path: 'some/path/to/file.csv',
};

type DBSource = { connectionUrl: string };
const dbSource: DBSource = {
    connectionUrl: 'some-connection-url',
};

type Source = FileSource | DBSource;

function loadData(source: Source) {
    if ('path' in source) {
        // source.path => use that to open the file
        return;
    }
    // source.connectionUrl; => to reach out to database
}
```

### 2. Discriminated Unions(판별된 유니언)

속성 이름이 언제든 변경될 수 있기 때문에, 두 객체가 모두 공유하는 하나의 판별 속성을 만든다. 더 안전하고 명확한 방법

```tsx
type FileSource = { type: 'file'; path: string };
const fileSource: FileSource = {
    type: 'file',
    path: 'some/path/to/file.csv',
};

type DBSource = { type: 'db', connectionUrl: string };
const dbSource: DBSource = {
    type: 'db',
    connectionUrl: 'some-connection-url',
};

type Source = FileSource | DBSource;

function loadData(source: Source) {
    if (source.type === 'file') {
        // source.path => use that to open the file
        return;
    }
    // source.connectionUrl; => to reach out to database
}
```

### 3. `instanceof` 연산자

클래스로 된 객체를 다룰 때 사용. 자바스크립트에도 있는 기본 연산자이다.

```tsx
class User {
    constructor(public name: string) {}

    join() {
        // ...
    }
}

class Admin {
    constructor(permissions: string[]) {}

    scan() {
        // ...
    }
}

const user = new User('Max');
const admin = new Admin(['ban', 'restore']);

type Entity = User | Admin;

function init(entity: Entity) {
    if (entity instanceof User) {
        entity.join();
        return;
    }

    entity.scan();
}
```

### 4. 사용자 정의 타입 가드 (“Outsourcing” Type Guards & Type Predicates)

타입 가드 로직을 별도 함수로 분리하여 재사용성을 높일 수 있다. 함수의 반환 타입에 `is` 키워드를 사용한다. 내부적으로는 부울이지만, 말하자면 추가 정보가 첨부된 부울이다.

```tsx
type FileSource = { type: 'file'; path: string };
const fileSource: FileSource = {
    type: 'file',
    path: 'some/path/to/file.csv',
};

type DBSource = { type: 'db', connectionUrl: string };
const dbSource: DBSource = {
    type: 'db',
    connectionUrl: 'some-connection-url',
};

type Source = FileSource | DBSource;

function isFile(source: Source): source is FileSource {
    return source.type === 'file';
}

function loadData(source: Source) {
    // if ('path' in source) {
    if (isFile(source)) {
        // source.path
        // source.path; => use that to open the file
        return;
    }
    // source.connectionUrl; => to reach out to database
}
```

## Function Overloads

함수가 서로 다른 타입의 매개변수를 받아 각각 다른 타입을 반환할 때, 더 정확한 타입 추론을 위해 사용

```tsx
function getLength(val: string | any[]) {
    if (typeof val === 'string') {
        const numberOfWords = val.split(' ').length;
        return `${numberOfWords} words`;
    }
    return val.length;
}

const numOfWords = getLength('does this work?');
// numOfWords.length; => 반환 타입이 string인지 any[]인지 알 수 없어 에러남
const numItems = getLength(['Sports', 'Cookies']);
```

function overloads 활용

```tsx
function getLength(val: any[]): number;
function getLength(val: string): string;
function getLength(val: string | any[]) {
    if (typeof val === 'string') {
        const numberOfWords = val.split(' ').length;
        return `${numberOfWords} words`;
    }
    return val.length;
}

const numOfWords = getLength('does this work?');
numOfWords.length;
const numItems = getLength(['Sports', 'Cookies']);
```

## Index Types

javascript 기능. 동적으로 속성을 추가할 수 있는 유연한 객체 타입을 정의할 때 사용

```tsx
type DataStore = {
    [prop: string]: number | boolean;
};

let store: DataStore = {};

// ...

// 동적으로 속성 추가 가능
store.id = 5;
store.isOpen = false;
// store.name = 'Max'; // 잘못된 타입은 오류 발생

// 키가 숫자인 경우도 정의 가능
type NumberIndexed = {
    [index: number]: string;
};

let arr: NumberIndexed = {};
arr[0] = 'first';
arr[1] = 'second';
```

## Constant type `as const`

타입스크립트 전용 기능. 값을 리터럴 타입으로 고정하고 읽기 전용으로 추론한다. 특히 배열이나 객체를 불변으로 만들고 정확한 리터럴 타입을 유지하고 싶을 때 유용함

```tsx
let roles = ['admin', 'guest', 'editor'] as const;
// let roles:readonly ["admin", "guest","editor"]

// roles.push('max') // 읽기 전용이므로 오류 발생

const firstRole = roles[0];
```

## Record 타입

특정 키 타입과 값 타입을 가진 객체 타입을 간단하게 정의할 때 사용. 인덱스 타입의 간결한 대안이다.

```tsx
type DataStore = {
    [prop: string]: number | boolean;
};

let someObj: Record<string, number | boolean>; // string 키로 number나 boolean 값을 가짐
```

## `satisfies`

타입 안전성과 유연성을 동시에 제공하는 연산자

```tsx
// 문제 상황: Record 타입 사용시
const dataEntries: Record<string, number> = {
  entry1: 0.51,
  entry2: -1.23
};

// 타입스크립트는 string 속성을 가지고 있으니 맞는 것으로 추론한다.
// 실제 존재 유무는 확인 x
dataEntries.entry3 // number 타입으로 추론되지만 실제로는 undefined

const betterDataEntries = {
	entry1: 0.51,
	entry2: -1.23
} satisfies Record<string, number>;

// betterDataEntries.entry3 // 컴파일 오류
```

- 정확한 키만 접근 가능
    - 실제 정의된 속성에만 접근 가능
- 각 속성의 정확한 타입 유지
    - 유니언 타입이 아닌 실제 할당된 값의 구체적인 타입으로 추론
    - 타입 가드 필요 없이 해당 타입의 메서드나 속성 사용 가능
