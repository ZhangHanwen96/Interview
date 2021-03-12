## Summary for interview questions
1. 判断数据类型:typeof, instanceof, Object.prototype.toString(), constructor
2. [类数组与数组的区别，转换](#2)
3. [new的原理](#3)
4. [如何正确判断this](#4)
5. [addEventlistener和onClick的区别](#5)
6. [new 和 Object.create的区别](#6)
7. [Js 底层 Array实现机制]: v8 engine < 10 插入，> 10快排
8. [Dom 常见操作方式](#7)

## 类数组与数组的区别转换
<span id='2'> </span>
```javascript
 Array.prototype.slice.call(arrayList);
 
 [...arrayLike]
 
 Array.from(arrayLike)
```

## new的原理
<span id='3'> </span>
1. 首先创建一个空对象
2. 将这个空对象的原型对象指向构造函数的原型属性，从而继承原型上的方法
3. 将this指向这个空对象，执行构造函数中的代码，以获取私有属性
4. 如果构造函数返回了一个对象res，就将该返回值res返回，如果返回值不是对象，就将创建的对象返回
```javascript
function new() {
  var obj = {};
  
  Constructor = [].shift(arguments);
  
  obj.__proto__ = Constuctor.prototype;
  
  var ret = Constructor.apply(obj, arguments);
  
  return typeof obj === 'object' ? ret : obj;

}
```


## 如何正确判断this
<span id='4'> </span>
1. 函数名直接调用： 指向window，use strict时是undefined
2. this的指向不能在声明的时候确定，而是取决于谁调用了它
3. 常用的this指向改变的方法，call，apply，bind
4. 箭头函数，它和普通函数最不同的一点就是，对于箭头函数的this指向，只需要看它是在哪里创建即可
5. new关键词，每当使用new关键字调用函数时，会自动把this绑定在新对象上，然后再调用这个函数

## addEventlistener和onClick的区别
<span id='5'> </span>
1. onclick事件在同一时间只能指向唯一对象 || addEventListener给一个事件注册多个listener
2. addEventListener对任何DOM都是有效的，而onclick仅限于HTML
3. addEventListener可以控制listener的触发阶段，（捕获/冒泡）。对于多个相同的事件处理器，不会重复触发，不需要手动使用removeEventListener清除

## new 和 Object.create的区别
<span id='6'> </span>
new操作符创建一个对象的过程
1. 创建一个对象obj
2. 将obj连接到原型链上，即设置obj.__proto__ = Constructor.prototype
3. 绑定this指向，传参执行原型函数(参数应用到obj对象上)
4. 判断执行结果，没有则返回obj

Object.create创建一个对象的过程

* 创造一个空构造函数
* 将空的构造函数prototype设为传入的prototype
* 返回实例

```javascript
function A(name) {
    this.name = "AAA";
}
A.prototype.age = 18;
// new
let a1 = new A();
console.log(a1);//A { name: 'AAA' }
console.log(a1.age);//18
// Object.create
let a2 = Object.create(A.prototype);
console.log(a2);//A {}
console.log(a2.age);//18
```
从执行过程和例子可以看出，new和Object.create创建的的实例都具备prototype的属性和参数，但是new创建的实例执行了原来的构造函数A使新对象具备了原构造函数A自身的属性，而Object.create先创建一个空构造函数F，再进行new F()操作，原构造函数A的自身属性不被继承.

## Dom 常见操作方式
<span id='7'> </span>
### 节点查找API
```javascript
document.getElementById ：根据ID查找元素，大小写敏感，如果有多个结果，只返回第一个；

document.getElementsByClassName ：根据类名查找元素，多个类名用空格分隔，返回一个 HTMLCollection 。注意兼容性为IE9+（含）。另外，不仅仅是document，其它元素也支持 getElementsByClassName 方法；

document.getElementsByTagName ：根据标签查找元素， * 表示查询所有标签，返回一个 HTMLCollection 。

document.getElementsByName ：根据元素的name属性查找，返回一个 NodeList 。

document.querySelector ：返回单个Node，IE8+(含），如果匹配到多个结果，只返回第一个。

document.querySelectorAll ：返回一个 NodeList ，IE8+(含）。

document.forms ：获取当前页面所有form，返回一个 HTMLCollection ；
```

### 节点创建API
* createElement
* createTextNode
* cloneNode
* createDocumentFragment

### 节点修改
* appendChild
* insertBefore
* removeChild
* repalceChild

### 节点关系API
1. 父关系
  - parentNode
  - parentElement
2. 子关系
  - children
  - childNodes
  - firstChild
3. 兄弟关系
  - previousSibling
  - nextSibling

### 元素属性
* setAttribute
* getAttribute
* hasAttribute
* dataset

### 样式相关
1. 直接修改
* el.style.color = 'red'
* el.style.setProperty('font-size', '16px')
* 
2.动态
```javascript
var style = document.createElement('style');  
style.innerHTML = 'body{color:red} #top:hover{background-color: red;color: white;}';  
document.head.appendChild(style); 
```
3. classList
```javascript
classList.remove
classList.add
classList.toggle
classList.replace
classList.contains
```
