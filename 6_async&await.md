## async/await



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
async 함수() => {    
    const 변수 = await 값
}
```

async/await도 Promise기반입니다.

단, await문은 try-catch 문으로 예외처리를 해 줘야 합니다.

## 장점

1. 간결함과 깔끔함  
위의 promise로 짠 코드들에 비하면 훨씬더 깔끔하고 기존 문법처럼 읽을 수 있다

2. 에러 핸들링

```javascript
const makeRequest = () => {
  try {
    getJSON()
      .then(result => {
        // 요기서 에러가 발생!!
        const data = JSON.parse(result)
        console.log(data)
      })
      // promise의 에러핸들링은 요기서..
      // .catch((err) => {
      //   console.log(err)
      // })
  } catch (err) { // 에러를 못잡는다
    console.log(err)
  }
}
```

```javascript

const makeRequest = async () => {
  try {
    // 에러가 발생한다면!!
    const data = JSON.parse(await getJSON())
    console.log(data)
  } catch (err) { // 요기서 처리 가능!!
    console.log(err)
  }
}
```

3. 분기

```javascript
const makeRequest = () => {
  return getJSON() // 요기서 
    .then(data => {
      if (data.needsAnotherRequest) {
        return makeAnotherRequest(data) // 또 요기서!!
          .then(moreData => {
            console.log(moreData)
            return moreData
          })
      } else {
        console.log(data)
        return data
      }
    })
}
```

```javascript
const makeRequest = async () => {
  const data = await getJSON() // 요기를 요렇게
  if (data.needsAnotherRequest) {
    const moreData = await makeAnotherRequest(data); // 요기를 저렇게
    console.log(moreData)
    return moreData
  } else {
    console.log(data)
    return data    
  }
}
```

동기 코드처럼 진행 가능

** 참고 ㅣ https://joshua1988.github.io/web-development/javascript/js-async-await/
