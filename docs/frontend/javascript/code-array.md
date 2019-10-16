---
title: 30s code array
---

> [30 seconds of code (Array)](https://www.30secondsofcode.org/tag/array)



## all

**FUNCTION：**

```js
const all = (arr, fn = Boolean) => arr.every(fn);
```

**CONCEPTS：**   
判断数组中所有元素是否均符合规则。简单封装了 Array.prototype.every 方法。

**EXAMPLES：**

```js
all([4, 2, 3], x => x > 1); // true
all([1, 2, 3]); // true
```



## allEqual

**FUNCTION：**

```js
const allEqual = arr => arr.every(val => val === arr[0]);
```

**CONCEPTS：** 

判断数组中的所有元素是否相等。将数组中所有参数与第一个数据作对比。

**EXAMPLES：**

```js
allEqual([1, 2, 3, 4, 5, 6]); // false
allEqual([1, 1, 1, 1]); // true
```



## any

**FUNCTION：**

```js
const any = (arr, fn = Boolean) => arr.some(fn);
```

**CONCEPTS：** 

判断数组中至少有一个元素符合规则。简单封装了 Array.prototype.some 方法。

**EXAMPLES：**

```js
any([0, 1, 2, 0], x => x >= 2); // true
any([0, 0, 1, 0]); // true
```



## arrayToCSV

**FUNCTION：**

```js
const arrayToCSV = (arr, delimiter = ',') =>
  arr
    .map(v => v.map(x => (isNaN(x) ? `"${x.replace(/"/g, '""')}"` : x)).join(delimiter))
    .join('\n');
```

**CONCEPTS：** 

将二维数组转为 CSV 格式。对二维数组做了两次 map 映射，内层映射确定分隔符，外层映射添加换行符，针对字符串中双引号作处理。

**EXAMPLES：**

```js
arrayToCSV([['a', 'b'], ['c', 'd']]); // '"a","b"\n"c","d"'
arrayToCSV([['a', 'b'], ['c', 'd']], ';'); // '"a";"b"\n"c";"d"'
arrayToCSV([['a', '"b" great'], ['c', 3.1415]]); // '"a","""b"" great"\n"c",3.1415'
```



## bifurcate

**FUNCTION：**

```js
const bifurcate = (arr, filter) =>
  arr.reduce((acc, cur, i) => (acc[filter[i] ? 0 : 1].push(cur), acc), [[], []]);
```

**CONCEPTS：** 

对数组中的数据进行分类。filter 具有下标，可判断为数组，再配合三元运算符进行分类，核心是利用 reduce 和 `,` 对返回结果进行累积，`,` 操作符总是对每个操作求值，并返回最后一个（push 方法会返回数组的长度，不符合要求）。

**EXAMPLES：**

```js
bifurcate(['beep', 'boop', 'foo', 'bar'], [true, true, false, true]); // [ ['beep', 'boop', 'bar'], ['foo'] ]
```



## bifurcateBy

**FUNCTION：**

```js
const bifurcateBy = (arr, fn) =>
  arr.reduce((acc, cur, i) => (acc[fn(cur, i) ? 0 : 1].push(cur), acc), [[], []]);
```

**CONCEPTS：** 

对数组中的数据进行分类。相对于 bifurcate，提供了过滤用函数。

**EXAMPLES：**

```js
bifurcateBy(['beep', 'boop', 'foo', 'bar'], x => x[0] === 'b'); // [ ['beep', 'boop', 'bar'], ['foo'] ]
// 感觉上面可以优化下，速度不一定快，但是语义更好了
bifurcateBy(['beep', 'boop', 'foo', 'bar'], x => x.startsWith('b')); // [ ['beep', 'boop', 'bar'], ['foo'] ]
```



## chunk

**FUNCTION：**

```js
const chunk = (arr, size) =>
  Array.from({ length: Math.ceil(arr.length / size) }, (v, i) =>
    arr.slice(i * size, i * size + size)
  );
```

**CONCEPTS：** 

对数组进行切片（分块）。利用 Array.from 的第二个参数 map 函数对每个分块进行映射。

**EXAMPLES：**

```js
chunk([1, 2, 3, 4, 5], 2); // [[1,2],[3,4],[5]]
```



## compact

**FUNCTION：**

```js
const compact = arr => arr.filter(Boolean);
// 个人之前使用，提供 Boolean 更直观
const compactSelf = arr => arr.filter(a => a);
```

**CONCEPTS：** 

移除数组中 “无效” 值。主要是利用 JavaScript 的隐式转换概念。

**EXAMPLES：**

```js
compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34]); // [ 1, 2, 3, 'a', 's', 34 ]
```



## countBy

**FUNCTION：**

```js
const countBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => 
    ((acc[val] = (acc[val] || 0) + 1), acc), {});
