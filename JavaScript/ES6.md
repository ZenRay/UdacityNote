# 目录
1. [ES6变化——关键字](#1.ES6关键字变化)

 * [1.1 let和const](#1.1let和const)
 * [1.2 模板字面量](#1.2模板字面量)
 * [1.3 解构](#1.3解构)
 * [1.4 迭代_iteration](#1.4迭代)
 * [1.5 展开运算符](#1.4展开运算符)

2. [ES6变化——函数](#2.ES6函数变化)

 * [2.1 箭头函数](#2.1箭头函数)
 * [2.2 this](#2.2this方法说明)

## 1.ES6关键字变化

### 1.1let 和 const
变量声明增加了let和const，两者的声明后的变量的作用域都是到**块**。而var的声明分为**本地作用域**和**函数作用域**。

如果在代码块（用花括号 { } 表示）中使用 let 或 const 声明变量，那么该变量会陷入暂时性死区，直到该变量的声明被处理。这种行为会阻止变量被访问，除非它们被声明了。

```
function getClothing(isCold) {
  if (isCold) {
    const freezing = 'Grab a jacket!';
  } else {
    const hot = 'It\'s a shorts kind of day.';
    console.log(freezing);
  }
}
getClothing(false);
//返回结果
VM2864:6 Uncaught ReferenceError: freezing is not defined
    at getClothing (<anonymous>:6:17)
    at <anonymous>:1:1
```

因为 const 是声明变量最严格的方式，我们建议始终使用 const 声明变量，因为这样代码更容易读懂，你知道标识符在程序的整个生命周期内都不会改变。如果你发现你需要更新变量或更改变量，则回去将其从 const 切换成 let。**关于使用 let 和 const 的规则：**

* 使用 let 声明的变量可以重新赋值，但是不能在同一作用域内重新声明
* 使用 const 声明的变量必须赋初始值，但是不能在同一作用域内重新声明，也无法重新赋值
* 当你打算为变量重新赋值时，使用 let
* 当你不打算为变量重新赋值时，使用 const
* 在某些情况下有必要使用 var，例如如果你想全局定义变量，但是这种做法通常都不合理，应该避免

### 1.2模板字面量
**模板字面量(template literal)**本质上是包含嵌入式表达式的字符串字面量，是作为字符串运算符(+)和字符串concat方法的替代方式。
模板字面量用**倒引号 (``)**(***而不是单引号 ( '' ) 或双引号( "" )）***表示，可以包含用\$\{expression\} 表示的占位符。这样更容易构建字符串。使用方法如下：

```
const teacher = {
  name: 'Mrs. Wilson',
  room: 'N231'
}
//使用字符串运算方式
let message = student.name + ' please see ' + teacher.name + ' in ' + teacher.room + ' to pick up your report card.';
//使用模板字面量方式
let message = `${student.name} please see ${teacher.name} in ${teacher.room} to pick up your report card.`;
```
对于多行字符串的处理方式，是去掉了引号和字符串连接运算符，以及换行符(\n)——直接在需要换行的地方直接使用enter换行。这是因为模板字面量也将换行符当做字符串的一部分！具体操作方法如下：

```
const student = {
  name: 'Richard Kalehoff',
  guardian: 'Mr. Kalehoff'
};

const teacher = {
  name: 'Mrs. Wilson',
  room: 'N231'
};
//使用字符串算法方式
let note = teacher.name + ',\n\n' +
  'Please excuse ' + student.name + '.\n' +
  'He is recovering from the flu.\n\n' +
  'Thank you,\n' +
  student.guardian;
  
//使用模板字面量方式
let note = `${teacher.name},

Please excuse ${student.name}.
He is recovering from the flu.

Thank you,
${student.guardian};`
```
> **提示：**模板字面量中的嵌入式表达式不仅仅可以用来引用变量。你可以在嵌入式表达式中进行运算、调用函数和使用循环！

模板字面量可以使用在对象以达到少写代码的方法，同时对象的方法也可以简化表达。具体使用方式如下：

```
let type = 'quartz';
let color = 'rose';
let carat = 21.29;
//完全使用名称表示对象value
const gemstone = {
  type: type,
  color: color,
  carat: carat
};
//使用模板字面量的方法来表示对象，并且在calculateWorth方法也相应的简写了
let gemstone = {
  type,
  color,
  carat,
  calculateWorth() { ... }
};
```

### 1.3解构
**解构(Destructing)**这一概念从 Perl 和 Python 等语言中获得灵感，使你能够指定要从赋值左侧上的数组或对象中提取的元素。听起来有点奇怪，实际上可以获得和之前一样的结果，但是用到的代码确更少。解构使用方式如下：

```{数组解构}
//数组解构
const point = [10, 25, -34];

const [x, y, z] = point;	//解构

console.log(x, y, z);	//输出

const [x, , z] = point; //忽略数组中的某个index值
```
> 在此示例中，方括号 [ ] 表示被解构的数组，x、y 和 z 表示要将数组中的值存储在其中的变量。注意，不需要指定要从中提取值的索引，因为索引可以暗示出来——根据原始变量对应的index来确认。

如果使用解构，不仅可以对数组解构，还可以对**对象**中的key\-value进行解构。方式如下：

```
const gemstone = {
  type: 'quartz',
  color: 'rose',
  karat: 21.29
};
//完全解构
const {type, color, karat} = gemstone;
//针对某个指定的key进行解构
let {color} = gemstone; 

```
> 在此示例中，花括号 { } 表示被解构的对象，type、color 和 karat 表示要将对象中的属性存储到其中的变量。注意不用指定要从其中提取值的属性。因为 gemstone 具有 type 属性，值自动存储在 type 变量中。类似地，gemstone 具有 color 属性，因此 color 的值自动存储在 color 变量中。karat 也一样。

> ***指定在解构对象时要选择的值***。例如，let {color} = gemstone; 将仅选择 gemstone 对象中的 color 属性。


### 1.4迭代
ES6增加了一个iteration protocol，利用for...of的循环来解决迭代问题——而不需要通过索引index来遍历。另外for...of不同于for...in——for...in迭代主要针对对象类型数据，因为in是key值；数组索引只是具有整数名称的枚举属性，并且与通用对象属性相同。不能保证for ... in将以任何特定的顺序返回索引。for ... in循环语句将返回所有可枚举属性，包括非整数类型的名称和继承的那些，虽然for...in在对数组使用的时候还是对应的index来遍历。

```
//for in使用
var obj = {a:1, b:2, c:3};
    
for (var prop in obj) {
  console.log("obj." + prop + " = " + obj[prop]);
}

// Output:
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```

**for循环**很明显是最常见的循环类型，**最大缺点**是需要**跟踪计数器**和**退出条件**。

```
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (let i = 0; i < digits.length; i++) {
  console.log(digits[i]);
}
```
> 使用变量 i 作为计数器来跟踪循环并访问数组中的值,使用 digits.length 来判断循环的退出条件。for 循环在循环数组时的确具有优势，但是某些数据结构不是数组，因此并非始终适合使用 loop 循环。

**for...in循环**改善了 for 循环的不足之处，它**消除了计数器逻辑和退出条件**。但是依然需要使用 index 来访问数组的值。此外，当需要向数组中添加额外的方法（或另一个对象）时，for...in 循环会带来很大的麻烦。因为 for...in 循环循环访问所有可枚举的属性，意味着如果向数组的原型中添加任何其他属性，这些属性也会出现在循环中。结果如下：

```
Array.prototype.decimalfy = function() {
  for (let i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(2);
  }
};

const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const index in digits) {
  console.log(digits[index]);
}

//output

0
1
2
3
4
5
6
7
8
9
function() {
 for (let i = 0; i < this.length; i++) {
  this[i] = this[i].toFixed(2);
 }
}
```

 数组中存在一个特殊方法，**forEach循环**是另一种形式的 JavaScript 循环。但是，forEach() 实际上是数组方法，因此只能用在数组中。也无法停止或退出 forEach 循环。如果希望你的循环中出现这种行为，则需要使用基本的 for 循环。

**for...of循环**用于循环访问任何可迭代的数据类型，可以忽略索引；其他**优势**，可以随时停止或退出 for...of循环；不用担心向对象中添加新的属性。for...of 循环将只循环访问对象中的值。使用方法：

```
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  console.log(digit);
}

//output
0
1
2
3
4
5
6
7
8
9

//使用continue语句跳过循环
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  if (digit % 2 === 0) {
    continue;
  }
  console.log(digit);
}
//数组中添加prototype的方法，不会出现在输出中
Array.prototype.decimalfy = function() {
  for (i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(2);
  }
};

const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const digit of digits) {
  console.log(digit);
}
//output
0
1
2
3
4
5
6
7
8
9
```
### 1.5展开运算符
展开运算符，能够将[字面量对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators#迭代器)展开，而仅需要三个连续的点好(...)。调用方法如下：

```
const books = ["Don Quixote", "The Hobbit", "Alice in Wonderland", "Tale of Two Cities"];
console.log(...books);

//output
Don Quixote The Hobbit Alice in Wonderland Tale of Two Cities
```

展开运算符的一个用途是结合数组,如果你需要结合多个数组，在有展开运算符之前，必须使用 Array 的 concat() 方法。

```
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];
const produce = fruits.concat(vegetables);
console.log(produce);

//output
 ["apples", "bananas", "pears", "corn", "potatoes", "carrots"]

```
展开运算符是为了避免出现以下这种情况：

```
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];
//错误方式，直接在添加进入数组对象
const produce = [fruits, vegetables];
console.log(produce);
//output
[Array[3], Array[3]]

//使用展开运算符方法
const produce = [...fruits, ...vegetables];

console.log(produce);
//output
[ 'apples', 'bananas', 'pears', 'corn', 'potatoes', 'carrots' ]
```

**展开运算符的其他应用**，可以通过展开运算符将对象压缩进一个对象。方法如下：

```
const order = [20.17, 18.67, 1.50, "cheese", "eggs", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
console.log(total, subtotal, tax, items);

//output
20.17 18.67 1.5 ["cheese", "eggs", "milk", "bread"]
```
>该代码将 order 数组的值分配给单个变量。数组中的前三个值被分配给了 total、subtotal 和 tax，但是需要重点注意的是 items。

因为可以压缩对象，所以可以在函数中使用展开运算符作形参来表示。以下是利用sum函数来展示该方法：

```
function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```
## 2.ES6函数变化

### 2.1箭头函数
箭头函数和普通函数的行为非常相似，但是在语法构成上非常不同。**普通函数**可以是**函数声明或函数表达式**，但是**箭头函数始终是表达式**。实际上，它们的全称是**“箭头函数表达式”**，因此仅在表达式有效时才能使用，所以其使用要求包括：

1. 存储在变量中，
2. 当做参数传递给函数，
3. 存储在对象的属性中。

```
const greet = name => `Hello ${name}!`;
greet('Asser');
//output
Hello Asser!
```

参数列表出现在箭头函数的箭头（即 =>）前面。如果列表中只有一个参数，那么可以像上述示例那样编写代码。但是，如果列表中有**两个或多个参数**，或者有**零个**，则需要将**参数列表放在圆括号内**：

```
// empty parameter list requires parentheses

const sayHi = () => console.log('Hello Udacity Student!');
sayHi();

//ouput
Hello Udacity Student!
// multiple parameters requires parentheses
const orderIceCream = (flavor, cone) => console.log(`Here's your ${flavor} ice cream in a ${cone} cone.`);
orderIceCream('chocolate', 'waffle');

//output

 Here's your chocolate ice cream in a waffle cone.
```

当函数body只有一个表达式时，可以使用**简写主体语法**，如果是多行表达式时就需要使用**常规主题语法**。两者差别——1)简写主体语法，在函数主体周围没有花括号以及自动返回表达式；2)常规主体语法， 它将函数主体放在花括号内以及需要使用 return 语句来返回内容。多行表达式举例如下：

```
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map( name => {
  name = name.toUpperCase();
  return `${name} has ${name.length} characters in their name`;
});
```


以下代码列出一组名字，使用普通函数把其中每个名字转换为大写形式：

```
//初始方式
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(function(name) { 
  return name.toUpperCase();
});
//箭头函数方式
const upperizedNames = ['Farrin', 'Kagure', 'Asser'].map(
  name => name.toUpperCase()
);
```
>向 map() 方法中传入的是箭头函数，而不是普通函数。注意以下代码中箭头函数的箭头(=>)。

将现有函数转换为普通函数的步骤：

1. 删掉关键字 function
2. 删掉圆括号
3. 删掉左右花括号
4. 删掉关键字 return
5. 删掉分号
6. 在参数列表和函数主体之间添加一个箭头（=>）

**箭头函数优势：**

1. 语法简短多了，
2. 更容易编写和阅读的简短单行函数，
3. 使用简写主体语法时，自动返回内容！

**箭头函数劣势：**

1. 箭头函数中的 this 关键字存在限制条件
2. 箭头函数只是表达式，因此没有箭头函数声明

### 2.2this方法说明
参考说明
[this All Makes Sense Now!](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

[this - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)

[清楚理解并掌握 JavasScript 的 'this'](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)
