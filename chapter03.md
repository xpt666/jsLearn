## 第3章 语言基础
### 3.1 语法
#### 3.1.1 严格模式
    function doSomething() { 
     "use strict"; 
     // 函数体 
    } 
任何支持的 JavaScript 引擎看到它都会切换到严格模式。选择这种语法形式的目的是不破坏 ECMAScript 3 语法。
#### 3.1.2 语句
加分号也有助于在某些情况下提升性能，因为解析器会尝试在合适的
位置补上分号以纠正语法错误。
#### 3.1.3 变量
ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据。

var 在 ECMAScript 的所有版本中都可以使用，而 const 和 let 只能在 ECMAScript 6 及更晚的版本中使用。

### 3.2 变量
#### 3.2.1 var关键字
    var message;
这行代码定义了一个名为 message 的变量，可以用它保存任何类型的值。（不初始化的情况下，变
量会保存一个特殊值 undefined）

1、var声明作用域

var声明的作用域是局部变量，该变量在函数退出时被销毁：

    function test() { 
     var message = "hi"; // 局部变量
    } 
    test(); 
    console.log(message); // 出错！

去掉之前的 var 操作符之后，message 就变成了全局变量。只要调用一次函数 test()，就会定义
这个变量，并且可以在函数外部访问到。

    function test() { 
     message = "hi"; // 全局变量
    } 
    test(); 
    console.log(message); // "hi" 

> 注意：虽然可以通过省略 var 操作符定义全局变量，但不推荐这么做。在局部作用域中定
义的全局变量很难维护，也会造成困惑。这是因为不能一下子断定省略 var 是不是有意而
为之。在严格模式下，如果像这样给未声明的变量赋值，则会导致抛出 ReferenceError。

如果需要定义多个变量，可以在一条语句中用逗号分隔每个变量（及可选的初始化）：
    
    var message = "hi", 
    found = false, 
    age = 29; 

2、var声明提升

使用 var 时，下面的代码不会报错。这是因为使用这个关键字声明的变量会自动提升到函数作用域
顶部：
    function foo() { 
     console.log(age); 
     var age = 26; 
    } 
    foo(); // undefined 

等同于：

    function foo() { 
     var age; 
     console.log(age); 
     age = 26; 
    } 
    foo(); // undefined

这就是所谓的“提升”（hoist），也就是把所有变量声明都拉到函数作用域的顶部。此外，反复多次
使用 var 声明同一个变量也没有问题：

    function foo() { 
     var age = 16; 
     var age = 26; 
     var age = 36; 
     console.log(age); 
    } 
    foo(); // 36 

#### 3.2.2 let声明
let 声明的范围是块作用域， 而 var 声明的范围是函数作用域。

    if (true) { 
     let age = 26; 
     console.log(age); // 26 
    } 
    console.log(age); // ReferenceError: age 没有定义

    if (true) { 
     let age = 26; 
     console.log(age); // 26 
    } 
    console.log(age); // ReferenceError: age 没有定义'

1、 暂时性死区

let 与 var 的另一个重要的区别，就是 let 声明的变量不会在作用域中被提升。

    // name 会被提升
    console.log(name); // undefined 
    var name = 'Matt'; 
    // age 不会被提升
    console.log(age); // ReferenceError：age 没有定义
    let age = 26; 

在 let 声明之前的执行瞬间被称为“暂时性死区”。

2、全局声明

与 var 关键字不同，使用 let 在全局作用域中声明的变量不会成为 window 对象的属性（var 声
明的变量则会）。

    var name = 'Matt'; 
    console.log(window.name); // 'Matt'


    let age = 26; 
    console.log(window.age); // undefined

3、条件声明

4、for循环中的let声明

var定义的for 循环定义的迭代变量会渗透到循环体外部：

    for (var i = 0; i < 5; ++i) { 
     // 循环逻辑 
    } 
    console.log(i); // 5

let 声明的变量仅限于for循环块内部：

    for (let i = 0; i < 5; ++i) { 
     // 循环逻辑
    } 
    console.log(i); // ReferenceError: i 没有定义】

**注意：**

在使用var的时候，最常见的问题是对迭代变量的奇特声明和修改：

    for (var i = 0; i < 5; ++i) { 
     setTimeout(() => console.log(i), 0) 
    } 
    // 你可能以为会输出 0、1、2、3、4 
    // 实际上会输出 5、5、5、5、5

这是因为 setTimeout 函数是异步的，并且会在循环完成后执行。在执行时，变量 i 的值已经是5了，因为这是循环终止的条件。

为了将数字0到4输出到控制台，可以使用闭包来捕获循环的每次迭代时的变量 i 值。例如：

    for (var i = 0; i < 5; ++i) {
      (function(j) {
        setTimeout(() => console.log(j), 0)
      })(i)
    }

在这个版本的代码中，每次循环迭代都会创建一个新的函数作用域，其中包含一个名为 j 的变量，它被初始化为当前的 i 值。然后，setTimeout 函数使用 j 
来输出期望的值到控制台。

