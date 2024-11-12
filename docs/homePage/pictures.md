# mkdocs图片的用法

在mkdocs中使用图片的时候，需要将其进行大小设置，图片形状切割，位置移动等等操作。直接使用markdown中的图片插入语法无法进行这种操作，而markdown是支持html和css语法的，所以我们可以直接使用相关语法进行图片引入和样式调整。

但是使用过程中会有一个小小的问题：

使用正常的markdown语法插入图片时，浏览器的图片请求地址为：http://127.0.0.1:8000/mysite/assets/goals.png

```
![](assets/goals.png)
```

当使用html语法引入图片时，浏览器的图片请求地址为：http://127.0.0.1:8000/mysite/about/assets/header.png

```
<img src="./assets/header.png"/>
```

对比可以发现，请求路径中，多了一个about，这个路径是nav标签中相应添加的，这里我们就不深究，只谈解决方案。

最初是想通过在github创建一个新的仓库，专门用于存放图片，img中的src就可以直接请求仓库中的图片地址，但是不同用户使用时，对github的访问稳定性真的难以保证

> 这里推荐一个网站-https://www.helloimg.com/，远程图片托管仓库，上传后可以拿到图片存放地址，用户免费一个G的容量，一般也够用了

```
<div style="text-align: center">
    <img src="https://www.helloimg.com/i/2024/11/11/6731ffea847a0.png" style="zoom:25%;border-radius: 50%" />
</div>
```

便可轻松实现图片加载
