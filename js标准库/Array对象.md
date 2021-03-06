# Array 对象

## 构造函数

`Array`是 JavaScript 的原生对象，同事也是一个构造函数，可以用它生成新的数组。

```js
let arr = new Array(2);
arr.length; // 2
arr; // [empty x 2]
```

上面代码中，`Array`构造函数的参数`2`，表示生成一个两个成员的数组，每个位置都是空值。

如果没有使用`new`，运行结果也是一样的。

```js
let arr = new Array(2);
// 等同于
let arr = Array(2);
```

`Array`构造函数有一个很大的缺陷，就是不同的参数，会导致它的行为不一致。

## 静态方法

### Array.isArray()

`Array.isArray`方法返回一个布尔值，表示参数是否为数组。它可以弥补`typeof`运算符的不足。

```js
let arr = [1, 2, 3];

typeof arr; // "object"
Array.isArray(arr); // true
```

上面代码中，`typeof`运算符只能显示数组的类型是`Object`，而`Array.isArray`方法可以识别数组。

### push(), pop()

`push`方法用于在数组的末端添加一个或多个元素，并返回添加元素后的数组长度。注意，该方法会改变原数组。

```js
let arr = [];

arr.push(1); // 1
arr.push("a"); // 2
arr.push(true, {}); // 4
arr; // [1, 'a', true, {}]
```

上面代码使用`push`方法，往数组中添加了四个成员。

`pop`方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组。

```js
let arr = ["a", "b", "c"];

arr.pop(); // 'c'
arr; // ['a', 'b']
```

上面的数组使用`pop`方法，删除了`c`。

对空数组使用`pop`方法，不会报错，而是返回`undefined`

```js
[].pop(); // undefined
```

`push`和`pop`结合使用，就构成了“后进先出”的栈结构（stack）。

```js
let arr = [];

arr.push(1, 2);
arr.push(3);
arr.pop();
arr; // [1, 2]
```

上面代码中，`3`是最后进入数组的，但是最早离开数组。

## shift(), unshift()

`shift()`方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组。

```js
let a = ["a", "b", "c"];

a.shift(); // 此处删除了'a'
a; // ['b', 'c']
```

`shift()`方法可以遍历并清空一个数组

```js
let list = [1, 2, 3, 4];
let item;

while ((item = list.shift())) {
  console.log(item); // 1, 2, 3, 4
}

list; // []
```

上面代码通过`list.shift()`方法每次取出一个元素，从而遍历数组。它的前提是数组元素不能是`0`或任何布尔值等于`false`的元素，因此这样的遍历不是很靠谱。

`push()`和`shift()`结合使用，就构成了“先进先出”的队列结构。

`unshift()`方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```js
let i = ["a", "b", "c"];

i.unshift("x"); // 4
i; // ['x', 'a', 'b', 'c']
```

`unshift()`方法可以接受多个参数，这些参数都会添加到目标数组头部。

```js
let arr = ["c", "d"];
arr.unshift("a", "b"); // 4
arr; // [ 'a', 'b', 'c', 'd']
```

## join()

`join()`方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分割。

```js
let a = [1, 2, 3, 4];

a.join(' ')         // '1 2 3 4'
a.join(‘ | ’)       // "1 | 2 | 3 | 4"
a.join()            // "1,2,3,4"
```

如果数组成员是`undefined`或`null`或空位，会被转成空字符串。

```js
[undefined, null].join('#')     // '#'

['a',, 'b'].join('-')           // 'a--b'
```

通过`call`方法，这个方法也可以用于字符串或类似数组的对象。

```js
Array.prototype.join.call("hello", "-");
// "h-e-l-l-o"

let obj = { 0: "a", 1: "b", length: 2 };
Array.prototype.join.call(obj, "-"); // 'a-b'
```

## concat()

`concat`方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后补，然后返回一个新的数组，原数组不变。

```js
['hello'].concat(['world'])
// ["hello", "world"]
	// 0: "hello"
	// 1: "world"
	// length: 2

['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]

[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]

[2].concat({a: 1})
// [2, {a: 1}]
```

