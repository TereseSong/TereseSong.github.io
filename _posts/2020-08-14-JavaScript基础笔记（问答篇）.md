---
layout:    post
title:     JavaScript基础笔记（问答篇）
subtitle: 
date:       2020-08-14 23:48:56
author:     terese
header-img: img/post-20.jpg
catalog:   true
tags:
    - 笔记
typora-copy-images-to: upload
---

### JavaScript基础笔记

### 基本语法

1.Javascript引擎的工作方式？

<details>
  JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。
</details>

2.Javascript标识符的命名规则？

<details>
  JavaScript对大小写敏感。
第一个字符，可以是任意 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（`$`）和下划线（`_`）。
第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字`0-9`。
下面这些则是不合法的标识符。
1a  // 第一个字符不能是数字
23  // 同上
***  // 标识符不能包含星号
a+b  // 标识符不能包含加号
-d  // 标识符不能包含减号或连词线
</details>



### 数据类型

1.有几种数据类型？分别是？

<details>
 原始数据类型：字符串（string）、数值（number）、布尔值（Boolean）、undefined、null、symbol（ES6）
复杂数据类型：对象（Object）、数组（array）、函数（function）
如何判断对象是一个数组？
Object.prototype.toString.call(arr) ==='[object Array]'
//true
</details>

2.JavaScript 有几种方法，可以确定一个值到底是什么类型？

<details>
typeof
instanceof
Object.prototype.toString
-   typeof 1            // "number"
    typeof Number(1)    // "number"
    typeof ''           // "string"
    typeof true         // "boolean"
    typeof null         // "object"
    typeof undefined    // "undefined"
    typeof Symbol       // "function"
instanceof可以检查引用类型、obj是所有对象的父类；
typeof除了fuction不能得到对象的具体数据类型；
默认情况下(不覆盖 toString 方法前提下)，任何一个对象调用 Object 原生的 toString 方法都会返回 "[object type]"，其中 type 是对象的类型；
toString 弥补 instanceof 在跨窗口下对类型检测问题, toString 只适应于原生对象;
对于用户自定义的对象无法区分的.
https://segmentfault.com/a/1190000017390709
</details>

3.null`和`undefined的区别？

<details>
http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html
  目前，null和undefined基本是同义的，只有一些细微的差别。
  null表示"没有对象"，即该处不应该有值。典型用法是：
（1） 作为函数的参数，表示该函数的参数不是对象。
（2） 作为对象原型链的终点。
  undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。
</details>

4.转换规则哪些值被转为`false`？

<details>
  除了下面六个值被转为`false`，其他值都视为`true`。注意，空数组（`[]`）和空对象（`{}`）对应的布尔值，都是`true`。
- `undefined`
- `null`
- `false`
- `0`
- `NaN`
- `""`或`''`（空字符串）
</details>

5.小数的运算要注意什么？举一些例子

<details>
  JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）。容易造成混淆的是，某些运算只有整数才能完成，此时 JavaScript 会自动把64位浮点数，转成32位整数，然后再进行运算
0.1 + 0.2 === 0.3
// false
0.3 / 0.1
// 2.9999999999999996
(0.3 - 0.2) === (0.2 - 0.1)
// false</details>



6.JavaScript 可以精确处理多少位的十进制数？

<details>
  大于2的53次方以后，整数运算的结果开始出现错误。所以，大于2的53次方的数值，都无法保持精度。由于2的53次方是一个16位的十进制数值，所以简单的法则就是，JavaScript 对15位的十进制数都可以精确处理。
</details>

7.JavaScript能表示对数值范围？

<details>
  根据标准，64位浮点数的指数部分的长度是11个二进制位，意味着指数部分的最大值是2047（2的11次方减1）。JavaScript 能够表示的数值范围为21024到2-1023（开区间），超出这个范围的数无法表示。
  如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回`Infinity`。
如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“负向溢出”，即 JavaScript 无法表示这么小的数，这时会直接返回0。
JavaScript 提供`Number`对象的`MAX_VALUE`和`MIN_VALUE`属性，返回可以表示的具体的最大值和最小值。
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324
</details>

