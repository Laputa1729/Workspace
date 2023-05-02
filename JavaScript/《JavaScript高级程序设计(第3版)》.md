# 《JavaScript 高级程序设计(第 3 版)》

## 第 2 章

-   `<script>`标签属性

    -   `defer`: 等待 DOM 完全构建，后台下载，延迟执行。保持先后顺序。
    -   `async`: 不阻塞 DOM 构建，加载完立即执行。不保证先后顺序。

## 第 3 章

-   基本数据类型`null`，表示一个 **_空对象指针_**
    -   `typeof null;  // 'object'`
    -   定义变量准备将来保存对象时，最好初始化为`null`
        > `var obj = null;`
-   特殊数值`NaN`，**_Not a Number_**

-   自增/自减

    ```javascript
    var counter = 1;
    var a = ++counter; // a: 2
    ```

    ```javascript
    var counter = 1;
    var a = counter++; // a: 1
    ```

-   字符串比大小
    -   实际比较的是字符对应的**字符编码值**
    -   大写字母的字符编码全部小于小写字母
    ```javascript
    var result = 'Brick' < 'alphabet'; // true
    ```
    ```javascript
    var result = 'Brick'.toLowerCase() < 'alphabet'.toLowerCase(); // false
    ```

## 第 4 章

-   参数是函数的**局部变量**
-   `typeof` 检测基本数据类型倒是得力，但对于引用类型的值，这个操作符用处就不大了
-   基本类型的值保存在 => 栈
-   引用类型的值保存在 => 堆
    -   复制引用类型的值，复制的其实是指针，因此两个变量最终都指向同一个对象。

## 第 5 章 引用类型

### Object

### Array

-   `sort()`

    -   会调用每个数组元素的 `toString()` 转型方法，再比较得到的字符串，以此来排序。

    ```javascript
    var arr = [0, 1, 5, 10, 15];
    arr.sort();
    console.log(arr); // [0, 1, 10, 15, 5]
    ```

    -   接收函数，自定义比较

    ```javascript
    // 升序 asc （要降序 desc，交换返回值即可）
    var arr = [0, 1, 5, 10, 15];
    arr.sort(function (item1, item2) {
        if (item1 > item2) {
            return 1; // 换位置
        } else if (item1 < item2) {
            return -1; // 不换
        } else {
            return 0; // 不换
        }
    });
    console.log(arr); // [0, 1, 10, 15, 5]
    ```

    -   数值类型，或 `valueOf()` 方法返回值是数值类型的对象，简化：

    ```javascript
    // 降序 desc
    var arr = [0, 1, 5, 10, 15];
    arr.sort(function (item1, item2) {
        return item2 - item1;
    });
    console.log(arr); // [15, 10, 5, 1, 0]
    ```

-   `slice()`

    ```javascript
    var arr = [0, 1, 5, 10, 15];
    var newArr1 = arr.slice(1); // [1, 5, 10, 15]
    var newArr2 = arr.slice(1, 4); // [1, 5, 10] 不包括第4项
    ```

    -   **如果参数是负数，用数组长度加上这个参数，以此来确定相应的位置：**
        ```javascript
        var newArr3 = arr.slice(-2, -1); // [10]
        ```
        -   `arr.length + (-2) = 3`
        -   `arr.length + (-1) = 4`
        -   `arr.slice(-2, -1) => arr.slice(3, 4)`

-   ★`splice()`
    -   删除
        -   `arr.splice(0, 2)` 从第`0`位开始，删除`2`个
    -   插入
        -   `arr.splice(2, 0, n1, n2, n3……)` 从第 `2` 位开始，`0`(表示不删东西)，插入多项
    -   替换
        -   `arr.splice(2, 1, n1, n2, n3……)` 把第 `2` 位的项，替换成别的项

#### 遍历方法

-   `Array.every(function(item, index, array) {})`
    -   **“且”**
    -   每一项都符合条件，返回 `true`，否则返回 `false`。
    -   某一项不满足，`return false`，剩余项不再检测。
-   `Array.some(function(item, index, array) {})`
    -   **“或”**
    -   每一项都未符合条件，返回 `false`。
    -   某一项满足，`return true`，剩余项不再检测。
-   `Array.filter(function(item, index, array) {})`
    -   筛出符合条件的项，返回新数组。
-   `Array.map(function(item, index, array) {})`
    -   对每一项进行加工，返回新数组。
-   `Array.forEach(function(item, index, array) {})`
    -   没有返回值。
-   `Array.reduce(function(prev, cur, curIndex, array) {}, initialValue)`
    -   返回计算结果。

### Date

```javascript
Date.now() === +new Date(); // true
```

### RegExp

### Function

> **“函数是对象，函数名是指针”**，函数名仅仅是一个包含指针的变量而已。

-   `fn.call(this, arg1, arg2, ……)`
-   `fn.apply(this, arguments)  <==> fn.apply(this, [arg1, arg2, ……])`

### `new String()`

```javascript
var s1 = 'Laputa';
var s2 = new String('Laputa');

typeof s1; // 'string'
typeof s2; // 'object'

new Object(s1) instanceof String; // true
s1 instanceof String; // false
```