除了数组作为参数，`concat`也接受其他类型的值作为参数，添加到目标数组尾部。

```js
[1, 2, 3].concat(4, 5, 6);
// [1, 2, 3, 4, 5, 6]
```

如果数组成员包括对象，`concat`方法返回但钱数组的一个浅拷贝。所谓“浅拷贝”明智的是新数组拷贝的是对象的引用。

```js
let obj = {a: 1};
let oldArray = [obj];

let new Array = oldArray.concat();

obj.a = 2;
newArray[0].a           // 2
```

上面代码中，原数组包含一个对象，`caocat`方法生成的新数组包含这个对象的引用。所以，改变元对象以后，新数组跟着改变。

## reverse()

`reverse`方法用于颠倒排列数组元素，返回改变后的数组。注意，该方法将改变原数组。

```js
let a = ["a", "b", "c"];

a.revarse(); // ["c", "b", "a"]
a; // ["c", "b", "a"]
```

## slice()

`slice`方法用于提取目标数组的一部分，返回一个新数组，原数组不变。

```js
arr.slice(start, end);
```

它的第一个参数为起始位置(从 0 开始)，第二个参数为终止位置(但该位置的元素本身不包括在内)。如果省略第二个参数，则一直返回到原数组的最后一个成员。

```js
var a = ["a", "b", "c"];

a.slice(0); // ["a", "b", "c"]
a.slice(1); // ["b", "c"]
a.slice(1, 2); // ["b"]
a.slice(2, 6); // ["c"]
a.slice(); // ["a", "b", "c"]
```

最后一个例子`slice`没有参数，实际上等于返回一个原数组的拷贝。

如果`slice`方法的参数是负数，则表示倒数计算的位置。

```js
var a = ["a", "b", "c"];
a.slice(-2); // ["b", "c"]
a.slice(-2, -1); // ["b"]
```

上面代码中，`-2`表示倒数计算的第二个位置，`-1`表示倒数计算的第一个位置。

如果第一个参数大于或等于数组长度，或者第二个参数小于第一个参数，则返回空数组。

`slice`方法的一个重要应用，是将类似数组的对象转为真正的数组。

```js
Array.prototype.slice.call({ 0: "a", 1: "b", length: 2 });
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```

上面代码的参数都不是数组，但是通过`call`方法，在它们上面调用`slice`方法，就可以把它们转为真正的数组。

## splice()

`splice`方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。

```js
arr.splice(start, count, addElement1, addElement2, ...);
```

`splice`的第一个参数是删除的起始位置(从 0 开始)，第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

```js
let a = ["a", "b", "c", "d", "e", "f"];
a.splice(4, 2); // ["e", "f"]
a; // ["a", "b", "c", "d"]
```

上面代码从原数组 4 号位置，删除了两个数组成员。

```js
let a = ["a", "b", "c", "d", "e", "f"];
a.splice(4, 2, 1, 2); // ["e", "f"]
a; // ["a", "b", "c", "d", 1, 2]
```

上面代码除了删除成员，还插入了两个新成员。

起始位置如果是负数，就表示从倒数位置开始删除

```js
let a = ["a", "b", "c", "d", "e", "f"];
a.splice(-4, 2); // ["c", "d"]
a; // ['a', 'b', 'e', 'f']
```

上面代码表示，从倒数第四个位置`c`开始删除两个成员。

如果只是单纯地插入元素，`splice`方法的第二个参数可以设为`0`。

```js
let a = [1, 1, 1];

a.splice(1, 0, 2); // 没有删除元素
a; // [1, 2, 1, 1]
```

如果值提供第一个参数，等同于将原数组在制定位置拆分成两个数组。

```js
let a = [1, 2, 3, 4];
a.splice(2); // [3, 4]
a; // [1, 2]
```

## sort()

`sort`方法对数组成员进行排序，默认是按照字典顺序排序。排序后，原数组将被改变。

```js
["d", "c", "b", "a"]
  .sort()
  [
    // ['a', 'b', 'c', 'd']

    (4, 3, 2, 1)
  ].sort()
  [
    // [1, 2, 3, 4]

    (11, 101)
  ].sort()
  [
    // [101, 11]

    (10111, 1101, 111)
  ].sort();
// [10111, 1101, 111]
```