8.哪两种情况，JavaScript 会自动将数值转为科学计数法表示？

<details>
  以下两种情况，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示。
**（1）小数点前的数字多于21位。**
**（2）小数点后的零多于5个。**
</details>

9.数值的进制表示？

<details>
 - 十进制：没有前导0的数值。
- 八进制：有前缀`0o`或`0O`的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
- 十六进制：有前缀`0x`或`0X`的数值。
- 二进制：有前缀`0b`或`0B`的数值。
默认情况下，JavaScript 内部会自动将八进制、十六进制、二进制转为十进制。
通常来说，有前导0的数值会被视为八进制，但是如果前导0后面有数字`8`和`9`，则该数值被视为十进制。
</details>

10.有哪些特殊数值？

<details>
  正零和负零
JavaScript 内部实际上存在2个`0`：一个是`+0`，一个是`-0`，区别就是64位浮点数表示法的符号位不同。它们是等价的。唯一有区别的场合是，`+0`或`-0`当作分母，返回的值是不相等的。
NaN
`NaN`是 JavaScript 的特殊值，表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合。`0`除以`0`也会得到`NaN`。一些数学函数的运算结果会出现`NaN`。
`NaN`不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于`Number`，使用`typeof`运算符可以看得很清楚。
`NaN`不等于任何值，包括它本身。
数组的`indexOf`方法内部使用的是严格相等运算符，所以该方法对`NaN`不成立。
`NaN`在布尔运算时被当作`false`。
`NaN`与任何数（包括它自己）的运算，得到的都是`NaN`。
**Infinity**
`nfinity`表示“无穷”，用来表示两种场景。一种是一个正的数值太大，或一个负的数值太小，无法表示；另一种是非0数值除以0，得到`Infinity`。
```
// 场景一
Math.pow(2, 1024)
// Infinity
// 场景二
0 / 0 // NaN
1 / 0 // Infinity
```
上面代码中，第一个场景是一个表达式的计算结果太大，超出了能够表示的范围，因此返回`Infinity`。第二个场景是`0`除以`0`会得到`NaN`，而非0数值除以`0`，会返回`Infinity`。
`Infinity`有正负之分，`Infinity`表示正的无穷，`-Infinity`表示负的无穷。
`Infinity`与`NaN`比较，总是返回`false`。
运算法则：
`Infinity`减去或除以`Infinity`，得到`NaN`。
`Infinity`与`null`计算时，`null`会转成0，等同于与`0`的计算。
`Infinity`与`undefined`计算，返回的都是`NaN`。
</details>

11.与数值相关的全局方法？

<details>
  parseInt()：`parseInt`方法用于将字符串转为整数。
 parseFloat()
isFinite()
  ...
</details>

12.怎么输出多行字符串？

<details>
  (function () { /*
line 1
line 2
line 3
*/}).toString().split('\n').slice(1, -1).join('\n')
// "line 1
// line 2
// line 3"
  console.log('1\n2')
</details>

13.JavaScript使用什么字符集？

<details>
  JavaScript 使用 Unicode 字符集。JavaScript 引擎内部，所有字符都用 Unicode 表示。JavaScript 对 UTF-16 的支持是不完整的，由于历史原因，只支持两字节的字符，不支持四字节的字符。
  总结一下，对于码点在`U+10000`到`U+10FFFF`之间的字符，JavaScript 总是认为它们是两个字符（`length`属性为2）。所以处理的时候，必须把这一点考虑在内，也就是说，JavaScript 返回的字符串长度可能是不正确的。
</details>

14.JavaScript 原生提供两个 Base64 相关的方法？

<details>
  JavaScript 原生提供两个 Base64 相关的方法。
- `btoa()`：任意值转为 Base64 编码
- `atob()`：Base64 编码转为原来的值
要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。encodeURIComponent(str)
</details>

15.对象是什么？

<details>
  简单说，对象就是一组“键值对”（key-value）的集合，是一种无序的复合数据集合。
</details>

16.怎么解释 {foo: 123 }?

