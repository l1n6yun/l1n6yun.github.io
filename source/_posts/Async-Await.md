title: Async-Await
author: l1n6yun
tags:
  - Promise
categories: []
date: 2022-02-08 00:22:00
---
# Async-Await

异步编程的最高境界，就是根本不用关心它异步。

async 函数就是隧道尽头的亮光，很多人认为它是异步操作的终极解决方案。

# async-await 与 promise 的关系

不存在谁替代谁，因为 async-await 是寄生于 Promise。Generator 的语法糖。

**async** ：声明一个方法是异步的

**await** ：可以认为是 async wait 的简写，等待一个异步方法执行完成。

# 基本语法

```js
async function demo(params){

}

demo()
```

async 函数返回的是一个 Promise 对象

必须了解的 AsyncFunction `console.log(async function(){}.constructor)`

在 Chrome 里申明这样一个函数，可以在控制台看到返回的其实就是一个 Promise 对象。

扩展需要了解的就是 Chrome 现在也支持 AsyncFunction , 可以在 Chrome 控制台测试：

```js
console.log(async function(){}.constructor)
function AsyncFunction()
```

# 规则

1. async 表示这是一个 async 函数，await 只能用在这个函数里面。
2. await 表示在这里等待 Promise 对象返回结果后，在继续执行。
3. await 后面跟着的应该是一个 Promise 对象（当然，其他返回值也没关系，只是会立即执行，不过那样就没有意义了）

```js
async function demo() {
    let result = await Promise.resolve(123);
    console.log(result);
}
demo();
```

# 应用

Promise 虽然一方面解决了 `callback` 的回调地狱，但是相对的把回调“纵向发展”了，形成了一个回调链。

```js
function sleep(wait) {
    return new Promise((res, rej) => {
        setTimeout(() => {
            res(wait)
        }, wait)
    })
}

// 回调地狱
sleep(100).then(result01 => {
    return sleep(result01 + 100);
}).then(result02 => {
    return sleep(result02 + 100);
}).then(result03 => {
    console.log(result03); // 300
})

// 改成async/await写法就是
async function demo(){
    // await 是强制把异步变成了同步，这一句代码执行完，才会执行下一句
    let result01 = await sleep(100);
    let result02 = await sleep(result01 + 100);
    let result03 = await sleep(result02 + 100);
    return result03;
}

demo().then(result => {
    console.log(result);	// 300
});
```

# 错误处理

既然 `.then(...)` 不用写了，那么 `.catch(...)` 也不用写，可以直接用标准的 `try catch` 语法捕捉错误。

```js
let p = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        reject('error')
    },1000)
})

async function demo(params){
    try{
        let result = await p;
    }catch(e){
        console.log(e)
    }
}

demo();	// error
```

这是基本的错误处理，但是当内部出现错一些错误时，和上面Promise有点类似，demo() 函数不会报错，函数需要catch回调捕捉吗，这就是内部错误被‘静默’处理了。

```js
let p = new Promise((resolve,reject) => {
    setTimeout(() => {
        reject('error');
    },1000);
});

async function demo(params) {
    let result = await p;
}

demo().catch((err) => {
    console.log(err);
})
```

# 注意你的并行执行和循环

如果这你想异步发出AJAX请求，使用 await 代码会同步执行，所以 async/await 需要谨慎使用。

await in for 循环：await 的执行上下文必须是 async 函数

现在有一些 `.forEach` 或者 `.map` 的循环里，比如在 `.forEach` 里使用 await , 这时候的上下文就变成了 Array ,而不是 `async function` ,就会报错。这时候你就要想到是什么错误。