上面代码的最后两个例子，需要特殊注意。`sort`方法不是按照大小排序，而是按照字典顺序。也就是说，数值会被先转成字符串，在按照字典顺序进行比较，所以`101`排在`11`的前面。

如果想让`sort`方法按照自定义方式排序，可以传入一个函数作为参数。

```js
[10111, 1101, 111].sort(function(a, b) {
  return a - b;
});
// [111, 1101, 10111]
```

上面代码中，`sort`的参数函数本身接受两个参数，表示进行比较的两个数组成员。如果该函数的返回值大于`0`，表示第一个成员排在第二个成员后面；其他情况下，都是第一个元素排在第二个元素前面。

```js
[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28 }
].sort(function(o1, o2) {
  return o1.age - o2.age;
});
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

## map()

`map`方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新的数组返回。

```js
let numbers = [1, 2, 3];

numbers.map(function(n) {
  return n + 1;
});
// [2, 3, 4]

numbers;
// [1, 2, 3]
```

上面代码中，`number`数组的所有成员依次执行参数函数，运行结果组成一个新数组返回，原数组没有变化。

`map`方法接受一个函数作为参数。该函数调用时，`map`方法向它传入三个参数：当前成员、当前位置和数组本身。

```js
[1, 2, 3].map(function(elem, index, arr) {
  return elem * index;
});
// [0, 2, 6]
```

上面代码中，`map`方法的回调函数有三个参数，`elem`为当前成员的值，`index`为当前成员的位置，`arr`为原数组（`[1, 2, 3]`）。

`map`方法还可以接受第二个参数，用来绑定回调函数内部的`this`变量

```js
let arr = ["a", "b", "c"];

[1, 2].map(function(e) {
  return this[e];
}, arr);
// ['b', 'c']
```

上面代码通过`map`方法的第二个参数，将回调函数内部的`this`对象，指向`arr`数组。

如果数组有空位，`map`方法的回调函数在这个位置不会执行， 会跳过数组的空位。

```js
var f = function (n) { return 'a' };