<details>
  JavaScript 引擎读到上面这行代码，会发现可能有两种含义。第一种可能是，这是一个表达式，表示一个包含`foo`属性的对象；第二种可能是，这是一个语句，表示一个代码区块，里面有一个标签`foo`，指向表达式`123`。
如果要解释为对象，最好在大括号前加上圆括号。因为圆括号的里面，只能是表达式，所以确保大括号只能解释为对象。
</details>

17.对象属性的读取有哪两种方法？

<details>
  读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符。
```
var obj = {
  p: 'Hello World'
};
obj.p // "Hello World"
obj['p']
```
如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理。
</details>

18.`in`运算符的作用？

<details>
  `in`运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回`true`，否则返回`false`。
</details>

19.怎么判断属性是对象自身的还是继承的？

<details>
  可以使用对象的`hasOwnProperty`方法判断一下，是否为对象自身的属性。
```
var obj = {};
if ('toString' in obj) {
  console.log(obj.hasOwnProperty('toString')) // false
}
```
</details>

20.为什么建议不要使用with语句？

<details>
  with语句内无法判断是全局变量还是对象`obj`的一个属性。建议不要使用`with`语句，可以考虑用一个临时变量代替`with`。
```
// 可以写成
var temp = obj1.obj2.obj3;
console.log(temp.p1 + temp.p2);
```
</details>

21.Javascript的三种声明函数的方法？

<details>
（1）function 命令
（2）函数表达式
（3）Function 构造函数
</details>

22.函数名的提升是怎么一回事？

<details>
  avaScript 引擎将函数名视同变量名，所以采用`function`命令声明函数时，整个函数会像变量声明一样，被提升到代码头部。
```
f();
function f() {}//正确
```
如果采用赋值语句定义函数，JavaScript 就会报错。
```
f();
var f = function (){};
// TypeError: undefined is not a function
```
如果同时采用`function`命令和赋值语句声明同一个函数，最后总是采用赋值语句的定义。因为采用`function`命令声明函数在代码头部，后面赋值语句声明同一个函数会覆盖。
</details>

23.简述函数的属性和方法？name、length、toString()?

<details>
  name获取函数名称、length获取函数参数数量、toString()返回函数代码
</details>

24.函数作用域？

<details>
JavaScript的函数作用域是指在函数内声明的所有变量在函数体内始终是可见的，可以在整个函数的范围内使用及复用，也就是说在函数体内变量声明之前就已经可用了(事实上在嵌套的作用域中也可以使用)。所有没有var直接赋值的变量都属于全局变量;
ES5:全局和局部变量;
ES6:块级作用域
</details>

25.函数内部的变量提升？

<details>
  与全局作用域一样，函数作用域内部也会产生“变量提升”现象。`var`命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部。
```
function foo(x) {
  if (x > 100) {
    var tmp = x - 100;
  }
}
// 等同于
function foo(x) {
  var tmp;
  if (x > 100) {
    tmp = x - 100;
  };
}
```
</details>



26.函数执行时所在的作用域，是定义时的作用域，还是调用时所在的作用域？

<details>
  函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。
</details>



27.函数参数是哪些值传递方式是按值传递？哪些值传递方式是传址传递？

<details>
   函数参数如果是原始类型的值（数值、字符串、布尔值），传递方式是传值传递（passes by value）。这意味着，在函数体内修改参数值，不会影响到函数外部。
    如果函数参数是复合类型的值（数组、对象、其他函数），传递方式是传址传递（pass by reference）。也就是说，传入函数的原始值的地址，因此在函数内部修改参数，将会影响到原始值。
</details>

28.什么是闭包？闭包的用处是？

<details>
  A函数返回B函数，B函数引用了A中的参数。B就是闭包。由于B的引用，A在内存中不销毁。
  闭包简单理解成“定义在一个函数内部的函数”。闭包最大的特点，就是它可以“记住”诞生的环境.
闭包的另一个用处，是封装对象的私有属性和私有方法。
</details>

29.让解释器以表达式来处理函数定义的方法？

