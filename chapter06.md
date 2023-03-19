# 第6章 集合引用类型
## 6.1 Object
1、new操作符
2、对象字面量
```javascript
let person = { 
 name: "Nicholas", 
 age: 29 
}; 
```
## 6.2 Array
> 跟其他语言中的数组一样，ECMAScript 数组也是一组有序的数据，但跟其他语言
不同的是，数组中每个槽位可以存储任意类型的数据。

### 6.2.1 创建数组
1、Array构造函数
```javascript
let colors = new Array(3);//创建一个包含3个元素的数组
```
2、数组字面量
```javascript
let names = [] //创建一个空数组
```
- from():用于将类数组结构转换为数组实例
- of():用于将一组参数转换为数组实例