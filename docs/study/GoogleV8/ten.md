# Generator函数与协程原理及实现

进程与线程的概念就不一一叙述了

## 协程

协程是一种基于线程之上，但是又比线程更加轻量级的存在，这种由程序员自己写程序来管理的轻量级线程叫做【用户空间线程】，具有对内核来说不可见的特性

正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程

### 协程的Generator函数实现

Generator函数就是异步任务的一个容器

通过yield关键字指定异步任务

```javascript
function* gen(x) {
    var y = yield x + 2 // 阶段一，用yield来划分阶段
    return y // 阶段二
}
var g = gen(1)
g.next() // {value:3,done:false}，调用一次next执行一个阶段，done表示函数内部还有没有执行完成的阶段
g.next() // {value:undefined,done:true}

function* gen(x) {
    var y = yield x + 2 // 阶段一，x+2  用yield来划分阶段
    return y // 阶段二 var y ={上一个阶段返回的结果}，return y
}
var g = gen(1)
g.next() // {value:3,done:false}，调用一次next执行一个阶段，done表示函数内部还有没有执行完成的阶段
g.next(2) // {value:3,done:true}，将上一个阶段返回的结果2给下一个阶段即，var y =2，return y


function* gen(x) {
    try{
         var y = yield x + 2 
    }catch(e){
        console.log(e)
    }
    return y 
}
var g = gen(1)
g.next()
g.throw('出错了') // 用于抛错

```

### 异步任务封装

还需要详细了解Generator函数~~

```JavaScript
var fetch = require('node-fetch')

function* gen(){
    var url ='https://...'
    var result = yield fetch(url) // 阶段一：var url ='https://...'；fetch(url)
    console.log(result) // 阶段二：var result = {}，console.log(result)
}

var g = gen()
var result = g.next()
result.value.then(function(data){
    retuen data.json();
}).then((data)=>{
    g.next(data)
})
```

## 基于Thunk函数封装Generator自动执行器

### 传值调用和传名调用

对于如下函数

```javascript
var x=1
function f(m){
	return m*2
}
f(x+5)
```

传值调用

```javascript
func(x+5)
等同于
func(6)
```

传名调用

```javascript
func(x+5)
等同于
(x+5)*2
```

传值调用和传名调用，各有各的好处，取决于入参在函数内部是否被使用以及使用的频率

### thunk函数的含义

```javascript
var x=1
function f(m){
	return m*2
}
f(x+5)
// 等同于
var thunk = function(){
	return x+5
}
function f(thunk){
    return thunk()*2
}
```

 定义一个自动执行器

```javascript
var fetch = require('node-fetch')

function* gen(){
    var url ='https://...'
    var result = yield fetch(url) // 阶段一：var url ='https://...'；fetch(url)
    console.log(result) // 阶段二：var result = {}，console.log(result)
}
function run(gen) { // gen是一个Generator函数，优点类似于前端常用的对axios的封装
    var g = gen();
    function next(data) {
        var result = g.next()
        if(result.done) return result.value
        result.value.then(function(data){
            return data.json()
        }).then(function(data){
            next(data)
        })
    }
    next()
}
//将Generator函数作为入参传进去即可
run(gen)
```

