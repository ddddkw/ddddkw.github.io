# Promise与控制反转

## 深入理解Promise

### 可靠性丢失

为什么会有可靠性丢失的问题，在1与2执行之间的空挡，我们对这个过程是不可控的。通俗的讲，就是我们无法控制2的回调函数的执行时机，尤其是当2是我们引用的一些第三方库的方法时

```javascript
// 1，现在立即执行的同步代码
someAsync(function(){
    // 2，未来执行的代码
})

```

### someAsync的改造

对someAsync添加一个成功或者失败的回调

```javascript
var listener =  someAsync(...);
    listener.on('success',function(data){
    	
	})
    listener.on('fail',function(data){
    	
	})
```

### Promise

```JavaScript
function someAsync() {
    return new Promise(function(resolve,reject){
        // resolve() or reject()
    })
};
var p = someAsync()
p.then(
	function(){
        // success happened
    }
    function(){
    	// failure happened
	}
)
```

Promise使我们重新获得了程序的控制权，但是Promise并不是移除了回调而是传递了回调，并不会解决回调地狱

#### 标准的Promsie机制

- 对象的状态不受外界映像。Promise对象代表一个异步操作，有三种状态：pending（进行中），fulfilled（已成功）和rejected（已失败）。只有异步操作的结果可以决定是哪一个状态，任何其它操作都无法改变这个状态
- 一旦状态改变了以后，就不会再变，任何时候都可以得到这个结果。两种改变请况：pending变为fulfilled，pending变为rejected
- 如果Promise返回了成功的信息，会执行成功的回调函数
- 如果发生了错误，Promise会收到一个带有错误信息的错误通知

#### Promise的缺点

- 无法取消Promise，一旦新建就会立即执行，无法中途取消
- 其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部
- 第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）