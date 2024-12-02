# react中的局部样式

## 一，样式混淆

在react中书写样式的过程中，我都是通过直接import引入css文件的形式实现的。

```javascript
import './index.css'
```

但是这样会导致样式混淆，css文件中使用的所有样式都是全局样式。

## 二，局部样式

使用过vue的应该都比较熟悉，在style标签上加一个scope属性，当前标签内的所有css样式都只在当前组件内生效，react中也可以实现：

对css文件进行重新命名，‘mycomponent.module.css’

```javascript
import styles from './index.module.css'
```

然后需要修改global.d.ts文件

```JavaScript
// src/types.d.ts
declare module '*.module.css' {
    const classes: { [key: string]: string };
    export default classes;
}
```

定义了一个模块，该模块可以导入任何以`.module.css`结尾的文件。它声明了一个对象`classes`，其键是类名（`string`类型），值是相应的CSS类名（也是`string`类型）。然后将这个对象导出为默认导出。在TypeScript中以类型安全的方式使用CSS模块。