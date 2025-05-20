# Interfaces

<details>
<summary style="cursor: pointer; font-weight: bold; font-size: 1.2em;">📑 목차</summary>

- [인터페이스란?(What Are Interfaces?)](#what-are-interfaces)
- [인터페이스의 선언](#인터페이스의-선언)
- [인터페이스의 사용](#인터페이스의-사용)
- [왜 인터페이스를 사용하는가?(Why Interface?)](#why-interface)
- [함수 인터페이스](#함수-인터페이스)
- [클래스와 인터페이스](#클래스와-인터페이스)
- [인터페이스를 사용한 타입 보장](#인터페이스를-사용한-타입-보장)
- [인터페이스 사이의 상속](#인터페이스-사이의-상속)
- [인터페이스의 컴파일](#인터페이스의-컴파일)
- [추상 클래스와의 차이점](#추상-클래스와의-차이점)
</details>

## What Are Interfaces?

- 타입스크립트 전용 기능. 자바스크립트에는 인터페이스가 존재하지 않는다.
- 객체의 구조와 클래스 구현을 정의하는 일종의 계약서 역할
- 개발자가 특정 객체가 어떤 속성과 메서드를 가져야 하는지 명확히 정의할 수 있게 해줌

### 인터페이스의 선언

- 기본적인 생김새는 클래스와 비슷해보이지만, 실제 구현 없이 타입(구조)만 정의
- 함수 타입 정의 시 중괄호 없이 매개변수의 타입이나 반환 타입만 명시

```tsx
interface Authenticatable {
		email: string;
    password: string;

    login(): void;
    logout(): void;
}
```

### 인터페이스의 사용

객체가 특정 형태를 따르도록 강제할 수 있음

```tsx
interface Authenticatable {
    email: string;
    password: string;

    login(): void;
    logout(): void;
}

let user: Authenticatable;

user = {
    email: 'test@example.com',
    password: 'abc1',
    login() {
        //reach out to a database, check credentials, create a session
    },
    logout() {
        // clear the session
    },
};
```

### Why Interface?

`type` 키워드를 사용할 수 있는데도, 인터페이스를 사용하는 이유는?

대부분의 시나리오에서는 단순 개인의 취향에 따라 갈리지만, 특정 시나리오에서는 유용하게 사용될 수 있다.

- `declaration merge(선언 병합)` : 동일한 이름의 인터페이스를 여러 번 정의하여, 속성이나 메서드를 쉽게 추가할 수 있다. 다른 파일이나 라이브러리 등에서 가져온 인터페이스를 확장할 때 유용하다.

```tsx
interface Authenticatable {
    email: string;
    password: string;

    login(): void;
    logout(): void;
}

interface Authenticatable {
    role: string;
}

let user: Authenticatable;

user = {
    email: 'test@example.com',
    password: 'abc1',
    role: 'admin', // 병합된 속성
    login() {
        //reach out to a database, check credentials, create a session
    },
    logout() {
        // clear the session
    },
};
```

### 함수 인터페이스

함수의 타입을 정의하는 데도 사용될 수 있다. 드물게 사용되지만, 알아두면 유용하다.

```tsx
// type을 사용한 함수 타입 정의
type SumFn = (a: number, b: number) => number;

let sum: SumFn;

sum = (a, b) => a + b;
```

```tsx
// interface를 사용한 함수 타입 정의
interface SumFn {
	(a: number, b: number): number;
}

let sum: SumFn;

sum = (a, b) => a + b;
```

### 클래스와 인터페이스

- 클래스가 특정한 형태를 가지고 있도록 강제하고 싶을 때 사용할 수 있다.
- `implements` 키워드 사용
- 여러 개의 인터페이스를 동시에 구현 가능
- 구현 클래스는 인터페이스에 정의된 속성 외에도 추가 속성과 메서드를 가질 수 있다.

```tsx
interface Authenticatable {
    email: string;
    password: string;

    login(): void;
    logout(): void;
}

class AuthenticatableUser implements Authenticatable{
    constructor(public userName: string,
                public email: string,
                public password: string) {}

    login() {
        // ...
    }

    logout() {
        // ...
    }
}
```

### 인터페이스를 사용한 타입 보장

함수의 매개변수에 인터페이스를 사용하면 특정 형태의 객체만 받도록 제한할 수 있다.

```tsx
// 인터페이스 없이 구현
// function authenticate(user: {login(): void}) {}

interface Authenticatable {
    email: string;
    password: string;

    login(): void;
    logout(): void;
}

// 함수의 매개변수 타입으로 인터페이스 사용
function authenticate(user: Authenticatable) {
    user.login();
}

// 이 함수는 Authenticatable 인터페이스를 만족하는 어떤 객체든 받을 수 있음
const userObj = {
    email: "test@example.com",
    password: "password123",
    login() { console.log("Logging in..."); },
    logout() { console.log("Logging out..."); },
    additionalMethod() {} // 추가 속성/메서드가 있어도 괜찮음
};

authenticate(userObj); // 정상 작동
```

### 인터페이스 사이의 상속

다른 인터페이스를 상속하여 확장할 수 있다.

선언 병합과의 차이점 : 기존의 인터페이스를 건드리지 않고, 별개의 인터페이스를 생성

`extends` 키워드 사용

```tsx
interface Authenticatable {
    email: string;
    password: string;

    login(): void;
    logout(): void;
}

interface AuthenticatableAdmin extends Authenticatable {
    role: 'admin' | 'superadmin';
}
```

### 인터페이스의 컴파일

인터페이스는 타입스크립트의 타입 체킹에만 사용되고, 컴파일된 자바스크립트에는 포함되지 않는다. 컴파일 후에는 모든 인터페이스 정의가 제거된다.

### 추상 클래스와의 차이점

1. 구현 포함 여부
- 추상 클래스 : 추상 메서드뿐만 아니라 구현된 메서드도 포함할 수 있다.
- 인터페이스 : 구현 없이 메서드와 속성의 선언만 포함
1. 상속 방식
- 추상 클래스 : `extends` 키워드로 단일 상속만 가능
- 인터페이스 : `implements` 키워드로 여러 인터페이스 구현 가능, `extends` 키워드로 여러 인터페이스 상속 가능
1. 런타임 존재
- 추상 클래스 : 컴파일된 JS에 클래스로 존재
- 인터페이스 : 컴파일 후 완전히 제거됨 (타입 체크에만 사용)
