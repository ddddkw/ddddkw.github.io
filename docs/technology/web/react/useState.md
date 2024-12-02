# 记一次使用useState的过程

## 一，什么是useState

useState是react中的一个钩子，它允许你在函数组件中添加状态。当你想要修改 `useState` 返回的状态数据时，你需要使用它提供的更新函数。

在使用过程中遇到的问题就是没有通过更新函数进行更新，而是直接对原始数据进行操作，导致数据发生变化后，页面不进行更新。

## 二，使用步骤

### 1，导入 `useState` 钩子：

```javascript
import React, { useState } from 'react';
```

### 2，声明变量和更新函数

```javascript
const [stateVariable, setStateVariable] = useState(initialState);
```

这里 `stateVariable` 是你的状态数据的当前值，`setStateVariable` 是一个函数，用于更新这个值。`initialState` 是状态的初始值。

### 3，使用变量

在组件中，就想使用普通变量一样使用声明的stateVariable即可

### 4，调用更新函数来修改状态

当你想要改变状态时，调用 `setStateVariable` 函数，并传入新的状态值。React 将重新渲染组件，以反映状态的变化。

```javascript
setStateVariable(newValue);
```

你也可以使用一个函数来更新状态，这个函数接收当前状态作为参数，并返回一个新的状态值。这对于基于当前状态计算新状态时非常有用。

```javascript
setStateVariable(prevState => {
  // 返回一个新的状态值
  return prevState + someValue;
});
```

