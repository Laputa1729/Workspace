# 构造函数 Promise

## `then()` 方法

```javascript
const p0 = new Promise((resolve, reject) => {
    resolve('成功……');
    // reject('失败……');
});
// then 方法可以接收两个参数，这两个参数都是函数
// then 方法的返回值是一个新的 promise 对象，状态是 pending
const p1 = p0.then((result) => {
    // 当 promise 的状态是 fulfilled 时，执行
}, (reason) => {
    // 当 promise 的状态是 rejected 时，执行
});

// 正因为返回了一个新的 promise，所以能实现链式操作
p1.then().then().then()...
```

-   ★ `promise` 的状态不改变，`then()` 里的回调函数就永远不会执行

    ```javascript
    const p0 = new Promise((resolve, reject) => {
        resolve('ook'); // 将 p0 的状态改成 fulfilled
    });

    const p1 = p0.then(
        (result) => {
            console.log('成功……', result);
            // 这里 return，可以将 p1 的状态改成 fulfilled
            // 这样，p1 的 then() 方法里的回调函数也会执行了
            return 123;

            // Note: 如果这里的代码报错，p1 的状态就变成了 rejected，p1 的 then() 方法自然能捕获到 Error
        },
        (reason) => {
            console.log('失败……');
        }
    );

    p1.then((res) => {
        console.log('成功1……', res);
    });

    // 结果：
    // 成功…… ook
    // 成功1…… 123
    ```

## `catch()` 方法

1. 当 `promise` 的状态改为 `rejected` 时，执行
2. 当 `promise` 执行体内出现代码错误时，执行
3. 人为抛出一个异常，也会执行

```javascript
const p = new Promise((resolve, reject) => {
    // reject();
    // console.log(abc);
    throw new Error('出错了。');
});

p.catch((error) => {
    console.log('失败', error);
});

console.log(p);
```

## `Promise` 解决回调地狱

```javascript
new Promise((resolve, reject) => {
    $.ajax({
        url: 'data1.json',
        type: 'GET',
        success: function (res) {
            if (res && res.succeed) {
                // 改变 promise 的状态为 fulfilled，结果为 res
                // 这样，promise 的 then() 方法就能拿到 res 了
                resolve(res);
            }
        },
        error: function (err) {
            // promise 的 catch() 方法捕获到 err
            reject(err);
        },
    });
})
    .then((result) => {
        console.log(result);
    })
    .catch((error) => {
        console.log(error);
    });
```

```javascript
async function func() {
    const promiseA = fetch('http://.../post/1');
    const promiseB = fetch('http://.../post/2');

    const [a, b] = await Promise.all([promiseA, promiseB]);
    // ...
}
```

## `forEach`/`map` 中异步任务的执行

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

-   `forEach`/`map` 并发执行异步任务，不会按顺序执行。
-   `for of` 和普通的 `for` 循环却能按顺序执行异步任务。


## async / await

- `async` 修饰的异步函数，它的返回值会自动包装为 `Promise`

    ```Javascript
    function fn1() {
        return new Promise((resolve) => {
            resolve(666);
        });
    }

    async function fn2() {
        return 666;
    }

    // fn1 与 fn2 完全等价
    ```

- 异步任务连续传值

    > then 方式
    ```Javascript
    async function fn1() {
        return 10;
    }
    async function fn2(num) {
        return num + 2;
    }
    async function fn3(num) {
        return num + 3;
    }
    async function fn4(num) {
        return num + 4;
    }

    fn1()
        .then(fn2)
        .then(fn3)
        .then(fn4)
        .then(res => console.log('最终结果：', res))
        .catch(e => console.log('出错了~~'));

    console.log('End...');

    // 结果：
    // End...
    // 最终结果： 19
    ```

    > await 方式
    ```Javascript
    async function fn1() {
        return 10;
    }
    async function fn2(num) {
        return num + 2;
    }
    async function fn3(num) {
        return num + 3;
    }
    async function fn4(num) {
        return num + 4;
    }

    async function getResult() {
        let res = await fn1();
        res = await fn2(res);
        res = await fn3(res);
        res = await fn4(res);

        console.log('最终结果：', res);
    }

    getResult();

    console.log('End...');

    // 结果：
    // End...
    // 最终结果： 19
    ```