在使用 let 声明迭代变量时，JavaScript 引擎在后台会为每个迭代循环声明一个新的迭代变量。
每个 setTimeout 引用的都是不同的变量实例，所以 console.log 输出的是我们期望的值，也就是循
环执行过程中每个迭代变量的值。

    for (let i = 0; i < 5; ++i) { 
     setTimeout(() => console.log(i), 0) 
    } 
    // 会输出 0、1、2、3、4

#### 3.2.3 const声明

const 的行为与 let 基本相同，唯一一个重要的区别是用它声明变量时必须同时初始化变量，且
尝试修改 const 声明的变量会导致运行时错误。

    const age = 26; 
    age = 36; // TypeError: 给常量赋值

    // const 也不允许重复声明
    const name = 'Matt'; 
    const name = 'Nicholas'; // SyntaxError 

    // const 声明的作用域也是块
    const name = 'Matt'; 
    if (true) { 
     const name = 'Nicholas'; 
    } 
    console.log(name); // Matt 

JavaScript 引擎会为 for 循环中的 let 声明分别创建独立的变量实例，虽然 const 变量跟 let 变
量很相似，但是不能用 const 来声明迭代变量（因为迭代变量会自增）：
    
    for (const i = 0; i < 10; ++i) {} // TypeError：给常量赋值

#### 3.3.4 声明风格及最佳实践

1、不使用var

有了let和const，大多数开发者不再需要var了。限制自己只使用let和const，有助于
提升代码质量，因为变量有了明确的作用域、声明位置，以及不变的值。

2、const优先，let次之

使用 const 声明可以让浏览器运行时强制保持变量不变，也可以让静态代码分析工具提前发现不
合法的赋值操作。

因此，很多开发者认为应该优先使用 const 来声明变量，只在提前知道未来会有修改时，再使用 let。这样可以让开发者更有信心地推断某些变量的值永远不会变，同时也能迅速发现因
意外赋值导致的非预期行为。

### 3.4 数据类型

- "undefined"表示值未定义；
- "boolean"表示值为布尔值；
- "string"表示值为字符串；
- "number"表示值为数值；
- "object"表示值为对象（而不是函数）或 null；
- "function"表示值为函数；
- "symbol"表示值为符号。

>
> Infinity（无穷）值
> 
> -Infinity（负无穷大）

要确定一个值是不是有限大（即介于 JavaScript 能表示的
最小值和最大值之间），可以使用 isFinite()函数，如下所示：

    let result = Number.MAX_VALUE + Number.MAX_VALUE; 
    console.log(isFinite(result)); // false 


有一个特殊的数值叫 NaN，意思是“不是数值”（Not a Number），用于表示本来要返回数值的操作
失败了（而不是抛出错误）。比如，用 0 除任意数值在其他语言中通常都会导致错误，从而中止代码执
行。但在 ECMAScript 中，0、+0 或0 相除会返回 NaN：

    console.log(0/0); // NaN 
    console.log(-0/+0); // NaN

有一个特殊的数值叫 NaN，意思是“不是数值”（Not a Number），用于表示本来要返回数值的操作
失败了（而不是抛出错误）。比如，用 0 除任意数值在其他语言中通常都会导致错误，从而中止代码执
行。但在 ECMAScript 中，0、+0 或0 相除会返回 NaN：

    console.log(0/0); // NaN 
    console.log(-0/+0); // NaN

NaN 不等于包括 NaN 在内的任何值。例如，下面的比较操作会返回 false：
    
    console.log(NaN == NaN); // false

ECMAScript 提供了 isNaN()函数。该函数接收一个参数，可以是任意数据类型，然后判断
这个参数是否“不是数值”。

把一个值传给 isNaN()后，该函数会尝试把它转换为数值。某些非数值的
值可以直接转换成数值，如字符串"10"或布尔值。任何不能转换为数值的值都会导致这个函数返回
true。举例如下：

    console.log(isNaN(NaN)); // true 
    console.log(isNaN(10)); // false，10 是数值
    console.log(isNaN("10")); // false，可以转换为数值 10 
    console.log(isNaN("blue")); // true，不可以转换为数值
    console.log(isNaN(true)); // false，可以转换为数值 1 

**数值转换**

有 3 个函数可以将非数值转换为数值：Number()、parseInt()和 parseFloat()。
1、Number()  

- 转型函数，可用于任何数据类型。
- 布尔值：true转换为1，false转换为0
- 数值：直接返回
- undefined：返回NaN
- 字符串
    - Number("1") -> 1
    - Number("1.1") -> 1.1
    - Number("0xf") -> 16
    - Number("") -> 0
    - Number("#@#$") -> NaN
  

- 对象

2、parseInt()

    let num1 = parseInt("1234blue"); // 1234 
    let num2 = parseInt(""); // NaN 
    let num3 = parseInt("0xA"); // 10，解释为十六进制整数
    let num4 = parseInt(22.5); // 22 
    let num5 = parseInt("70"); // 70，解释为十进制值
    let num6 = parseInt("0xf"); // 15，解释为十六进制整数