[1, undefined, 2].map(f)    // ["a", "a", "a"]
[1, null, 2].map(f)         // ["a", "a", "a"]
[1, , 2].map(f)             // ["a", , "a"]
```

上面代码中，`map`方法不会跳过`undefined`和`null`，但是会跳过空位。

## forEach()

`forEach`方法与`map`方法很相似，也是对数组的所有成员依次执行参数函数。但是，`forEach`方法不返回值，只用来操作数据。这就是说，如果数组遍历的目的是为了得到返回值，那么使用`map`方法，否则使用`forEach`方法。

`forEach`的用法与`map`方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。

```js
function log(element, index, array) {
  console.log("[" + index + "] = " + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

上面代码中，`forEach`遍历数组不是为了得到返回值，而是为了在屏幕输出内容，所以不必使用`map`方法。

`forEach`方法也可以接受第二个参数，绑定参数函数的`this`变量。

```js
var out = [];

[1, 2, 3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out; // [1, 4, 9] elem是当前值，相乘
```

上面代码中，空数组`out`是`forEach`方法的第二个参数，结果，灰调函数内部的`this`关键字就指向`out`。

注意，`forEach`方法无法中断执行，总是会将所有成员遍历完。如果希望符合某种条件时，就中断遍历，要使用`for`循环。

```js
let arr = [1, 2, 3];

for (let i = 0; i < arr.length; i++) {
  if (arr[i] === 2) break;
  console.log(arr[i]); // 1
}
```

上面代码中，执行到数组的第二个成员时，就会中断执行。`forEach`方法做不到这一点。

`forEach`方法也会跳过数组的空位。

```js
var log = function (n) {
	console.log(n + 1);
};

[1, undefined, 2].forEach(log)
// 2
// NaN
// 3

[1, null, 2].forEach(log)
// 2
// 1
// 3

[1, , 2].forEach(log)
// 2
// 3
```

上面代码中，`forEach`方法不会跳过`undefined`和`null`，但会跳过空位。

## filter()

`filter`方法用于过滤数组成员，满足条件的成员组成一个新数组返回。

它的参数是一个函数，所有数组成员依次执行该函数，返回结果为`true`的成员组成一个新数组返回。该方法不会改变原数组。

```js
[1, 2, 3, 4, 5].filter(function(elem) {
  return elem > 3;
});
// [4, 5]
```

上面代码将大于`3`的数组成员，作为一个新数组返回。

```js
var arr = [0, 1, "a", false];

arr.filter(Boolean);
// [1, "a"]
```

上面代码中，`filter`方法返回数组`arr`里面所有布尔值为`true`的成员。

`filter`方法的参数函数可以接受三个参数：当前成员、当前位置和整个数组。

```js
[1, 2, 3, 4, 5].filter(function(elem, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]
```

上面代码返回偶数位置的成员组成的新数组。

`filter`方法还可以接受第二个参数，用来绑定参数函数内部的`this`变量。

```js
let obj = { MAX: 3 };
let myFilter = function(item) {
  if (item > this.MAX) return true;
};
let arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj); // [8, 4, 9]
```

上面代码中，过滤器`myFilter`内部有`this`变量，它可以被`filter`方法的第二个参数`obj`绑定，返回大于`3`的成员。

## some(), every()

这两个方法类似“断言”(assert)，返回一个布尔值，表示判断数组成员是否符合某种条件。

它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员、当前位置和整个数组，然后返回一个布尔值。

`some`方法是只要一个成员的返回值是`true`，则整个`some`方法的返回值就是`true`，否则返回`false`。

```js
var arr = [1, 2, 3, 4, 5];
arr.some(function(elem, index, arr) {
  return elem >= 3;
});
// true
```

上面代码中，如果数组`arr`有一个成员大于等于 3，`some`方法就返回`true`。

`every`方法是所有成员的返回值都是`true`，整个`every`方法才返回`true`，否则返回`fasle`。

```js
var arr = [1, 2, 3, 4, 5];
arr.every(function(elem, index, arr) {
  return elem >= 3;
});
// false
```

上面代码中，数组`arr`并非所有成员大于等于`3`,所以返回`fasle`。

注意，对于空数组，`some`方法返回`false`，`every`方法返回`true`，回调函数都不会执行。

```js
function isEven(x) { return x % 2 === 0 }

[].some(isEven) // false
[].every(isEven) // true
```

`some`和`every`方法还可以接受第二个参数，用来绑定参数函数内部的`this`变量。

## indexOf(), lastIndexOf()

`indexOf`方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回`-1`。

```js
var a = ["a", "b", "c"];

a.indexOf("b"); // 1
a.indexOf("y"); // -1
```

`indexOf`方法还可以接受第二个参数，表示搜索的开始位置。

```js
["a", "b", "c"].indexOf("a", 1); // -1
```

上面代码从 1 号位置开始搜索字符`a`，结果为`-1`，表示没有搜索到。

`lastIndexOf`方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回`-1`

```js
var a = [2, 5, 9, 2];
a.lastIndexOf(2); // 3
a.lastIndexOf(7); // -1
```

注意，这两个方法不能用来搜索`NaN`的位置，即它们无法确定数组成员是否包含`NaN`。

```js
[NaN]
  .indexOf(NaN) // -1
  [NaN].lastIndexOf(NaN); // -1
```

这是因为这两个方法内部，使用严格相等运算符（`===`）进行比较，而`NaN`是唯一一个不等于自身的值。

## 链式使用

上面这些数组方法之中，有不少返回的还是数组，所以可以链式使用

```js
var users = [
  { name: "tom", email: "tom@example.com" },
  { name: "peter", email: "peter@example.com" }
];

users
  .map(function(user) {
    return user.email;
  })
  .filter(function(email) {
    return /^t/.test(email);
  })
  .forEach(function(email) {
    console.log(email);
  });
// "tom@example.com"
```

上面代码中，先产生一个所有 Email 地址组成的数组，然后再过滤出以`t`开头的 Email 地址，最后将它打印出来。
