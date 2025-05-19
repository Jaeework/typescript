# Classes

<details>
<summary style="cursor: pointer; font-weight: bold; font-size: 1.2em;">📑 목차</summary>

- [클래스란?(What Are Classes?)](#what-are-classes)
  - [필드(프로퍼티)](#필드프로퍼티)
  - [접근 제한자 `public` and `private`](#접근-제한자-public-and-private)
  - [`readonly`](#readonly)
- [고급 클래스 기능(Advanced Classes)](#advanced-classes)
    - [getter](#getter)
    - [setter](#setter)
    - [Static Properties & Methods](#static-properites--methods)
    - [상속](#상속)
    - [접근제한자 `protected`](#접근제한자-protected)
    - [추상 클래스(abstract class)](#abstract-class)
</details>

## What Are Classes?

ES6(ECMAScript 2015)부터 자바스크립트에 도입된 문법. 객체에 대한 청사진(blueprint) 역할을 한다. 타입스크립트에서는 클래스에 타입 시스템과 다양한 객체지향 프로그래밍(OOP) 기능이 추가되어, 더 안전하고 강력한 객체 설계가 가능.

참고 : Interface는 타입스크립트에서만 제공하는 타입 선언 문법

### 필드(프로퍼티)

해당 클래스를 기반으로 생성되는 모든 객체가 공통적으로 가지게 되는 속성

자바스크립트에서는 `constructor`를 통해 필드 초기화 수행

```jsx
class User {
	constructor() {
		this.name = 'Max';
	}
}
```

타입스크립트에서는 오류 발생.

타입스크립트는 클래스 본문에서 미리 필드를 선언

```tsx
class User {
	name = 'Max';
	age: number;
	
	constructor(n: string, a: number) {
		this.name = n;
		this.age = a;
	}
}
```

타입스크립트에서는 생성자의 매개변수 앞에 `접근 제한자`를 붙여주면, 자동으로 같은 이름을 가진 프로퍼티를 생성해줄 뿐만 아니라, 값을 받아 초기화 해준다. 따라서, 다음과 같이 생략 가능하다.

```tsx
class User {
	constructor(public name: string, public age: number) {}
}
```

### 접근 제한자 `public` and `private`

```tsx
class User {
    public hobbies: string[] = [];

    constructor(public name: string, private age: number = 39) {
        this.age = age;
        this.name = name;
    }
}
```

`public` : 클래스 외부에서도 접근 가능

`private` : 클래스 내부에서만 접근 가능

※참고

접근제한자는 타입스크립트에서 제공하는 기능. 자바스크립트는 기본적으로 `public` . private하게 하고 싶을 땐 변수명 앞에 `#` 를 붙여준다. ex) `#role = 'admin'`

### `readonly`

접근제한자와 함께 사용하여 읽기 전용으로 사용할 수 있게 하는 키워드.

`readonly` 키워드는 변수가 참조하는 메모리 주소의 재할당을 금지할 뿐, push() 메서드를 통해 기존 객체나 배열의 내부 값은 변경될 수 있다.

```tsx
class User {
    readonly hobbies: string[] = [];

    constructor(public name: string, protected readonly age: number = 39) {
        this.age = age;
        this.name = name;
    }

    greet() {
        console.log('My age: ' + this.age);
    }
}

const max = new User('Max', 36);
const fred = new User('Fred', 29);

//max.hobbies = ['Sports']; // 에러 발생할 것임
max.hobbies.push('Sports');

console.log(max, fred);
```

## Advanced Classes

### getter

```tsx
class User {
	constructor(private firstName: string, private lastName: string) {}
	
	get fullName() {
		return this.firstName + ' ' + this.lastName;
	}
}

const max = new User('Max', 'Schwarzmuller');
console.log(max.fullName);
```

getter는 ‘계산된(computed) 프로퍼티’

일반 프로퍼티는 값이 저장되어 있지만, 게터는 호출될 때마다 현재 객체 상태를 반영한 최신 값을 제공한다. 값의 동적 특성. 참조될 때마다 실행되므로 항상 최신 상태를 반영

### setter

프로퍼티에 조건을 만족하는 값만 넣고 싶을 때 유용

```tsx
class User {
    private _firstName: string = '';
    private _lastName: string = '';

    set firstName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._firstName = name;
    }
    set lastName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._lastName = name;
    }

    get fullName() {
        return this._firstName + ' ' + this._lastName;
    }
}

const max = new User();
max.firstName = 'Max';
max.lastName = '';
console.log(max.fullName);
```

### Static Properites & Methods

클래스 자체에 귀속되는 속성/메서드로, 인스턴스 생성 없이 클래스명으로 바로 접근이 가능

```tsx
class User {
    private _firstName: string = '';
    private _lastName: string = '';

    set firstName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._firstName = name;
    }
    set lastName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._lastName = name;
    }

    get fullName() {
        return this._firstName + ' ' + this._lastName;
    }

    static eid = 'USER';

    static greet() {
        console.log('Hello!');
    }
}

// 다른 변수나 함수와는 다르게 User 객체 생성하지 않고도 바로 접근 가능
console.log(User.eid);
User.greet();

const max = new User();
max.firstName = 'Max';
max.lastName = '';
console.log(max.fullName);
```

### 상속

자바스크립트에서도 지원하는 기능. 부모 클래스의 `public`, `protected` 멤버에 접근할 수 있다.

```tsx
class User {
    private _firstName: string = '';
    private _lastName: string = '';

    set firstName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._firstName = name;
    }
    set lastName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._lastName = name;
    }

    get fullName() {
        return this._firstName + ' ' + this._lastName;
    }

    static eid = 'USER';

    static greet() {
        console.log('Hello!');
    }
}

console.log(User.eid);
User.greet();

const max = new User();
max.firstName = 'Max';
max.lastName = '';
console.log(max.fullName);

class Employee extends User {
	constructor(public jobTitle: string) {
        super();
        // super.firstName = 'Max';
    }
}
```

### 접근제한자 `protected`

- `private` + 상속 받은 클래스에서 접근 가능
- 클래스 내부와 해당 클래스를 상속받은 자식 클래스에서만 접근 가능
- 보호된 상태에서 특정 개체에 접근해야 하는 경우 사용

```tsx
class User {
    protected _firstName: string = '';
    private _lastName: string = '';

    set firstName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._firstName = name;
    }
    set lastName(name: string) {
        if (name.trim() === '') {
            throw new Error('Invalid name.');
        }
        this._lastName = name;
    }

    get fullName() {
        return this._firstName + ' ' + this._lastName;
    }
}

const max = new User();
// max._firstName = 'Max 2'; // 클래스 외부이기 때문에 오류

class Employee extends User {
    constructor(public jobTitle: string) {
        super();
        // super.firstName = 'Max';
    }

    work() {
        // ...
        console.log(this._firstName);
        // super._firstName = ''
    }
}
```

### abstract class

- 자바스크립트에는 없는 타입스크립트 전용 기능
- 직접 인스턴스화 할 수 없고, 반드시 상속받은 파생 클래스에서 구현해야 하는 클래스
- `abstract` 키워드로 선언한 추상 메서드는 파생 클래스에서 반드시 구현해야 한다.

```tsx
abstract class UIElement {
	constructor(public identifier: string) {}
	
	clone(targetLocation: string) {
		// logic to duplicate the UI element
	}
}

// `new` 키워드로 직접 인스턴스화할 수 없다.
// let uiElement = new UIElement();

class SideDrawerElement extends UIElement {
	constructor(public identifier: string, public position: 'left' | 'right') {
		super(identifier);
	}
}
```
