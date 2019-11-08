## Class 

함수가 특정기능을 하는 구문이라면, 클래스는 이렇게 만들어 질 수 있는 변수와 함수 중 연관있는 변수와 함수만을 선별하여 포장하는 기술입니다.

**클래스는 객체라고도 하며,** 속성과 메소드로 구성됩니다. 속성은 객체의 전용 변수이고 메소드는 객체의 전용 함수입니다. 멤버변수, 멤버함수라고도 합니다.

이렇게 클래스로 포장하는 이유는 객체 단위로 코드를 그룹화 할 수 있으며 코드를 재사용하기 위함 입니다.

```javascript
// class 선언문
// 이전에는 생성자 함수와 프로토타입, 클로저를 사용하여 객체 지향 프로그래밍을 구현했지만, ES6에 class개념이 등장하면서 현재는 class 키워드를 사용하여 정의를합니다.

let PersonClass = class {
    constructor(name) {
        this.name = name;
    }
    sayName() {
        console.log(this.name);
    }
};

let person = new PersonClass("Nicholas");
person.sayName();   // "Nicholas" 출력
```



클래스 몸체(class body)에는 메소드만 선언할 수 있습니다. 클래스 바디에 클래스 필드(멤버 변수)를 선언하면 문법 에러(SyntaxError)가 발생합니다. 따라서 멤버 변수의 선언과 초기화는 반드시 constructor 내부에서 해야 합니다.

```javascript
class Foo {
  name = ''; // SyntaxError
  constructor() {}
}

class Foo {
  constructor(name = '') {
    this.name = name; // 클래스 필드의 선언과 초기화
  }
}

const foo = new Foo('Lee');
console.log(foo); // Foo { name: 'Lee' }
```



클래스는 클래스 선언문 이전에 참조할 수 없습니다.

```javascript
console.log(Foo);
// ReferenceError: Cannot access 'Foo' before initialization

class Foo {}

// 호이스팅(hoisting)이 일어나지 않습니다.
// 호이스팅이란? .. 동훈오빠 정리~
```



###인스턴스(instance)

일반적인 함수의 경우는 함수를 선언한 후 함수를 호출해줘야 동작하듯이 클래스의 경우는 클래스 인스턴스를 생성해야만 클래스에 들어 있는 함수들의 내부 기능들을 사용할 수 있습니다.

```javascript
var 인스턴스 = new 클래스이름();        // new 키워드를 사용

// 인스턴스 생성
const me = new Person('Lee');
me.sayHi(); // Hi! Lee
```



### 생성자(Constructor)

`constructor`는 인스턴스를 생성하고 클래스 필드를 초기화하기 위한 특수한 메소드입니다. 클래스에서 constructor 이름을 갖는 메소드는 하나여야 합니다.

constructor는 생략이 가능합니다. constructor를 생략하면 클래스에 `constructor() {}`를 포함한 것과 동일하게 동작합니다. 즉, 빈 객체를 생성합니다. 따라서 인스턴스에 프로퍼티를 추가하려면 인스턴스를 생성한 이후, 프로퍼티를 동적으로 추가해야 합니다.

```javascript
var Polygon = class {
    constructor(height, width) {
        this.height = height;    
        this.width = width;    
    }    
    constructor(height2, width2) {
        this.height = height2 * 2 ;
        this.width = width2 * 2;    
    }
}; // Uncaught SyntaxError: A class may only have one constructor
```



### setter/getter


getter는 이름 그대로 무언가를 취득할 때 사용하므로 반드시 무언가를 반환해야 합니다. setter는 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용하며 할당 시에 메소드가 호출됩니다. 

```javascript
class Foo {
  constructor(arr = []) {
    this._arr = arr;
  }

  // getter: get 키워드 뒤에 오는 메소드 이름 firstElem은 클래스 필드 이름처럼 사용된다.
  get firstElem() {
    // getter는 반드시 무언가를 반환하여야 한다.
    return this._arr.length ? this._arr[0] : null;
  }

  // setter: set 키워드 뒤에 오는 메소드 이름 firstElem은 클래스 필드 이름처럼 사용된다.
  set firstElem(elem) {
    // ...this._arr은 this._arr를 개별 요소로 분리한다
    this._arr = [elem, ...this._arr];
  }
}

const foo = new Foo([1, 2]);

// 클래스 필드 lastElem에 값을 할당하면 setter가 호출된다.
foo.firstElem = 100;

console.log(foo.firstElem); // 100
```