<details>
  任何让解释器以表达式来处理函数定义的方法，都能产生同样的效果，比如下面三种写法。
```
var i = function(){ return 10; }();
true && function(){ /* code */ }();
0, function(){ /* code */ }();
```
甚至像下面这样写，也是可以的。
```
!function () { /* code */ }();
~function () { /* code */ }();
-function () { /* code */ }();
+function () { /* code */ }();
```
通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是 IIFE 内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。
```
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);
// 写法二
(function () {
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());
```
上面代码中，写法二比写法一更好，因为完全避免了污染全局变量。
</details>

30.eval的作用？使用要注意什么？

<details>
  `eval`命令接受一个字符串作为参数，并将这个字符串当作语句执行。`eval`的本质是在当前作用域之中，注入代码。由于安全风险和不利于 JavaScript 引擎优化执行速度，所以一般不推荐使用。通常情况下，`eval`最常见的场合是解析 JSON 数据的字符串，不过正确的做法应该是使用原生的`JSON.parse`方法。
</details>

31.eval的别名调用，`eval`内部变量的作用域是？

<details>
  JavaScript 的标准规定，凡是使用别名执行`eval`，`eval`内部一律是全局作用域。
</details>

32.数组的本质是？

<details>
  本质上，数组属于一种特殊的对象。`typeof`运算符会返回数组的类型是`object`。
由于数组成员的键名是固定的（默认总是0、1、2...），因此数组不用为每个元素指定键名，而对象的每个成员都必须指定键名。JavaScript 语言规定，对象的键名一律为字符串，所以，数组的键名其实也是字符串。之所以可以用数值读取，是因为非字符串的键名会被转为字符串。
</details>

33.数组键名是字符串还是数值？

<details>
  JavaScript 语言规定，对象的键名一律为字符串。分别用数值和字符串作为键名，结果都能读取数组。
对象有两种读取成员的方法：点结构（`object.key`）和方括号结构（`object[key]`）。但是，对于数值的键名，不能使用点结构。
</details>

34.数组的length属性是可写的吗？length属性的值等于什么？

<details>
  可写。
  `length`属性的值就是等于最大的数字键加1，如果这个数组没有整数键，`length`属性保持为`0`。
  清空数组的一个有效方法，就是将`length`属性设为0。当`length`属性设为大于数组个数时，读取新增的位置都会返回`undefined`。
</details>

35.for...in循环和数组的遍历？分别用`for`循环或`while`循环、`forEach`方法遍历数组？

<details>
  `for...in`不仅会遍历数组所有的数字键，还会遍历非数字键。所以，不推荐使用`for...in`遍历数组。
数组的遍历可以考虑使用`for`循环或`while`循环。
```
var a = [1, 2, 3];
// for循环
for(var i = 0; i < a.length; i++) {
  console.log(a[i]);
}
// while循环
var i = 0;
while (i < a.length) {
  console.log(a[i]);
  i++;
}
var l = a.length;
while (l--) {
  console.log(a[l]);
}
```
```
var colors = ['red', 'green', 'blue'];
colors.forEach(function (color) {
  console.log(color);
});
// red
// green
// blue
```
</details>

36.数组的空位算一位吗？空位读取为？数组的某个位置为空位或undefiend进行遍历后的区别？

<details>
  数组的空位不影响`length`属性。数组的空位读取返回`undefined`。
数组的某个位置是空位，与某个位置是`undefined`，是不一样的。如果是空位，使用数组的`forEach`方法、`for...in`结构、以及`Object.keys`方法进行遍历，空位都会被跳过。
如果某个位置是`undefined`，遍历的时候就不会被跳过。
空位就是数组没有这个元素，所以不会被遍历到，而`undefined`则表示数组有这个元素，值是`undefined`，所以遍历不会跳过。
</details>

37.什么是类似数组的对象？

<details>
  如果一个对象的所有键名都是正整数或零，并且有`length`属性，那么这个对象就很像数组，语法上称为“类似数组的对象”（array-like object）。
