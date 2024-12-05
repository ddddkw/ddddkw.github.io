# js对象模型全解

类与对象就不在此一一介绍了，大家应该都比较熟悉

## JavaScript对象的特征

1. 对象具有唯一标识性
2. 对象具有状态
3. 对象具有行为

对象有唯一的标识性：对象在内存中存储的地址是不一样的，所以比较的结果也是不一样的

```JavaScript
var obj =  {}
var obj1 = {}
console.log(obj===obj2) //false
```

## js中的对对象的抽象能力

```javascript
var obj {
	count:1,
    func: function(){
        
    }
}
Object.defineProperty(obj,"count",{
    get:function(){ // 访问器函数，钩子函数，进行拦截，还有set函数
        return "222"
    }，
    set:function(newVal){
    	console.log(newVal)
	}
})
console.log(obj.count) // 打印结果是222
obj.count = 3 //打印出333
```

