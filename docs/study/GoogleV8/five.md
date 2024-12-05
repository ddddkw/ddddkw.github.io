# 基于toPrimitive函数解析大厂面试

*主要是对V8拆箱装箱机制的理解使用*

第一题：

写代码让a==3  && a==8 && a==5 成立

```javascript
console.log(a==3)// 会进行拆箱转换
console.log(a===3)// 全等的时候不会进行拆箱转换
// 拆箱转化  valueof  toString  {}._proto_  因为拆箱的过程就是对着三步进行操作
var a = {
    reg:/\d/g
    valueOf:function(){ // 使用正则可以同时实现
        return this.reg.exec('385')[0] // this 指向的是a这个对象
    }
}
console.log(a==3 && a==8 && a==5) // true
```

第二题：

```javascript
([][[]]+[])[+!![]]  +  ([]+{})[!+[]+!![]]输出什么？
// 本质上就是由左右两部分组成的，一步一步地进行拆箱转换
// 对左侧
{
  	([][[]]+[])  [+!![]]
    {
        [][[]] + []
        [] [[]] = [][0] = undefined
        [] = ''
        [][[]] + [] = 'undefined'
    }
    {
        [+!![]] // !![] 或者 !!{} 都是true
        [+true]
        [1] // 进行装箱转换
    }  
    ([][[]]+[])  [+!![]]= 'undefined'[1]  = 'n'
}
// 对右侧
{
    ([]+{})[!+[]+!![]]
    {
        ([]+{})
        '[Object,Object]'
        [!+[]+!![]]
        [!+''+true]
        [true+true]
        [2]
    }
    ('[Object,Object]')[2]  = 'b'
}
// 所以最后输出‘nb’

```

