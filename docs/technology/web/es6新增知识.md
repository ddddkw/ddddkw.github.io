## 一、ES6中新增的特性
### 1.块级作用域
ES6中新增的块级作用域主要是新增let/const带来的
**let特性：**
1. 不允许在同一个作用域下重复声明变量；
2. let拥有块级作用域，这意味着在 if 语句、for 循环、while 循环等代码块中声明的 let 变量，只在该代码块内有效。
   **const特性**
3. const也拥有块级作用域；
4. 声明时必须进行初始化赋值，且赋值后不能再重新赋值修改其值；
5. const声明的变量实际上不是不可变的，只是变量指向的内存中的数据不可变，对于基本数据类型来说，变量指向的内存地址中存的就是数据本身，给我们带来的观感就是声明后的变量不可变；但如果声明的是引用数据类型，const指向的内存地址中存的是数据的指针，指针指向是不可变得，但是可以改变指针指向地址中的数据
### 2.箭头函数
箭头函数在为处理作用域和this指向问题时提供了更直接的问题
箭头函数的特性：
1. 箭头函数本身是不具有上下文的，而是继承外层函数的 this 值；这样在处理回调函数和对象方法时非常有效，可以避免this指向错误的问题
2. 箭头函数不能使用 new 操作符来创建实例，因为它们没有自己的 prototype 属性。
3. 箭头函数没有自己的 arguments 对象，但可以通过剩余参数来获取参数。
### 3.解构赋值
ES6中解构赋值是方便数据的提取和赋值，主要用于数组和对象操作中使用，它允许从数组或对象中提取值，并将其赋给变量。
### 4.模板字符串
ES6中的模版字符串方便进行字符串拼接处理，使用反引号进行表示（`），具体示例：

```javascript
let job = "程序员",
  salary = "100";
let say = `我的工作是${job}, 我每月可以挣${salary}大洋，真开心！`;
```
### 5.扩展运算符
ES6 的扩展运算符（...）是一种方便的操作符，有许多可以通过其扩展出来的功能
将数组展开：

```javascript
console.log(...[1,2,3])
```
数组拷贝：

```javascript
let arr = [1, 2, [3, 4]];
let arr1 = [...arr];
```
合并数组或对象
```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
var arr3 = [...arr1, ...arr2];
```
字符串转数组
```javascript
const title = "china";
const charts = [...title];
console.log(charts); // 输出：['c', 'h', 'i', 'n', 'a']
```
数组去重

```javascript
const arrayNumbers = [1, 5, 9, 3, 5, 7, 10, 4, 5, 2, 5];
const newNumbers = [...new Set(arrayNumbers)];
console.log(newNumbers); // 输出：[1, 5, 9, 3, 7, 10, 4, 2]
```
### 6.类
ES6中引入了类的概念，使面向对象编程在 JavaScript 中更加清晰和直观。
### 7.模块化
ES6中引入了模块化编程的概念，使得开发者可以将代码组织成独立的模块，可以更好地管理代码的复杂性，提高了代码重用性。
ES6模块化的重点在于import和export两个关键字，通过export可以导出模块中的函数，对象或者是任何值；在需要使用某个模块的地方通过import来引入模块进行使用
### 8.promise
promise是ES6中提供的一种处理异步操作的方法，相比较于传统的回调函数而言，promise提供了更加完整的控制流和错误处理机制；**了解到异步任务时，也可去了解下异步任务中的宏任务和微任务**。
promise表示一个尚未完成但是最终会完成的操作的对象。主要有三种状态：
**pengding：** 初始状态，既不是成功也不是失败
**Fullfilled：** 表示操作执行成功
**Rejected：** 表示操作失败
创建promise的实例时，需要传递一个构造函数，该构造函数接受一个带有两个参数的执行器函数（reslove和reject）

```javascript
const promise = new Promise((resolve, reject) => {
    // 异步操作
    setTimeout(() => {
        try {
            const result = 'Success!';
            resolve(result); // 成功时调用 resolve
        } catch (error) {
            reject(error); // 失败时调用 reject
        }
    }, 1000);
});
promise
    .then(result => {
        console.log('Success:', result);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```
promise中的一系列方法：
**then：**  注册回调函数用于处理promise返回成功的结果
**catch：**  注册回调函数用于处理promise失败的结果
**finally：**  无论promise执行的结果是成功还是失败，都会执行finally中的结果
promise还支持链式调用，可以连续的调用then，catch和finally
promise.all()会等待所有的promise执行完毕后再去执行后续操作，如果有任何一个promise执行失败都会拒绝并且会在catch中返回第一个被拒绝的原因
```javascript
const promises = [
    new Promise(resolve => setTimeout(() => resolve('First'), 1000)),
    new Promise(resolve => setTimeout(() => resolve('Second'), 2000)),
    new Promise(resolve => setTimeout(() => resolve('Third'), 3000))
];
Promise.all(promises)
    .then(results => {
        console.log('All promises resolved:', results);
    })
    .catch(error => {
        console.error('One of the promises rejected:', error);
    });
```
promise.race()方法用于等待传入的 Promise 中的第一个完成（成功或失败）。一旦有一个 promise完成，promise.race() 就会立即返回。
### 9.Map/Set
Map和Set是ES6中新增的两种数据结构，
**Map：** 是一种集合类型，可以用于存储任意类型的键值对，与js中的普通的对象不同的是，Map中的键可以是任意类型的数据，甚至可以是对象。
创建一个新的map对象
```javascript
const myMap = new Map();
```
还可通过set，get，has，delete，clear等方法对Map进行操作
**Set：** set也是一种集合类型，存储唯一值，它与Map的区别在于Set中不存储键，只存储值
创建一个新的Set对象
```javascript
const mySet = new Set();
```
Set中也有一系列的add，has，delete，clear等方法
### 11.symbol
Symbol 是一种新的原始数据类型，它主要用于创建唯一的键名，特别是在对象属性和某些场景下的标识符上。Symbol 的设计目的是解决一些传统键名（如字符串键名）可能带来的问题，比如键名冲突等。多用于给元素创建属性使用
### 12.迭代器和生成器
迭代器（Iterator）和生成器（Generator）是两个重要的概念，它们为 JavaScript 提供了更强大的迭代和生成数据的能力。
### 13.其它新增内容
除上述内容外，还新增了一些数据结构的处理方法，比如数组等还，还为函数新增了一些扩展

---