“类似数组的对象”的根本特征，就是具有`length`属性。只要有`length`属性，就可以认为这个对象类似于数组。但是有一个问题，这种`length`属性不是动态值，不会随着成员的变化而变化。
```
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};
obj[0] // 'a'
obj[1] // 'b'
obj.length // 3
obj.push('d') // TypeError: obj.push is not a function
```
</details>

38.怎么可以将“类似数组的对象”变成真正的数组？

<details>
  数组的`slice`方法可以将“类似数组的对象”变成真正的数组。
```
var arr = Array.prototype.slice.call(arrayLike);
```
除了转为真正的数组，“类似数组的对象”还有一个办法可以使用数组的方法，就是通过`call()`把数组的方法放到对象上面。
```
function print(value, index) {
  console.log(index + ' : ' + value);
}
Array.prototype.forEach.call(arrayLike, print);
```
上面代码中，`arrayLike`代表一个类似数组的对象，本来是不可以使用数组的`forEach()`方法的，但是通过`call()`，可以把`forEach()`嫁接到`arrayLike`上面调用。
最好还是先将“类似数组的对象”转为真正的数组，然后再直接调用数组的`forEach`方法。
```
var arr = Array.prototype.slice.call('abc');
arr.forEach(function (chr) {
  console.log(chr);
});
// a
// b
// c
```
</details>

### 运算符

1.对象的相加规则？



2.余数运算要注意什么？



3.数值运算符的作用？



4.valueOf方法的作用？



5.不相等运算符规则？



6.严格相等运算符规则？`==`和`===`的区别？



7.不相等运算符的运用规则？



8.哪些值取反后为true？



9.&&的运算规则及注意点？运算符连用规则？



10.||的运算规则及注意点？运算符连用规则？



11.二进制运算符有哪些？



 12.在 JavaScript 内部，数值以什么形式储存？在做位运算时，是以什么形式进行运算？



13.void运算符的作用？



14.逗号运算符的作用？



15.运算符优先级从高到低依次是？



### 语法专题

1.`Number`函数和parseInt的区别



2.`String`函数的转换规则？如果参数是对象，返回什么？



3.`Boolean()`函数的转换规则？



4.哪三种情况JavaScript 会自动转换数据类型？



5.new关键字的执行过程？



6.JavaScript中的this指向问题？



7.var和let的区别？



8.let和const的区别和相似处？



9.JSON 字符串与 JavaScript 对象相互转换？



10.**javascript:void(a=0)** 的含义？



11.简单介绍一下Javascript异步编程？



12.构造函数、实例、原型三者之间的关系？



13.闭包的用途？

<details>
  闭包的用途：
- 可以在函数外部读取函数内部成员
- 让函数内成员始终存活在内存中
</details>



### 运算符回答

1.

对象必须先转换为原始类型的值，再相加。

对象转成原始类型的值，规则如下。

首先，自动调用对象的`valueOf`方法。

```
var obj = { p: 1 };
obj.valueOf() // { p: 1 }
```

一般来说，对象的`valueOf`方法总是返回对象自身，这时再自动调用对象的`toString`方法，将其转为字符串。

```
var obj = { p: 1 };
obj.valueOf().toString() // "[object Object]"
```

2。



3.

数值运算符的作用在于可以将任何值转为数值（与`Number`函数的作用相同）。

```
+true // 1
+[] // 0
+{} // NaN
```

上面代码表示，非数值经过数值运算符以后，都变成了数值（最后一行`NaN`也是数值）。具体的类型转换规则？

4.

valueOf方法将对象转换成原始类型的值



5.

字符串比较：

JavaScript 引擎内部首先比较首字符的 Unicode 码点。如果相等，再比较第二个字符的 Unicode 码点，以此类推。

非字符串比较：

1）原始类型值：

先转换为数值再比较

字符串和布尔值都会先转成数值，再进行比较。

任何值（包括`NaN`本身）与`NaN`使用非相等运算符进行比较，返回的都是`false`。

2）对象：

如果运算子是对象，会转为原始类型的值，再进行比较。

对象转换成原始类型的值，算法是先调用`valueOf`方法；如果返回的还是对象，再接着调用`toString`方法

