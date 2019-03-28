# for循环和for in遍历和forEach 3个比较



## 1、**for...in** 

以任意顺序遍历一个对象的可枚举属性。对于每个不同的属性，语句都会被执行。

语法：

```
`for` `(variable ``in` `object) {...}`
```

参数：

- `variable`

  在每次迭代时,将不同的属性名分配给*变量*。

- `object`

  被迭代其枚举属性的对象。

 

#### **for..in 不应该被用来迭代一个下标顺序很重要的 Array .**

数组索引仅是可枚举的整数名，其他方面和别的普通对象属性没有什么区别。for...in 并不能够保证返回的是按一定顺序的索引，但是它会返回所有可枚举属性，包括非整数名称的和继承的。

因为迭代的顺序是依赖于执行环境的，所以数组遍历不一定按次序访问元素。 因此当迭代那些访问次序重要的 arrays 时用整数索引去进行 [`for`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/for) 循环 (或者使用 [`Array.prototype.forEach()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 或 [`for...of`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of) 循环) 。

### 仅迭代自身的属性

如果你只要考虑对象本身的属性，而不是它的原型，那么使用 [`getOwnPropertyNames()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) 或执行  [`hasOwnProperty()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty) 来确定某属性是否是对象本身的属性 (也能使用[`propertyIsEnumerable`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable))。另外，如果你知道外部不存在任何的干扰代码，你可以扩展内置原型与检查方法。

### 例子

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var obj = {a:1, b:2, c:3};
    
for (var prop in obj) {
  console.log("obj." + prop + " = " + obj[prop]);
}

// Output:
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var triangle = {a:1, b:2, c:3};

function ColoredTriangle() {
  this.color = "red";
}

ColoredTriangle.prototype = triangle;

var obj = new ColoredTriangle();

for (var prop in obj) {
  if( obj.hasOwnProperty( prop ) ) {
    console.log("o." + prop + " = " + obj[prop]);
  } 
}

// Output:
// "o.color = red"
```

## 2、for循环

用于创建一个循环,它包含了三个可选的表达式,三个可选的表达式包围在圆括号中并由分号分隔,后面跟随一个语句或一组语句在循环中执行.

语法：

```
for ([initialization]; [condition]; [final-expression])
   statement
```

参数：

`initialization`一个表达式 (包含赋值语句) 或者变量声明。典型地被用于初始化一个计数器。该表达式可以使用var关键字声明新的变量。初始化中的变量不是该循环的局部变量，而是与该循环处在同样的作用域中。该表达式的结果无意义。

`condition`一个条件表达式被用于确定每一次循环是否能被执行。如果该表达式的结果为true， 循环体内的语句将被执行。 这个表达式是可选的。如果被忽略，那么就被认为永远为true。如果计算结果为false，那么执行流程将被跳到for语句结构后面的第一条语句。

```
final-expression`每次循环的最后都要执行的表达式。执行时机是在下一次`condition的计算之前。通常被用于更新或者递增计数器变量。
```

`statement`只要`condition的结果为true就会被执行的语句。` 要在循环体内执行多条语句，使用一个 [block](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/block) 结构 (`{ ... }`) 来包含要执行的语句。没有任何语句要执行，使用一个 [empty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/Empty) 语句 (`;`)。

### 一般使用

以下例子声明了变量i并被初始赋值为0，for语句检查i的值是否小于9，如果小于9，则执行语句块内的语句，并且最后将i的值递增。

```
for (var i = 0; i < 9; i++) {
   console.log(i);
   // more statements
}
```

### 忽略表达式

for语句的所有的表达式都是可选的

举个例子，初始化语句（*initialization*）中的表达式没有被指定：

```
var i = 0;
for (; i < 9; i++) {
    console.log(i);
    // more statements
}
```

就像*initialization，condition*也是可选的。如果你忽略了，为了不要创建了死循环（无限循环），你必须确保循环体内存在可以退出循环的语句，使用break。

```
for (var i = 0;; i++) {
   console.log(i);
   if (i > 3) break;
   // more statements
}
```

你当然可以忽略所有的表达式。同样的，确保使用了 `break` 语句来退出循环并且你还需要修改（递增）一个变量，以确保能够正常执行break语句。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
var i = 0;

for (;;) {
  if (i > 3) break;
  console.log(i);
  i++;
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

### 使用空语句的例子

以下为在[final-expression]部分中循环计算一个节点的偏移位置,它不需要执行一个语句或者一组语句,因此使用了空语句。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
function showOffsetPos (sId) {
  var nLeft = 0, nTop = 0;

  for (var oItNode = document.getElementById(sId); // initialization
       oItNode; // condition
       nLeft += oItNode.offsetLeft, nTop += oItNode.offsetTop, oItNode = oItNode.offsetParent) // final-expression
       /* empty statement */ ;
  
  console.log("Offset position of \"" + sId + "\" element:\n left: " + nLeft + "px;\n top: " + nTop + "px;");
}

// Example call:

showOffsetPos("content");

// Output:
// "Offset position of "content" element:
// left: 0px;
// top: 153px;"
```



## 4、`**forEach()**` 

`**forEach()**` 方法对数组的每个元素执行一次提供的函数。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
let a = ['a', 'b', 'c'];

a.forEach(function(element) {
    console.log(element);
});

// a
// b
// c
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

语法：

```
array.forEach(callback(currentValue, index, array){
    //do something
}, this)

array.forEach(callback[, thisArg])
```

参数：

`callback`为数组中每个元素执行的函数，该函数接收三个参数：

- currentValue(当前值)

  数组中正在处理的当前元素。

- index(索引)

  数组中正在处理的当前元素的索引。

- array

  forEach()方法正在操作的数组。

`thisArg`可选可选参数。当执行回调 函数时`用作``this的`值(参考对象)。

### **描述：**

```
forEach` 方法按升序为数组中含有效值的每一项执行一次`callback` 函数，那些已删除（使用`delete`方法等情况）或者未初始化的项将被跳过（但不包括那些值为 `undefined 的项）（例如在稀疏数组上）。
```

`callback` 函数会被依次传入三个参数：

- **数组当前项的值**
- **数组当前项的索引**
- **数组对象本身**

如果`给forEach传递了thisArg`参数，当调用时，它将被传给`callback` 函数，作为它的this值。否则，将会传入 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined) 作为它的this值。callback函数最终可观察到this值，这取决于 [函数观察到`this`的常用规则](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)。

`forEach` 遍历的范围在第一次调用 `callback` 前就会确定。调用`forEach` 后添加到数组中的项不会被 `callback` 访问到。如果已经存在的值被改变，则传递给 `callback` 的值是 `forEach` 遍历到他们那一刻的值。已删除的项不会被遍历到。如果已访问的元素在迭代时被删除了(例如使用 [`shift()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)) ，之后的元素将被跳过 - 参见下面的示例。

### **例子：**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
function logArrayElements(element, index, array) {
    console.log("a[" + index + "] = " + element);
}

// 注意索引2被跳过了，因为在数组的这个位置没有项
[2, 5, ,9].forEach(logArrayElements);

// a[0] = 2
// a[1] = 5
// a[3] = 9

[2, 5,"" ,9].forEach(logArrayElements);
// a[0] = 2
// a[1] = 5
// a[2] = 
// a[3] = 9

[2, 5, undefined ,9].forEach(logArrayElements);
// a[0] = 2
// a[1] = 5
// a[2] = undefined
// a[3] = 9


let xxx;
// undefined

[2, 5, xxx ,9].forEach(logArrayElements);
// a[0] = 2
// a[1] = 5
// a[2] = undefined
// a[3] = 9
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
//从每个数组中的元素值中更新一个对象的属性：
function Counter() {
    this.sum = 0;
    this.count = 0;
}

Counter.prototype.add = function(array) {
    array.forEach(function(entry) {
        this.sum += entry;
        ++this.count;
    }, this);
    //console.log(this);
};

var obj = new Counter();
obj.add([1, 3, 5, 7]);

obj.count; 
// 4 === (1+1+1+1)
obj.sum;
// 16 === (1+3+5+7)
```

