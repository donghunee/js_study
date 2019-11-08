## async/await

제너레이트

\* yield : 순서대로 실행되는 것 처럼 보이게 착각!

```javascript
//프로미스
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


//async/await
async function() => {    
  const user = await Users.findOne('zero');    
  const updatedUser = await Users.update('zero','nero');    
  const removedUser = await Users.remove('nero');    
  console.log('다 찾았니?')}
```

위 코드는 같은 코드를 각각 promise 와 async/await 로  표현 한 것 입니다.



**async/await 표현**

```javascript
async 함수()=>{    const 변수 = await 값}
```

async/await도 Promise기반입니다.

단, await문은 try-catch 문으로 예외처리를 해 줘야 합니다.