eg：

```
[2] > [1] // true
// 等同于 [2].valueOf().toString() > [1].valueOf().toString()
// 即 '2' > '1'

[2] > [11] // true
// 等同于 [2].valueOf().toString() > [11].valueOf().toString()
// 即 '2' > '11'

{ x: 2 } >= { x: 1 } // true
// 等同于 { x: 2 }.valueOf().toString() >= { x: 1 }.valueOf().toString()
// 即 '[object Object]' >= '[object Object]'
```



6.

简单说，它们的区别是相等运算符（`==`）比较两个值是否相等，严格相等运算符（`===`）比较它们是否为“同一个值”。如果两个值不是同一类型，严格相等运算符（`===`）直接返回`false`，而相等运算符（`==`）会将它们转换成同一个类型，再用严格相等运算符进行比较。

**（1）不同类型的值**

如果两个值的类型不同，直接返回`false`。

（2）同一类的原始类型值

同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回`true`，值不同就返回`false`。

`NaN`与任何值都不相等（包括自身）。另外，正`0`等于负`0`。

```
NaN === NaN  // false
+0 === -0 // true
```

（3）复合类型值

两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个地址。如果两个变量引用同一个对象，则它们相等。

**（4）undefined 和 null**

`undefined`和`null`只有与自身比较，或者互相比较时，才会返回`true`；与其他类型的值比较时，结果都为`false`。

```
undefined == null // true
```

**（4）相等运算符的缺点**

相等运算符隐藏的类型转换，会带来一些违反直觉的结果。

```
0 == ''             // true
0 == '0'            // true

2 == true           // false
2 == false          // false

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```

上面这些表达式都不同于直觉，很容易出错。因此建议不要使用相等运算符（`==`），最好只使用严格相等运算符（`===`）。

7.

相等运算符有一个对应的“不相等运算符”（`!=`），它的算法就是先求相等运算符的结果，然后返回相反值。

```
1 != '1' // false

// 等同于
!(1 == '1')
```

8.

以下六个值取反后为`true`，其他值都为`false`。

- `undefined`
- `null`
- `false`
- `0`
- `NaN`
- 空字符串（`''`）

```
!undefined // true
!null // true
!0 // true
!NaN // true
!"" // true

!54 // false
!'hello' // false
![] // false
!{} // false
```

9.

如果第一个运算子的布尔值为`false`，则直接返回第一个运算子的值，且不再对第二个运算子求值。

```
var x = 1;
(1 - 1) && ( x += 1) // 0
x // 1
```

上面代码的最后一个例子，由于且运算符的第一个运算子的布尔值为`false`，则直接返回它的值`0`，而不再对第二个运算子求值，所以变量`x`的值没变。（||运算规则也要注意这一点）

&&运算符可以多个连用，这时返回第一个布尔值为`false`的表达式的值。如果所有表达式的布尔值都为`true`，则返回最后一个表达式的值。

```
true && 'foo' && '' && 4 && 'foo' && true
// ''

1 && 2 && 3
// 3
```

上面代码中，例一里面，第一个布尔值为`false`的表达式为第三个表达式，所以得到一个空字符串。例二里面，所有表达式的布尔值都是`true`，所以返回最后一个表达式的值`3`。

10.

如果第一个运算子的布尔值为`true`，则返回第一个运算子的值，且不再对第二个运算子求值；如果第一个运算子的布尔值为`false`，则返回第二个运算子的值。

或运算符可以多个连用，这时返回第一个布尔值为`true`的表达式的值。如果所有表达式都为`false`，则返回最后一个表达式的值。

或运算符常用于为一个变量设置默认值。

```
function saveText(text) {
  text = text || '';
  // ...
}

// 或者写成
saveText(this.text || '')
```

上面代码表示，如果函数调用时，没有提供参数，则该参数默认设置为空字符串。

11.（跳过后面看）

二进制位运算符用于直接对二进制位进行计算，一共有7个。

