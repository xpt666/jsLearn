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