```

**CONCEPTS：** 

对数组中的数据进行分组并计数。对原函数进行修改，贯彻 `,` 操作符， typeof 处理了非函数。

**EXAMPLES：**

```js
countBy([6.1, 4.2, 6.3], Math.floor); // {4: 1, 6: 2}
countBy(['one', 'two', 'three'], 'length'); // {3: 2, 5: 1}
```



## countOccurrences

**FUNCTION：**

```js
const countOccurrences = (arr, val) => arr.reduce((a, v) => (v === val ? a + 1 : a), 0);
```

**CONCEPTS：** 

对数组中某个值进行计数。简单的利用了 reduce 方法。

**EXAMPLES：**

```js
countOccurrences([1, 1, 2, 1, 2, 3], 1); // 3
```



## deepFlatten

**FUNCTION：**

```js
const deepFlatten = arr => [].concat(...arr.map(v => (Array.isArray(v) ? deepFlatten(v) : v)));
// MDN 同样提供了该方法
const deepFlatten = arr => arr.reduce((acc, val) => Array.isArray(val) ? acc.concat(deepFlatten(val)) : acc.concat(val), []);
```

**CONCEPTS：** 

展开数组。数组中可能嵌套数组，利用了递归。

**EXAMPLES：**

```js
deepFlatten([1, [2], [[3], 4], 5]); // [1,2,3,4,5]
```



## difference

**FUNCTION：**

```js
const difference = (a, b) => {
  const s = new Set(b);
  return a.filter(x => !s.has(x));
};
```

**CONCEPTS：** 

返回两个数组之差（差异），其中存在的问题是，仅暴露了 a 的差异而忽略的 b 的差异，利用该方法找相同倒是完美。核心利用了 Set 结构。

**EXAMPLES：**

```js
difference([1, 2, 3], [1, 2, 4]); // [3]
```



## differenceBy

**FUNCTION：**

```js
const differenceBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.map(fn).filter(el => !s.has(el));
};
```

**CONCEPTS：** 

两个数组格式化后之间的差异。一般数据会有较为复杂的结构，往往需要先处理数据。

**EXAMPLES：**

```js
differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [1]
differenceBy([{ x: 2 }, { x: 1 }], [{ x: 1 }], v => v.x); // [2]
```



## differenceWith

**FUNCTION：**

```js
const differenceWith = (arr, val, comp) => arr.filter(a => val.findIndex(b => comp(a, b)) === -1);
```

**CONCEPTS：** 

两个区间范围的差异。除了复杂的数据结构，还需要监听异常的数据（非范围内）。

**EXAMPLES：**

```js
differenceWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0], (a, b) => Math.round(a) === Math.round(b)); // [1, 1.2]
```



## drop

**FUNCTION：**

```js
const drop = (arr, n = 1) => arr.slice(n);
```

**CONCEPTS：** 

删除数组前侧（左侧）的元素。简单使用了 slice 方法，slice 方法同样支持字符串，需要注意的是表象是删除，其实是提取指定位置的元素。

**EXAMPLES：**

```js
drop([1, 2, 3]); // [2,3]
drop([1, 2, 3], 2); // [3]
drop([1, 2, 3], 42); // []
drop("hello world", 5); // " world"
```



## dropRight

**FUNCTION：**

```js
const dropRight = (arr, n = 1) => arr.slice(0, -n);
```

**CONCEPTS：** 

删除数组后侧（右侧）的元素。

**EXAMPLES：**

```js
dropRight([1, 2, 3]); // [1,2]
dropRight([1, 2, 3], 2); // [1]
dropRight([1, 2, 3], 42); // []
```




## dropWhile

**FUNCTION：**

```js
const dropWhile = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr = arr.slice(1);
  return arr;
};
```

**CONCEPTS：** 

从头开始删除数组中不符合条件的，直至符合。

**EXAMPLES：**

```js
// 示例存在误导嫌疑，修改一下
dropWhile([1, 2, 3, 2, 4], n => n >= 3); // [3,2,4]
```



## dropRightWhile

**FUNCTION：**

```js
const dropRightWhile = (arr, func) => {
  let rightIndex = arr.length;
  while (rightIndex-- && !func(arr[rightIndex]));
  return arr.slice(0, rightIndex + 1);
};
```

**CONCEPTS：** 

从末尾开始删除数组中不符合条件的，直至符合。

**EXAMPLES：**

```js
dropRightWhile([1, 2, 3, 2, 4], n => n < 3); //  [1, 2, 3, 2]
```



## everyNth

**FUNCTION：**

```js
const everyNth = (arr, nth) => arr.filter((e, i) => i % nth === nth - 1);
```

**CONCEPTS：** 

返回数组中的第 nth 组成的数组。

**EXAMPLES：**

```js
everyNth([1, 2, 3, 4, 5, 6], 2); // [ 2, 4, 6 ]
```



## filterFalsy

与 [compact](/frontend/javascript/code-array.html#compact) 一致。



## filterNonUnique

**FUNCTION：**

```js
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
```

**CONCEPTS：** 

移除数组中多次出现的数据。利用双指针来判断是否数据重复，需要注意到的这个方法并非是去重。

**EXAMPLES：**

```js
filterNonUnique([1, 2, 2, 3, 4, 4, 5]); // [1, 3, 5]
```



## filterNonUniqueBy

**FUNCTION：**

```js
const filterNonUniqueBy = (arr, fn) =>
  arr.filter((v, i) => arr.every((x, j) => (i === j) === fn(v, x, i, j)));