- **二进制或运算符**（or）：符号为`|`，表示若两个二进制位都为`0`，则结果为`0`，否则为`1`。
- **二进制与运算符**（and）：符号为`&`，表示若两个二进制位都为1，则结果为1，否则为0。
- **二进制否运算符**（not）：符号为`~`，表示对一个二进制位取反。
- **异或运算符**（xor）：符号为`^`，表示若两个二进制位不相同，则结果为1，否则为0。
- **左移运算符**（left shift）：符号为`<<`。
- **右移运算符**（right shift）：符号为`>>`。
- **头部补零的右移运算符**（zero filled right shift）：符号为`>>>`。

这些位运算符直接处理每一个比特位（bit），所以是非常底层的运算，好处是速度极快，缺点是很不直观，许多场合不能使用它们，否则会使代码难以理解和查错。

12.

虽然在 JavaScript 内部，数值都是以64位浮点数的形式储存，但是做位运算的时候，是以32位带符号的整数进行运算的，并且返回值也是一个32位带符号的整数。

13.

`void`运算符的作用是执行一个表达式，然后不返回任何值，或者说返回`undefined`。

eg：

```
<a href="javascript: void(f())">文字</a>
```

会运行f()函数，但是不返回任何值。

下面是一个更实际的例子，用户点击链接提交表单，但是不产生页面跳转。

```
<a href="javascript: void(document.form.submit())">
  提交
</a>
```

14.

逗号运算符用于对两个表达式求值，并返回后一个表达式的值。前一个表达式是辅助操作。

```
var value = (console.log('Hi!'), true);
// Hi!

value // true
```

15.

小于等于（`<=`)、严格相等（`===`）、或（`||`）、三元（`?:`）、等号（`=`）。

圆括号（`()`）可以用来提高运算的优先级，因为它的优先级是最高的，即圆括号中的表达式会第一个运算。圆括号只改变优先级，不会求值。

16.



### 语法专题回答

1.

`Number`函数将字符串转为数值，要比`parseInt`函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为`NaN`。`parseInt`逐个解析字符，而`Number`函数整体转换字符串的类型。

另外，`parseInt`和`Number`函数都会自动过滤一个字符串前导和后缀的空格。



2.

`String`函数可以将任意类型的值转化成字符串

`String`方法的参数如果是对象，返回一个类型字符串；如果是数组，返回该数组的字符串形式。



3.

`Boolean()`函数可以将任意类型的值转为布尔值。

它的转换规则相对简单：除了以下五个值的转换结果为`false`，其他的值全部为`true`。

- `undefined`
- `null`
- `0`（包含`-0`和`+0`）
- `NaN`
- `''`（空字符串）

所有对象（包括空对象）的转换结果都是`true`，甚至连`false`对应的布尔对象`new Boolean(false)`也是`true`



4.

遇到以下三种情况时，JavaScript 会自动转换数据类型，即转换是自动完成的，用户不可见。

第一种情况，不同类型的数据互相运算。

```
123 + 'abc' // "123abc"
```

第二种情况，对非布尔值类型的数据求布尔值。

```
if ('abc') {
  console.log('hello')
}  // "hello"
```

第三种情况，对非数值类型的值使用一元运算符（即`+`和`-`）。

```
+ {foo: 'bar'} // NaN
- [1, 2, 3] // NaN
```

自动转换的规则是这样的：预期什么类型的值，就调用该类型的转换函数。比如，某个位置预期为字符串，就调用`String()`函数进行转换。如果该位置既可以是字符串，也可能是数值，那么默认转为数值。

除了加法运算符（`+`）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。



5.

new会在内存中创建一个新的空对象
new 会让this指向这个新的对象
执行构造函数  目的：给这个新对象加属性和方法
new会返回这个新对象



6.

![image-20210116202422733](/Users/terese/Library/Application Support/typora-user-images/image-20210116202422733.png)



7.var除了在函数里，都是全局的。

let在块内是局部的，其他是全局的。

在 JavaScript 中, 全局作用域是针对 JavaScript 环境。

在 HTML 中, 全局作用域是针对 window 对象。

使用 **var** 关键字声明的全局作用域变量属于 window 对象:

