# UtilsJs 工具函数
#### 1.unary
   创建一个最多接受一个参数的函数，忽略任何其他参数。
```
const unary = fn => val => fn(val);
```
*Examples*

*['6', '8', '10'].map(unary(parseInt)); // [6, 8, 10]*
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

*groupBy([6.1, 4.2, 6.3], Math.floor); // {4: [4.2], 6: [6.1, 6.3]}*

*groupBy(['one', 'two', 'three'], 'length'); // {3: ['one', 'two'], 5: ['three']}*
***

#### 2.quickSort
   使用快速排序算法对数字数组进行排序。
   使用递归。
   使用扩展运算符 ( ...) 克隆原始数组arr。
   如果length数组的 小于2，则返回克隆的数组。
   使用Math.floor()计算轴心元素的索引。
   使用Array.prototype.reduce()和Array.prototype.push()将数组拆分为两个子数组。第一个包含小于或等于的pivot元素，第二个包含大于它的元素。将结果解构为两个数组。
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