### 익명 클래스/ 이름이 부여된 클래스

```javascript
//익명클래스
let PersonClass = class {
    // PersonType constructor와 같습니다.
    constructor(name) {
        this.name = name;
    }
    // PersonType.prototype.sayName과 같습니다.
    sayName() {
        console.log(this.name);
    }
};
let person = new PersonClass("Nicholas");
person.sayName();   // "Nicholas" 출력
```

```javascript
//이름이 부여된 클래스
let PersonClass = class PersonClass2 {
  ...
    //코드 동일
};
```



### 정적 메소드/ 프로토타입 메소드

```js
class Person {
  constructor({name, age}) {
    this.name = name;
    this.age = age;
  }
  
  // 정적 메소드.
  static sumAge(x, y) {
    return x + y;
  }
  
  // 프로토타입 메소드
  subtract(x, y) {
    return x - y;
  }
}

const person1 = new Person({name: '윤아준', age: 19});
const person2 = new Person({name: '신하경', age: 20});

Person.sumAge(person1.age, person2.age); // 39
person1.sumAge(person1.age, person2.age); // error

person1.subtract(person1.age, person2.age); // -1
```

`static` 키워드를 메소드 이름 앞에 붙여주면 해당 메소드는 **정적 메소드**가 됩니다.

> static 메소드는 클래스의 인스턴스 (var a = new testFunc()) 필요없이 호출 가능합니다. 또한 클래스의 인스턴스에서 static 메소드를 호출 할 수 없습니다.

*ES6 등장 이전의 statkc/prototype 사용법 :  https://jaeworld.github.io/2018-09-03/Javascript_static



### 클래스 필드와 this

`class` 블록은 새로운 블록 스코프를 형성하고, 이 내부에서 사용된 `this`는 인스턴스 객체를 가리키게 됩니다.

```js
class MyClass {
  a = 1;
  b = this.a;
}

new MyClass().b; // 1
```



### 클래스 상속

클래스 상속(Class Inheritance)은 코드 재사용 관점에서 매우 유용합니다. 새롭게 정의할 클래스가 기존에 있는 클래스와 매우 유사하다면, 상속을 통해 그대로 사용하되 다른 점만 구현하면 됩니다. 코드 재사용은 개발 비용을 현저히 줄일 수 있는 잠재력이 있으므로 매우 중요합니다.

1. extends 키워드

```js
class Parent {
  static staticProp = 'staticProp';
  static staticMethod() {
    return 'I\'m a static method.';
  }
  instanceProp = 'instanceProp';
  instanceMethod() {
    return 'I\'m a instance method.';
  }
}

class Child extends Parent {}

console.log(Child.staticProp); // staticProp
console.log(Child.staticMethod()); // I'm a static method.

const c = new Child();
console.log(c.instanceProp); // instanceProp
console.log(c.instanceMethod()); // I'm a instance method.
```

- 자식 클래스 A를 통해 부모 클래스 B의 **정적 메소드와 정적 속성**을 사용할 수 있습니다.
- 부모 클래스 B의 **인스턴스 메소드와 인스턴스 속성**을 자식 클래스 A의 인스턴스에서 사용할 수 있습니다.

2. super 키워드

```js
class Person {
  constructor({name, age}) {
    this.name = name;
    this.age = age;
  }
  introduce() {
    return `제 이름은 ${this.name}입니다.`
  }
}

class Student extends Person {
  constructor({grade, ...rest}) {
    // 부모 클래스의 생성자를 호출할 수 있습니다.
    super(rest);
    this.grade = grade;
  }
  introduce() {
    // 부모 클래스의 `introduce` 메소드를 호출할 수 있습니다.
    return super.introduce() + ` 저는 ${this.grade}학년입니다.`;
  }
}

const s = new Student({grade: 3, name: '윤아준', age: 19});
s.introduce(); // 제 이름은 윤아준입니다. 저는 3학년입니다.
```

- super 키워드는 부모 클래스를 참조(Reference)할 때 또는 부모 클래스의 constructor를 호출할 때 사용합니다.



** 참고 ㅣ https://helloworldjavascript.net/pages/270-class.html