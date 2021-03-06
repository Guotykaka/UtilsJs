# UtilsJs 工具函数
#### 1.unary
   创建一个最多接受一个参数的函数，忽略任何其他参数。
```
const unary = fn => val => fn(val);
```
*Examples*

*['6', '8', '10'].map(unary(parseInt)); 

// [6, 8, 10]*
***
#### 2.groupBy
   根据给定的函数对数组的元素进行分组。
   
   用于Array.prototype.map()将数组的值映射到函数或属性名称。
   
   使用Array.prototype.reduce()创建一个对象，其中，所述密钥是从映射结果产生的。
   
```
const groupBy = (arr, fn) =>
  arr
    .map(typeof fn === 'function' ? fn : val => val[fn])
    .reduce((acc, val, i) => {
      acc[val] = (acc[val] || []).concat(arr[i]);
      return acc;
    }, {});
```
*Examples*

*groupBy([6.1, 4.2, 6.3], Math.floor);

// {4: [4.2], 6: [6.1, 6.3]}*

*groupBy(['one', 'two', 'three'], 'length');

// {3: ['one', 'two'], 5: ['three']}*
***
#### 3.countBy
   根据给定的函数对数组的元素进行分组，并返回每组中元素的计数。  
   
   用于Array.prototype.map()将数组的值映射到函数或属性名称。  
   
   使用Array.prototype.reduce()创建一个对象，其中，所述密钥是从映射结果产生的。  
```
const countBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => {
    acc[val] = (acc[val] || 0) + 1;
    return acc;
  }, {});
```
*Examples*

*countBy([6.1, 4.2, 6.3], Math.floor); 

// {4: 1, 6: 2}*

*countBy(['one', 'two', 'three'], 'length'); 

// {3: 2, 5: 1}*

*countBy([{ count: 5 }, { count: 10 }, { count: 5 }], x => x.count)

// {5: 2, 10: 1}*
***

#### 4.quickSort
   使用快速排序算法对数字数组进行排序。
   
   使用递归。
   
   使用扩展运算符 ( ...) 克隆原始数组arr。
   
   如果length数组的 小于2，则返回克隆的数组。
   
   使用Math.floor()计算轴心元素的索引。
   
   使用Array.prototype.reduce()和Array.prototype.push()将数组拆分为两个子数组。第一个包含小于或等于的pivot元素，第二个包含大于它的元素。将结果解构为两个数组
   。
   递归调用quickSort()创建的子数组。
```
const quickSort = arr => {
  const a = [...arr];
  if (a.length < 2) return a;
  const pivotIndex = Math.floor(arr.length / 2);
  const pivot = a[pivotIndex];
  const [lo, hi] = a.reduce(
    (acc, val, i) => {
      if (val < pivot || (val === pivot && i != pivotIndex)) {
        acc[0].push(val);
      } else if (val > pivot) {
        acc[1].push(val);
      }
      return acc;
    },
    [[], []]
  );
  return [...quickSort(lo), pivot, ...quickSort(hi)];
};
```
*Examples*

*quickSort([1, 6, 1, 5, 3, 2, 1, 4]); // [1, 1, 1, 2, 3, 4, 5, 6]*
***
#### 5.partition
   将元素分组为两个数组，具体取决于为每个元素提供的函数的真实性。
   
   使用Array.prototype.reduce()以创建两个数组的数组。
   
   使用Array.prototype.push()针对其元素添加fn返回true到所述第一阵列和用于该元件fn返回false到第二个。ay.prototype.push()将数组拆分为两个子数组。第一个包含小于或等于的pivot元素，第二个包含大于它的元素。将结果解构为两个数组。
   
   递归调用quickSort()创建的子数组。
```
const partition = (arr, fn) =>
  arr.reduce(
    (acc, val, i, arr) => {
      acc[fn(val, i, arr) ? 0 : 1].push(val);
      return acc;
    },
    [[], []]
  );
```
*Examples*

*const users = [

  { user: 'barney', age: 36, active: false },
  
  { user: 'fred', age: 40, active: true },
  
];

partition(users, o => o.active);

// [

//   [{ user: 'fred', age: 40, active: true }],

//   [{ user: 'barney', age: 36, active: false }]

// ]*
***
#### 6.differenceBy
   在将提供的函数应用于两个数组的每个数组元素后，返回两个数组之间的差异。
   
   Set通过应用fn到 中的每个元素来创建b。
   
   使用Array.prototype.map()适用fn于每个元素a。
   
   用Array.prototype.filter()结合fn上a只养不包含在值b，使用Set.prototype.has()。
```
const differenceBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.map(fn).filter(el => !s.has(el));
};
```
*Examples*

*differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [1]*

*differenceBy([{ x: 2 }, { x: 1 }], [{ x: 1 }], v => v.x); // [2]*
***
***
#### 7.symmetricDifferenceBy
   在将提供的函数应用于两个数组的每个数组元素后，返回两个数组之间的对称差异。
   
   new Set()从每个数组创建一个以在应用fn到每个数组后获取每个数组的唯一值。
   
   Array.prototype.filter()对它们中的每一个使用以仅保留不包含在另一个中的值。
```
const symmetricDifferenceBy = (a, b, fn) => {
  const sA = new Set(a.map(v => fn(v))),
    sB = new Set(b.map(v => fn(v)));
  return [...a.filter(x => !sB.has(fn(x))), ...b.filter(x => !sA.has(fn(x)))];
};
```
*Examples*

*symmetricDifferenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [ 1.2, 3.4 ]*

*symmetricDifferenceBy(

  [{ id: 1 }, { id: 2 }, { id: 3 }],
  
  [{ id: 1 }, { id: 2 }, { id: 4 }],
  
  i => i.id
  
);

// [{ id: 3 }, { id: 4 }]*
***

