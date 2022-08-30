title: Promise 对象
author: l1n6yun
tags:
  - Promise
categories: []
date: 2022-01-28 00:23:00
---
> Promise 对象代表了未来将要发生的事件，用来传递异步操作的消息。简单来说就是一个容器，里面保存着某个未来才会结束的事件(通常是一个异步操作的结果)

## 简介

ECMAscript 6 原生提供了 Promise 对象。

Promise 对象代表了未来将要发生的事件，用来传递异步操作的消息。简单来说就是一个容器，里面保存着某个未来才会结束的事件(通常是一个异步操作的结果)

Promise 对象的基本语法：

```js
var promise = new Promise(function(resolve, reject) {
    // 异步处理
    // 处理结束后、调用resolve 或 reject
});
```

## 特点

1. 对象的状态不受外界影响

   Promise 对象表示一个异步操作，有三种状态

   - pending: 初始状态

   - fulfilled: 操作成功
   - rejected: 操作失败

2. 一旦状态改变，就不会再变，状态就凝固了

列如:

```js
var promise = new Promise(function(resolve, reject) {
    resolve('success')
    console.log('after resolve')
    reject('error')
});

promise.then(result => {
   console.log(result);
});

promise.catch(result => {
    console.log(result);
})

// 运行结果
after resolve
success
Promise { <state>: "fulfilled", <value>: "success" }
```

`resolve` 后面的语句其实是可以执行的。那么为什么 `reject` 的状态信息在下面没有接受到呢？这就是 Promise 对象的状态凝固特点。`new` 出一个 Promise 对象时，这个对象的起始状态就是 pending 状态，再根据 `resolve` 或 `reject` 返回 fulfilled 状态或 rejected 状态。

## 传递回调

Promise 对象可以调用 `promise.then()` 方法，传递 `resolve` , `reject` 方法的回调。

```js
promise.then(onFulfilled, onRejected)
promise.then(onFulfilled)		// 只用于接受 resolve 处理
promise.then(null,onFulfilled)	// 只用于接受 reject 处理

// promise简化了对error的处理
promise.then(onFulfilled).catch(onRejected)
```

## 链式调用

我们来执行一段代码

```js
let p = new Promise((resolve,reject) => {
    reject('reject');
});

let resultP = p.then(null,result => {
    console.log(result);
});

console.log(resultP);

// 运行结果
Promise { <state>: "pending" }
reject
```

js 的执行顺序就是这样，同步->异步->回调，在同步执行的时候，Promise 对象还处于 pending 的状态，也说明了这个 then 返回的是一个 Promise 对象。

而且必须在then里面给一个返回值，才能继续调用，否则undefined。

```js
let p = new Promise((resolve,reject) => {
    reject('error');
});

let resultP = p.then(null,result => {
    console.log(result);
    return 123;
});

// console.log(resultP);
resultP.then(tmp => {
    console.log(tmp);
})

// 运行结果
error
123
```

## Promise.resolve()

将现有对象转为 Promise 对象的快捷方式。

传递一个普通的对象：

```js
let p1 = Promise.resolve({name:'l1n6yun',age:'18'});
// Promise { <state>: "fulfilled", <value>: { name: "l1n6yun", age: "18" } }

p1.then(result => {
    console.log(result);
});

// 运行结果
Object { name: "l1n6yun",age:"18" }
```

如果是Promise对象呢，直接返回

```js
let p = new Promise((resolve,reject) => {
    setTimeout(() => {
        resolve('success');
    },500);
});

let pp = Promise.resolve(p);

pp.then(result => {
    console.log(result);
});

console.log(pp == p);

// 运行结果
true
success
```

## Promise.reject()

快速的获取一个拒绝状态的 Promise 对象。

```js
let p = Promise.reject(123);

console.log(p);

p.then(result => {
    console.log(result);
}).catch(result => {
    console.log('catch',result);
})

// 运行结果
Promise { <rejected> 123 }
catch 123
```

## Promise.all()

`Promise.all()` 方法用于对多个 Promise 实例包装成一个新的 Promise 实例。

```jsx
let p1 = Promise.resolve(123);
let p2 = Promise.resolve('hello');
let p3 = Promise.resolve('success');


Promise.all([p1,p2,p3]).then(result => {
    console.log(result);
})

// 执行结果
Array(3) [ 123, "hello", "success" ]
```

成功之后就是数组类型，但所有状态都是成功状态才可以返回数组吗。如果其中有一个对象状态为 reject 就返回 reject 的状态值。

```js
let p1 = Promise.resolve(123);
let p2 = Promise.resolve('hello');
let p3 = Promise.resolve('success');
let p4 = Promise.reject('error');

Promise.all([p1,p2,p4]).then(result => {
    console.log(result);
}).catch(result => {
    console.log(result);
});

// 执行结果
error
```

```js
//用sleep来模仿浏览器的AJAX请求
function sleep(wait) {
    return new Promise((res,rej) => {
        setTimeout(() => {
            res(wait);
        },wait);
    });
}

let p1 = sleep(500);
let p2 = sleep(500);
let p3 = sleep(1000);

Promise.all([p1,p2,p3]).then(result => {
    console.log(result);
    //.....
    //loading
});
```

## Promise.race()

`Promise.race()` 同样是键多个 Promise 实例包装成一个新的 Promise 实例。

和 all 同样接受多个对象，不同的是 race 接受的对象中，哪个对象返回的快就返回哪个对象。

就如race直译的赛跑这样。如果对象其中有reject状态的，必须catch捕捉到，如果返回的够快，就返回这个状态。race最终返回的只有一个值。

```jsx
//用sleep来模仿浏览器的AJAX请求
function sleep(wait) {
    return new Promise((res,rej) => {
        setTimeout(() => {
            res(wait);
        },wait);
    });
}

let p1 = sleep(500);
let p0 = sleep(2000);

Promise.race([p1,p0]).then(result => {
    console.log(result);
});

let p2 = new Promise((resolve,reject) => {
    setTimeout(()=>{
        reject('error');
    },1000);
});

Promise.race([p0,p2]).then(result => {
    console.log(result);
}).catch(result => {
    console.log(result);
});

// 执行结果
500
error
```

## 异常处理

> 錯誤處理的聲音實在安靜,安靜得聽不見 from Nolan Lawson 

当 promise 被明确拒绝时,会发生拒绝；但是如果是在构造函数回调中引发的错误,则会隐式拒绝。

为什么说安静，一个例子， Promise 内部的错误外界用 try-catch 捕捉不到

```jsx
try {
    let p = new Promise((resolve, reject) => {
        throw new Error("I'm error");
        // reject(new Error("I'm Error"));
    });
}catch(e) {
    console.log('catch',e);
}
```

结果什么都没打印。
 但是抛出的错误可以通过catch来捕捉：

```jsx
// try {
    let p = new Promise((resolve, reject) => {
        throw new Error("I'm error");
        // reject(new Error("I'm Error"));
    });
// }catch(e) {
//     console.log('catch',e);
// }


p.catch(result => {
    console.log(result);
});
```

这样就捕捉到错误。所以:

**建议**在 Promise 的链的尾部必须要有个catch接着。