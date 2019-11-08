## 객체 리터럴

```javascript
//객체 리터럴
const obj = {       
  A:1;    
  B:2;
};
```

: 객체 리터럴이란 기존 자바스크립트에서 사용하던 객체 정의 방식을 개선한 문법을 말합니다. 객체표현은 **중괄호**로 합니다. 중괄호로 표현하는 객체를 **객체 리터럴** 이라고 함



```javascript
var sayNode = function(){    
  console.log('Node');
};
var es = 'ES';  

//이전 문법
var oldObject = {    
  sayJS : function(){        
    console.log('JS');    },    
  sayNode: sayNode,
};
oldObject[es + 6] = 'Fanstastic';  

//변경 문법
const newObject = {    
  sayJS(){    //function 생략        
  console.log('JS');    },    
  sayNode,    
  [es + 6]: 'Fantastic'    //대괄호로
}
```



### 객체 요소 접근 방법

1) newObject.sayJS

2) newObject.["sayJS"] : 프로퍼티 이름을 대괄호 안에 문자열로 적습니다.



### ES6+ 문법 변화

**1) function() 생략**

**2)**

```javascript
//변경 이전
{data: data, 
value:value, 
length:length}

 
//변경 이후
{data, 
value,
length}
```



**3) 키로 객체 리터럴  접근**

객체 리터럴에 키로 접근하고자 할 때, 이전에는 객체 리터럴 외부에서 설정이 가능했지만,

객체리터럴 안에서도 >> [es + 6] : ‘Fantastic’ << 와 같이 대괄호 형태로 접근이 가능하게 되었습니다.



## Getter 함수와 Setter 함수

객체를 만들고 나면, 다음과 같이 객체안의 값을 수정 할 수도 있습니다.

```javascript
const numbers = {
  a: 1,
  b: 2
};

numbers.a = 5;
console.log(numbers);
```



Getter 함수와 Setter 함수를 사용하게 되면 특정 값을 바꾸려고 하거나, 특정 값을 조회하려고 할 때 우리가 원하는 코드를 실행 시킬 수 있습니다.

```javascript
const numbers = {
  _a: 1,
  _b: 2,
  sum: 3,
  calculate() {
    console.log('calculate');
    this.sum = this._a + this._b;
  },
  
  //getter
  get a() {
    return this._a;
  },
  get b() {
    return this._b;
  },
  
  //setter
  set a(value) {
    console.log('a가 바뀝니다.');
    this._a = value;
    this.calculate();
  },
  set b(value) {
    console.log('b가 바뀝니다.');
    this._b = value;
    this.calculate();
  }
};

console.log(numbers.sum);
numbers.a = 5;
numbers.b = 7;
numbers.a = 9;
//a(), b()가 아니라, a,b만으로도 접근 가능해짐

console.log(numbers.sum);
console.log(numbers.sum);
console.log(numbers.sum);
```



### Get/Set함수를 쓰는 이유

객체.setter함수를 쓰면, 변수이름을 모르게 만드는 것이 가능합니다.. 

(캡슐화, 정보은닉 관점에서 사용됨)



##비구조화 할당(destructuring)

: 비구조화 할당이란, 배열 또는 객체 안의 값을 편하게 꺼낼 수 있게 하는 문법을 말합니다.



### 객체에서 비구조화 할당

```javascript
//변경 전 문법 : 비구조화 할당을 받는 변수와 객체의 속성의 이름이 같습니다.

var candyMachine = {    
  status : {        
    name: 'node',        
    count: 5,    
  },    
  getCandy : function() {        
    this.status.count--;        
    return this.status.count;    
  }
}; 

var status = candyMachine.status
var getCandy  = candyMachine.getCandy
```



```javascript
//변경 후 문법 : candyMachine이라는 객체에서 알아서 status와 getCandy를 가지고 와서 저장합니다. => 속성들을 많이 꺼내오면 꺼내 올 수록 효율성이 향상됩니다.

var candyMachine = {    
  status : {        
    name: 'node',        
    count: 5,    
  },    
  getCandy(){        
    this.status.count--;        
    return this.status.count;
  }
}; 

const {status, getCandy} = candyMachine;
```

:const {status,getCandy,a,b} = candyMachine; 와 같이 없는 속성(a,b)을 꺼내 올 경우, 문법적으로 오류는 발생하지 않지만, 빈 것 그대로 저장되어서 undefined 상태가 됩니다.



### this 의 속성

```javascript
const candyMachine = {    
  status : {        
    name : 'node',        
    count : 5,    },    
  getCandy() {        
    this.status.count--;        
    return this.status.count;}
}; 

candyMachine.getCandy();
//4
//getCandy가 호출될 때, getCandy에 있는 this는 candyMachine이 됨!

getCandy()
//undefined
//getCandy()와 같은형태로 속성 앞에 점이 없는 채로 호출 될 때는 this를 찾지 못함.
```



###배열에서 비구조화 할당

```javascript
var array = ['nodejs', {}, 10, true]; 

//비구조화 할당 X
var node = array[0];
var obj = array[1];
var bool = array[arrat.length - 1];

//비구조화 할당
const [node, obj, , bool] = array;
```

단!! 배열과 객체는 **괄호표시에 유의 해야 함!**

| 객체 리터럴 | 배열    |
| ----------- | ------- |
| const {}    | const[] |



## rest문법, 객체참조

### rest문법

```javascript
// 1
const array = ["nodejs", {}, 10, true];
const [node, obj, bool] = array; //"node" {} 10
const [node, obj, ...bool] = array; 

// 2
const n = (x,...y) => console.log(x,y)n(4,5,6,7,8)
//4를 제외한 모든 수는 y에 들어감
//(4)[5,6,7,8]
```

**argument**를 이용해서 객체의 나머지 속성들을 받아왔습니다.  그러나 **rest**의 등장으로 조금 더 편하게 나머지 속성을 받아올 수 있게 되었습니다.

```javascript
Const p= (…rest) => console.log(rest)P(5,6,7,8,9)
//(5)[5,6,7,8,9]
```



### 객체참조

```javascript
const x = {a : 1, b:2}
let y = x;
```

: y는 x를 참조합니다. 따라서 x 의 내용이 바뀌면, y의 내용도 내용이 같이 변합니다. x자체는 바꿀 수 없지만, 안에있는 속성들( ex a,b등) 은 마음대로 바꿀 수 있습니다.