3、parseFloat()

    let num1 = parseFloat("1234blue"); // 1234，按整数解析
    let num2 = parseFloat("0xA"); // 0 
    let num3 = parseFloat("22.5"); // 22.5 
    let num4 = parseFloat("22.34.5"); // 22.34 
    let num5 = parseFloat("0908.5"); // 908.5 
    let num6 = parseFloat("3.125e7"); // 31250000 

**String类型**

转换为字符串：toString()方法

    let age = 11; 
    let ageAsString = age.toString(); // 字符串"11" 
    let found = true; 
    let foundAsString = found.toString(); // 字符串"true"

toString()方法可见于数值、布尔值、对象和字符串值。（没错，字符串值也有 toString()方法)

null 和 undefined 值没有 toString()方法。

默认情况下，toString()返回数值的十进制字符串表示。而通过传入参数，可以得到数值的二进制、八进制、十六进制，或者其他任何有效基
数的字符串表示，比如：

    let num = 10; 
    console.log(num.toString()); // "10" 
    console.log(num.toString(2)); // "1010" 
    console.log(num.toString(8)); // "12" 
    console.log(num.toString(10)); // "10" 
    console.log(num.toString(16)); // "a" 

String():

    let value1 = 10; 
    let value2 = true; 
    let value3 = null; 
    let value4; 
    console.log(String(value1)); // "10" 
    console.log(String(value2)); // "true" 
    console.log(String(value3)); // "null" 
    console.log(String(value4)); // "undefined" 

**字符串插值**

// 现在，可以用模板字面量这样实现：

    let interpolatedTemplateLiteral = `${ value } to the ${ exponent } power is ${ value * value }`; 
    
    console.log(interpolatedString); // 5 to the second power is 25 
    console.log(interpolatedTemplateLiteral); // 5 to the second power is 25 


将表达式转换为字符串时会调用 toString()：

    let foo = { toString: () => 'World' }; 
    console.log(`Hello, ${ foo }!`); // Hello, World!

### 3.5 操作符

#### 3.5.1 位操作符

1、按位非（~）

举例：

    let num1 = 25; 
    let num2 = -num1 - 1; 
    console.log(num2); // "-26"】

2、按位与（&）

> 全1为1，有0为0

3、按位或（|）

> 全0为0，有1为1

4、按位异或（^）
> 相同为0，不同为1

5、左移（<<）

    let oldValue = 2; // 等于二进制 10 
    let newValue = oldValue << 5; // 等于二进制 1000000，即十进制 64
6、有符号右移（>>）

    let oldValue = 64; // 等于二进制 1000000 
    let newValue = oldValue >> 5; // 等于二进制 10，即十进制 2

7、无符号右移（>>>）

对于正数，无符号右移与 有符号右移结果相同。仍然以前面有符号右移的例子为例，64 向右移动 5 位，会变成 2：
    
    let oldValue = 64; // 等于二进制 1000000 
    let newValue = oldValue >>> 5; // 等于二进制 10，即十进制 2

对于负数，差别较大。无符号右移操作符将附负数的二进制表示当成正数的二进制表示来处理
因为负数是其绝对值的二补数，所以右移后结果变得非常大，举例：

    let oldValue = -64; // 等于二进制 11111111111111111111111111000000 
    let newValue = oldValue >>> 5; // 等于十进制 134217726

#### 3.5.11 逗号操作符（不多见）
    let num = (5, 1, 4, 8, 0); // num 的值为 0 
在这个例子中，num 将被赋值为 0，因为 0 是表达式中最后一项。

### 3.6 语句
#### 3.6.1 for-in语句

    const a = [14,5,2,5,11]
    for (const propName in a) {
        console.log(propName); // 0 1 2 3 4 5
        console.log(a[propName]); // 14 5 2 5 11
    }
#### 3.6.2 for-of语句

    for (const el of [2,4,6,8]) {
        console.log(el); // 2 4 6 8
    }
#### 3.6.3 标签语句
     outerLoop:
    for (var i = 0; i < 10; i++) {
      innerLoop:
      for (var j = 0; j < 10; j++) {
        if (i === 5 && j === 5) {
          break outerLoop;
        }
      }
    }

在这个例子中，我们定义了两个标签 outerLoop 和 innerLoop，然后在嵌套的 for 循环中使用了这些标签。当 i 和 j 的值都等于 5 时，我们使用 
break outerLoop 语句跳出了外部的循环。

#### 3.6.4 with语句
    with (object) {
        // code block
    }
其中，object 是一个 JavaScript 对象，它定义了一个作用域。在 with 语句中，你可以直接访问 object 中的属性和方法，而不需要每次都写出 object. 的前缀。


例如，假设你有一个名为 person 的对象，它包含了一个 name 属性和一个 sayHello 方法。你可以使用 with 语句来访问这些属性和方法，如下所示：

    var person = {
        name: "John",
        sayHello: function() {
            console.log("Hello, my name is " + this.name);
        }
    };
    
    // 使用 with 语句访问 person 对象中的属性和方法
    with (person) {
        console.log(name);      // 输出 "John"
        sayHello();             // 输出 "Hello, my name is John"
    }

### 3.7 函数
