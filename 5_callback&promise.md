

네트워킹 관련 코드를 짜고있다고 가정해 봅시다!!

```javascript
function logName() {
  var user = fetchUser('domain.com/users/1'); // 2) 비동기함수
  if (user.id === 1) {
    console.log(user.name); // 1)
  }
}
```

함수 logname()를 간단하게 유저의 정보를 받아오는 비동기함수라고 가정해봅시다. 
return 값으로 user 정보를 가져오고 if문을 통해 user.name을 찍는 간단한 함수입니다.

하지만 문제가 생기죠. 바로 user정보가 언제 넘어올지 모르기때문에 실행순서를 보장받을 수 없다는 것입니다.

그래서 생긴 자바스크립트의 특징이 바로 callback입니다. callback을 사용함으로서 실행 순서를 보장받을 수 있게 됩니다.

```javascript
function logName() {
  // 아래의 user 변수는 위의 코드와 비교하기 위해 일부러 남겨놓았습니다.
  var user = fetchUser('domain.com/users/1', function(user) {
    if (user.id === 1) {
      console.log(user.name);
    }
  });
}
```

---


## 콜백


```javascript
Users.findOne('zero', (err,user)=>{ // 2) user을 발견하면 실행    
  if(error){        
    return console.error(error);    
  }
  console.log(user);
});
console.log("다 찾았니?") // 1) 먼저 실행
```



Users라는 테이블이 있는 데이터베이스에 , zero라는 한 사람을 찾는  방법 : 콜백 

// 위 코드에서 (err,user) 이후 구문을 콜백 이라고 합니다.


- 콜백을 사용하는 이유 : 비동기화, 논블로킹 (얼마나 걸릴지 모르기 때문에..) //자바스크립트는 비동기, 언제 데이터를 받을지 모름.

- 콜백이 연달아 이뤄지는 경우….(비동기라서 한번 콜백을 비동기 형태로 쓰면, 그 이후에 콜백은..쿼리문은.. 그 안에 써 줘야 함) ~~.. 뭐래..

그런데 다음과 같은 코드가 작성될 수 있음

```javascript
step1(function (err, value1) {
    if (err) {
        console.log(err);
        return;
    }
    step2(function (err, value2) {
        if (err) {
            console.log(err);
            return;
        }
        step3(function (err, value3) {
            if (err) {
                console.log(err);
                return;
            }
            step4(function (err, value4) {
                // 정신 건강을 위해 생략
            });
        });
    });
});
```

보기만해도 머리가 어지러워지지 않는가?? 이것이 바로 콜백 지옥(callback hell)이라는 것..

js를 다루다보면 어쩔 수 없이 마주하게 되는 문제...

이를 해결하기 위한 방법이 여럿 있지만 가장 깔끔한 방법이 하나 있다!! 바로

## 프로미스

ECMA javascript 6 

```javascript
Users.findOne('zero')    
  .then((user)=>{        
  console.log(user);        
  return Users.update('zero','nero');    
})    
  .then((updateUser) => {        
  console.log(updateUser);        
  return Users.remove('nero');    
})     
  .then((removeUser) => {        
  console.log(removeUser);    
})    
  .catch((err)=>{        
  console.error(error);    
});

console.log('다 찾았니?');
```



1) **프로미스란?**

Promise 는 함수라기 보다는 생성자라고 할 수 있음!( 함수 첫글자가 대문자인 경우는 생성자!)

프로미스를 사용하면, 비동기작업들을 순차적으로 진행할 수 있게 됩니다. ( 자바스크립트에서의 비동기 작업이란, 특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성을 말함!) 뎁스가 제한되기 때문에 가독성이 좋습니다. 그러나 모든 곳에 프로미스를 쓸 수 있는 것은 아닙니다.

프로미스는 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용합니다. 일반적으로 웹 애플리케이션을 구현할 때 서버에서 데이터를 요청하고 받아오기 위해 아래와 같은 API를 사용합니다.

2) **프로미스 만들기**

//껍데기를 만들어놓고! .. 코딩!

1. 프로미스 선언

```javascript
const plus = new Promise((resolve , reject) =>{    
  const a =1;    
  const b =2;    
  
  if(a+b > 2){        
    resolve(a+b);//성공메세지    
  }else{        
    reject(a+b); //실패메세지    
  }
  
}));
//반환값 : 3
```

```javascript
plus.then((success)=>{        
  console.log(success);    
})      
  .catch((fail)=>{        
  console.log(fail);    
})
//반환값 : Promise {<resolved>: undefined}
```

promise를 이용함으로써 then/catch사용이 가능 해 짐!



2. 

```javascript
const Users = {    
  findOne() {        
    return new Promise((res,rej)=>{            
      if('사용자를 찾았으면'){                
        res('사용자');            
      }else{                
        rej('못 찾았어요')            
      }        
    })    
  },    
  remove() { return new Promise()},        
  update() { return new Promise()},
}

Users.findOne()    
  .then()  
  .catch()
```



3) **프로미스의 흐름**

```javascript
promise    
  .then((message)=>{        
  return new Promise((resolve,reject)=>{            
    resolve(message2);        
  });    
})    
  .then((message2)=>{        
  return new Promise((resolve,reject)=>{ 
    if (message2 == 'false'){
        reject(new Error('에러가 발생'))
    }           
    console.log(message2);        
    resolve(message3);        
  });    
})    
  .then((message3)=>{        
    console.error(error);    
})
  .catch((err) => {
    console.log(err)
});
```

=> 프로미스 실행 후, 인자값은 다음 then의 인자값으로 넘겨줍니다.

1. resolve의 메세지는 이후 then의 인자로 넘어갑니다.

2. reject의 메세지는 모두 건너뛰고 catch로 넘어갑니다.

3. then()에서 return을 사용한다면, 이는 resolve와 동일하게 작동합니다.

```javascript
promise.then((data) => data.json) // resolve(data.json)와 동일한 결과 
    .then((data) => {
        console.log(data)
    })
```

##프로미스(Promise) API

```javascript
//기본 프로미스
const promise = new Promise((res,rej)=>{    
  rej("실패");}); 

//무조건 성공하는 프로미스
const successPromise = Promise.resolve('성공')    
.then(); 

//무조건 실패하는 프로미스
const faillurePromise = Promise.reject('실패')    
.catch();
```

ㅇ프로미스들을 또한 모두 모아 놓고 사용 할 수도 있습니다.

```javascript
Promise.all([Users.findOne(), Users.remove(), Users.update()])     
  .then((result)=> {}) //result도 배열의 형태로! 단, result가 모두!! true일때만 들어올 수 있음.      
  .catch((error)=>{})
```

ㅇPromise를 아주쉽게 말하면, 수행을 했는데, 바깥에 티를 안내는 것 뿐인 것이라고 할 수 있습니다.

ㅇ결과는 이미 갖고있는데 결과를 내보내지 않습니다 .then을 만나야만 결과를 볼 수 있습니다. 

이렇게 결과(데이터를 가져오는 부분)와 결과를 보여주는 것이 분리되어 있다는 것도 콜백과의 차이점 입니다.

