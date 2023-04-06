## forEach 中异步任务的执行

```javascript
const list = [1,2,3]
const square = (num)=>{
    return new Promise((resolve,reject)=>{
        setTimeout(() => {
            resolve(num*num)
        }, 1000);
    })
}
const test = function(){
    list.forEach(async (item)=>{
        const res = await square(item)
        console.log(res)
    })
}
test()

// 结果：
// 等待 1 秒，1、4、9 一起输出
```

用 `for`/`for of` 循环改造：
```javascript
const square = (num)=>{
    return new Promise((resolve,reject)=>{
        setTimeout(() => {
            resolve(num*num)
        }, 1000);
    })
}

async function test1(){
    for(let i=0;i<list.length;i++){
        const res = await square(list[i])
        console.log(res)
    }
}
test1()

async function test2(){
    for(let val of list){
        const res = await square(val)
        console.log(res)
    }
}
test2()
```

- `forEach` 并发执行异步任务，不会按顺序执行
- `for of` 和普通的 `for` 循环却能顺序执行异步任务