使用 **let** 关键字声明的全局作用域变量不属于 window 对象:

在相同的作用域或块级作用域中，不能使用 **let** 关键字来重置 **var** 关键字声明的变量:

在相同的作用域或块级作用域中，不能使用 **let** 关键字来重置 **let** 关键字声明的变量:

在相同的作用域或块级作用域中，不能使用 **var** 关键字来重置 **let** 关键字声明的变量:

let 关键字定义的变量则不可以在使用后声明，也就是变量需要先声明再使用。



8.

const **是块级作用域**，用于声明一个或多个常量，声明时必须进行初始化，且初始化后值不可再修改。可以修改常量对象、数组，但是不能对常量对象重新赋值；**const** 关键字在不同作用域，或不同块级作用域中是可以重新声明赋值的:

```
var x = 10;
// 这里输出 x 为 10
{ 
    const x = 2;
    // 这里输出 x 为 2
}
// 这里输出 x 为 10
```

`const`定义常量与使用`let` 定义的变量相似：

- 二者都是块级作用域
- 都不能和它所在作用域内的其他变量或函数拥有相同的名称
- 都不能在同一块级重新声明；都能在不同块级重新声明；
- 都需要先声明后使用；

两者还有以下两点区别：

- `const`声明的常量必须初始化，而`let`声明的变量不用
- const 定义常量的值不能通过再赋值修改，也不能再次声明。而 let 定义的变量值可以修改。



9.

| [JSON.parse()](https://www.runoob.com/js/javascript-json-parse.html) | 用于将一个 JSON 字符串转换为 JavaScript 对象。 |
| ------------------------------------------------------------ | ---------------------------------------------- |
| [JSON.stringify()](https://www.runoob.com/js/javascript-json-stringify.html) | 用于将 JavaScript 值转换为 JSON 字符串。       |



10.

void()仅仅是代表不返回任何值，但是括号内的表达式还是要运行



11.

function print() {    document.getElementById("demo").innerHTML="RUNOOB!"; } setTimeout(print, 3000);

这里setTimeout 就是一个消耗时间较长（3 秒）的过程，它的第一个参数是个回调函数，第二个参数是毫秒数，setTimeout 这个函数执行之后会产生一个子线程，子线程会等待 3 秒，因为有这个子线程所以后面才执行回调函数 "print"。

 setTimeout 会在子线程中等待 3 秒，在 setTimeout 函数执行之后主线程并没有停止

```
setTimeout(function () {
    console.log("1");
}, 1000);
console.log("2");
```

12.

![image-20210119210546302](/Users/terese/Library/Application Support/typora-user-images/image-20210119210546302.png)

每个构造函数都有一个prototype属性，该属性是一个对象。这个对象的所有属性和方法，都会被构造函数所拥有。通过构造函数得到的实例对象内部会包含一个指向构造函数的 `prototype` 对象的指针 `__proto__`。

这也就意味着，我们可以把所有对象实例需要共享的属性和方法直接定义在 `prototype` 对象上。

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
}

console.log(Person.prototype)

Person.prototype.type = 'human'

Person.prototype.sayName = function () {
  console.log(this.name)
}

var p1 = new Person(...)
var p2 = new Person(...)

console.log(p1.sayName === p2.sayName) // => true
```

这时所有实例的 `type` 属性和 `sayName()` 方法，
其实都是同一个内存地址，指向 `prototype` 对象，因此就提高了运行效率。

总结：

- 任何函数都具有一个 `prototype` 属性，该属性是一个对象
- 构造函数的 `prototype` 对象默认都有一个 `constructor` 属性，指向 `prototype` 对象所在函数
- 通过构造函数得到的实例对象内部会包含一个指向构造函数的 `prototype` 对象的指针 `__proto__`
- 所有实例都直接或间接继承了原型对象的成员



13.

闭包的用途：

- 可以在函数外部读取函数内部成员
- 让函数内成员始终存活在内存中



以上笔记回答参考教程：

[JavaScript教程](https://wangdoc.com/javascript/types/number.html)

