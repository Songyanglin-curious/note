## for in

白话：循环出来的是对象的key值

## for of

**`for...of`语句**在[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)（包括 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)，[`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)，[`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)，[`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)，[`TypedArray`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)，[arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments) 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

白话：循环出来的是需要的元素

## for each

`**forEach()**` 方法对数组的每个元素执行一次给定的函数。

[语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#语法)

```
arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

[参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#参数)

- `callback`

  为数组中每个元素执行的函数，该函数接收一至三个参数：

  `currentValue`数组中正在处理的当前元素。`index` 可选数组中正在处理的当前元素的索引。`array` 可选`forEach()` 方法正在操作的数组。

- `thisArg` 可选

  可选参数。当执行回调函数 `callback` 时，用作 `this` 的值。

## [MDN关于迭代协议解释](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%AF%E8%BF%AD%E4%BB%A3%E5%8D%8F%E8%AE%AE)

### [关于for循环比较好的解释](https://www.cnblogs.com/sghy/p/7356681.html)