-   ★`String.prototype.substring(beginIndex[, endIndex])` 比较特殊

    -   如果 `beginIndex` 等于 `endIndex`，`substring()` 返回一个空字符串。
    -   如果任一参数小于 `0` 或为 `NaN`，则被当作 `0`。
    -   如果任一参数大于 `str.length`，则被当作 `str.length`。
    -   如果 `beginIndex` 大于 `endIndex`，则两个参数调换位置。

    ```javascript
    var str = 'Laputa';

    str.substring(3); // 'uta'
    str.substring(3, 3); // ''
    str.substring(-3, NaN); // ''
    str.substring(9); // ''
    str.substring(3, 0); // 'Lap'
    ```

### `new Boolean()`

```javascript
var value_false = false;
var obj_false = new Boolean(false);

value_false && true; // false
obj_false && true; // true

typeof value_false; // 'boolean'
typeof obj_false; // 'object'
```

### `new Number()`

### 单体内置对象

-   Global 对象

-   Math 对象

    -   `Math.max()`
        ```javascript
        var arr = [1, 2, 3, 4, 5, 6, 7, 8];
        var max = Math.max.apply(Math, arr);
        console.log(max); // 8
        ```

## 第 6 章 面向对象

### 创建对象

-   工厂模式
-   构造函数模式
-   原型模式

    > 注：对象字面量重写原型对象，切断了新原型与旧实例对象之间的联系。
    > 逐条添加原型属性，却没有问题。

    ```javascript
    function Person() {
        // ...
    }

    var p0 = new Person();
    // 用对象字面量方式重写原型对象
    Person.prototype = {
        constructor: Person,
        name: 'Laputa',
        age: '30',
        sayHi: function () {
            console.log('Hi, ', this.name);
        },
    };
    // 重写原型之后
    p0.sayHi(); // caught TypeError: p0.sayHi is not a function.
    ```

### 继承

原型链 + 构造函数：使用原型链实现对原型（属性&方法）的继承，借用构造函数实现对实例（属性）的继承。这样，既通过在原型上定义方法实现了函数的复用，又保证每个实例都有自己的私有属性。

```javascript
function SuperClass(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}
SuperClass.prototype.sayHi = function () {
    console.log('Hi,', this.name);
};

function SubClass(name, age) {
    SuperClass.call(this, name); // 继承属性，继承父类的 name 和 colors，而且还能传参
    this.age = age;
}
// 继承方法
SubClass.prototype = new SuperClass();
// 私有方法
SubClass.prototype.constructor = SubClass;
SubClass.prototype.sayHello = function () {
    console.log('Hello,' + this.age + '岁');
};

var _instance1 = new SubClass('Maxj', 30);
_instance1.colors.push('yellow');

console.log(_instance1.colors); // ['red', 'blue', 'green', 'yellow']
_instance1.sayHi(); // Hi, Maxj
_instance1.sayHello(); // Hello, 30岁

var _instance2 = new SubClass('Laputa', '+ ∞ ');

console.log(_instance2.colors); // ['red', 'blue', 'green']
_instance2.sayHi(); // Hi, Laputa
_instance2.sayHello(); // Hello, + ∞ 岁
```

## 第 7 章 函数表达式

### 闭包

**闭包**是指有权访问另一个函数作用域中的变量的**函数**。

-   手动释放内存

    ```javascript
    function createComparisonFn(key) {
        // 这个返回的匿名函数有权访问 key
        return function (obj1, obj2) {
            var _value1 = obj1[key];
            var _value2 = obj2[key];

            if (_value1 < _value2) {
                return -1;
            } else if (_value1 > _value2) {
                return 1;
            } else {
                return 0;
            }
        };
    }

    // 创建函数
    var compareName = createComparisonFn('name');
    // 调用函数
    compareName({ name: 'Laputa' }, { name: 'Maxj' });
    // 解除对匿名函数的引用，释放内存
    compareName = null;
    ```

## 第 22 章 高阶函数

### 高级函数

#### 类型检测

```javascript
function isArray(value) {
    return Object.prototype.toString.call(value) === '[object Array]';
}

function isFunction(value) {
    return Object.prototype.toString.call(value) === '[object Function]';
}

function isRegExp(value) {
    return Object.prototype.toString.call(value) === '[object RegExp]';
}
```

#### 作用域安全的构造函数

-   当未使用 `new` 操作符来调用构造函数时，由于该 `this` 对象是在运行时绑定的，`this`会映射到全局对象 `window` 上。

#### 函数柯里化

> 官方的解释是：把接收多个参数的函数变换成只接后一个单一参数的函数，并返回接受余下的参数和结果的新函数的技术。
> 就是把一个需要传递多参的函数转换成传单参的形式。

```javascript
(function () {
    // 函数柯里化
    function curry(fn) {
        var args = Array.prototype.slice.call(arguments, 1);

        return function () {
            var innerArgs = Array.prototype.slice.call(arguments);
            // var finalArgs = args.concat(innerArgs);
            var finalArgs = [...args, ...innerArgs];

            return fn.apply(null, finalArgs);
        };
    }

    function add(num1, num2) {
        return num1 + num2;
    }

    var curriedAdd = curry(add, 5, 12);

    console.log(curriedAdd(3)); // 17
})();
```
