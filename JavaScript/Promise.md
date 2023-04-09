# 构造函数 Promise

## `then()` 方法

```javascript
const p = new Promise((resolve, reject) => {
    resolve('成功……');
    // reject('失败……');
});
// then 方法可以接收两个参数，这两个参数都是函数
// then 方法的返回值是一个新的 promise 对象，状态是 pending
const t = p.then((result) => {
    // 当 promise 的状态是 fulfilled 时，执行
}, (reason) => {
    // 当 promise 的状态是 rejected 时，执行
});

// 正因为返回了一个新的 promise，所以能实现链式操作
t.then().then().then()...
```

## `forEach` 中异步任务的执行

```javascript
const arr = [1, 2, 3];
const squareFn = (num) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(num * num);
        }, 1000);
    });
};
const test = function () {
    arr.forEach(async (item) => {
        const res = await squareFn(item);
        console.log(res);
    });
};
test();

// 结果：
// 等待 1 秒，1、4、9 一起输出
```

用 `for`/`for of` 循环改造：

```javascript
const arr = [1, 2, 3];
const squareFn = (num) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(num * num);
        }, 1000);
    });
};

async function test1() {
    for (let i = 0; i < arr.length; i++) {
        const res = await squareFn(arr[i]);
        console.log(res);
    }
}
test1();

async function test2() {
    for (let val of arr) {
        const res = await squareFn(val);
        console.log(res);
    }
}
test2();

// 结果
// 间隔 1 秒，分别输出 1、4、9
```

-   `forEach` 并发执行异步任务，不会按顺序执行。
-   `for of` 和普通的 `for` 循环却能顺序执行异步任务。
