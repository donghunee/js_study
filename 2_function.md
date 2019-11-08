## 화살표 함수

```javascript
// 일반함수
function add(a, b) {
  return a + b;
}

// 화살표 함수
(a,b) => { return a+b }  
```



1. 함수 선언문

```javascript
// 함수 선언문
function add1(x,y) {    
  return x+y;}
```



2. 함수 표현 식 : 변수를 선언하고, 함수를 대입하는 식을 말합니다.

```javascript
// 화살표 함수 X
var add1 = function(x,y) {    
  return x + y;}; 

// 화살표 함수 O
var add2 = (x,y) => { return x + y;} //function 삭제    
var add3 = (x,y) => return x+y //함수의 내용이 return 밖에 없는 경우

```



####화살표 함수의 다양한 표현 방법

```javascript

// 매개변수 지정 방법
    () => { ... } // 매개변수가 없을 경우
     x => { ... } // 매개변수가 한 개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... } // 매개변수가 여러 개인 경우, 소괄호를 생략할 수 없다.

// 함수 몸체 지정 방법
x => { return x * x }  // single line block
x => x * x             // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다.
           
// 호출 방법
const pow = x => x * x;
console.log(pow(10)); // 100
```

함수표현식은 function을 삭제하고, 화살표를 표시하는 방법으로 표기법이 변경되었습니다. 또한 <u>함수의 내용이 return 밖에 없는 경우, x,y를 매개변수로 받은 후, 중괄호 없이 바로 리턴합니다.</u>



###this 의 역할

function안에 있는 this와 화살표함수 안에있는 this는 다른 역할을 한다는 점이, 일반 함수화 화살표 함수의 가장 큰 차이점 입니다.

```javascript
//function안에 있는 this
var relationship1 = {    
  name : 'zero',    
  friends : ['nero','hero','xero'],    
  logFriends : function() {        
    var that = this; //this는 relationship1을 가르킴
    this.friends.forEach(function(friend){            
      console.log(that.name, friend);        
    });    
  },};

relationship1.logFriends();
```

> 함수 호출 방식에 의해 [this](https://poiemaweb.com/js-this)에 바인딩할 어떤 객체가 동적으로 결정됩니다. **함수를 호출할 때 함수가 어떻게 호출되었는지에 따라** this에 바인딩할 객체가 동적으로 결정된다.

: Function에서 this 는 자신이 속해있는 객체이름을 말합니다.



* 화살표 함수 안에 있는 this

```javascript
//화살표 함수 안에 있는 this
var relationship1 = {    
  name : 'zero',  
  friends : ['nero','hero','xero'],    
  logFriends() {        
    this.friends.forEach(friend =>{        
      //바로 밖에 있는 this와 같은 것으로 만들어 줌            
      console.log(this.name, friend);        
    });    
  },};

relationship1.logFriends();
```

> 화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정된다. 동적으로 결정되는 일반 함수와는 달리 **화살표 함수의 this 언제나 상위 스코프의 this를 가리킨다.** 