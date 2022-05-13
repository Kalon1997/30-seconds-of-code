---
title: Avoid Callbackhell with Promise or Async / Await
tags: callback,callbackhell,promise,asyncawait
expertise: intermediate
firstSeen: 2022-05-12T05:00:00-04:00
---

Following codes explains callbackhell and how Promise and Async/Await can help.

- Part I Callbackhell.
- Calling one callback function inside another and so on is callbackhell.
- First we are defining three functions addTen, subFive and mulTwo. 
- These three functions while called with a number, will return a callback.
- The callback function will return either result or error.

```js
const addTen = (num, callback) =>
  {return callback(num+10, false)}
```

```js
const subFive = (num, callback) =>
  {return callback(num-5, false)}
```

```js
const mulTwo = (num, callback) =>
  {return callback(num*2, false)}
```

- Now lets call these one by one in nested way.
- The result of previous will serve as input for next callback.

```js
const ans = addTen(5, (addRes, addErr) => { // addRess = 15
        if(!addErr)
        {
            return subFive(addRes , (subRes, subErr) => { //subRes = 10
                if(!subErr){
                    return mulTwo(subRes, (mulRes, mulErr) => {
                        if(!mulErr)
                        {
                            return mulRes; //20
                        }
                    })
                }
            })
        }
    }) 
console.log(ans); // 20
```

- Part II Promise.
- Promise has two parameters resolve and reject. 
- Rewrting those three function definations as well, without a callback.

```js
const addTen = (num) => {return num+10}
```

```js
const subFive = (num) => {return num-5}
```

```js
const mulTwo = (num) => {return num*2}
```

- Creating a promise.

```js
const promise = new Promise((resolve, reject) => {
    if(true)
      resolve(5)
    else
      reject("Something went wrong ")
})
```

- Calling those three functions one by one.
- "then" will keep on returning the result and if any error "catch" will catch it.

```js
promise.then(addTen).then(subFive).then(mulTwo).then((ans)=>{
console.log(ans)
}).catch((err)=>{console.log(err)});
```

- Part III Async / Await.
- It actually uses promise internally.

```js
const addTen = ( num ) => {
    return new Promise( ( resolve, reject ) => {
        resolve( num+10)
    } )
}
```

```js
const subFive = ( num ) => {
    return new Promise( ( resolve, reject ) => {
        resolve( num-5)
    } )
}
```

```js
const mulTwo = ( num ) => {
    return new Promise( ( resolve, reject ) => {
        resolve( num*2)
    } )
}
```

- Put Async keyword before function name and Await before the statments inside the function
- Await will make the later code wait until the result of that statement is returned.
- Always put this inside a try/catch block.

```js
const ans = async (num) => {
    try {
        var addRes = await addTen(num);
        var subRes = await subFive(addRes);
        var mulRes = await mulTwo(subRes);
        console.log(mulRes)
    } catch (err) {
        console.log(err)
    }
}
ans(5)
```