```

**CONCEPTS：** 

移除数组中多次出现的某一数据。`(i === j) === fn(v, x, i, j)` 是比较细节的地方，用于排除自身（存在更优解）。

**EXAMPLES：**

```js
filterNonUniqueBy(
  [
    { id: 0, value: 'a' },
    { id: 1, value: 'b' },
    { id: 2, value: 'c' },
    { id: 1, value: 'd' },
    { id: 0, value: 'e' }
  ],
  (a, b) => a.id == b.id
); // [ { id: 2, value: 'c' } ]
```



## findLast

**FUNCTION：**

```js
const findLast = (arr, fn) => arr.filter(fn).pop();
```

**CONCEPTS：** 

返回最后一个符合的数据。filter 会返回一个新的数组，pop 方法不会影响原数组。

**EXAMPLES：**

```js
findLast([1, 2, 3, 4], n => n % 2 === 1); // 3
```



## findLastIndex

**FUNCTION：**

```js
const findLastIndex = (arr, fn) =>
  (arr
    .map((val, i) => [i, val])
    .filter(([i, val]) => fn(val, i, arr))
    .pop() || [-1])[0];
```

**CONCEPTS：** 

返回最后一个符合的数据的索引。利用 map 方法保留索引。

**EXAMPLES：**

```js
findLastIndex([1, 2, 3, 4], n => n % 2 === 1); // 2 (index of the value 3)
findLastIndex([1, 2, 3, 4], n => n === 5); // -1 (default value when not found)
```



## flatten

**FUNCTION：**

```js
const flatten = (arr, depth = 1) =>
  arr.reduce((a, v) => a.concat(depth > 1 && Array.isArray(v) ? flatten(v, depth - 1) : v), []);
```

**CONCEPTS：** 

指定数组扁平化深度。在递归中增加一个判断开关，一个无开关[示例](/frontend/javascript/code-array.html#deepflatten)。

**EXAMPLES：**

```js
flatten([1, [2], 3, 4]); // [1, 2, 3, 4]
flatten([1, [2, [3, [4, 5], 6], 7], 8], 2); // [1, 2, 3, [4, 5], 6, 7, 8]
```



## forEachRight

**FUNCTION：**

```js
const forEachRight = (arr, callback) =>
  arr
    .slice(0)
    .reverse()
    .forEach(callback);
```

**CONCEPTS：** 

forEach 从右执行。reverse 会改变原数组，故用 `slice(0)` 进行了数组的浅拷贝。

**EXAMPLES：**

```js
forEachRight([1, 2, 3, 4], val => console.log(val)); // '4', '3', '2', '1'
```



## groupBy

**FUNCTION：**

```js
const groupBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val, i) => {
    acc[val] = (acc[val] || []).concat(arr[i]);
    return acc;
  }, {});
```

**CONCEPTS：** 

数组分类。可见之前更简洁的示例 [bifurcateBy](/frontend/javascript/code-array.html#bifurcateby)。

**EXAMPLES：**

```js
groupBy([6.1, 4.2, 6.3], Math.floor); // {4: [4.2], 6: [6.1, 6.3]}
groupBy(['one', 'two', 'three'], 'length'); // {3: ['one', 'two'], 5: ['three']}
```



## indexOfAll

**FUNCTION：**

```js
const indexOfAll = (arr, val) => arr.reduce((acc, el, i) => (el === val ? [...acc, i] : acc), []);

// 借用 findLastIndex 中的方法
const indexOfAllSelf = (arr, fn) =>
  arr
    .map((val, i) => [i, val])
    .filter(([i, val]) => fn(val, i, arr))
    .map([i] => i);
```

**CONCEPTS：** 

返回出现目标的索引的组成的数组。

**EXAMPLES：**

```js
indexOfAll([1, 2, 3, 1, 2, 3], 1); // [0,3]
indexOfAll([1, 2, 3], 4); // []
indexOfAllSelf([1, 2, 3, 1, 2, 3], n => n === 1); // [0,3]
```



## initialize2DArray

**FUNCTION：**

```js
// 修改参数更语义化
const initialize2DArray = (col, row, val = null) =>
  Array.from({ length: row }).map(() => Array.from({ length: col }).fill(val));
```

**CONCEPTS：** 

初始化指定宽高的二维数组。

**EXAMPLES：**

```js
initialize2DArray(2, 2, 0); // [[0,0], [0,0]]
```