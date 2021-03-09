## Summary for interview questions
1. 判断数据类型:typeof, instanceof, Object.prototype.toString(), constructor
2. [类数组与数组的区别，转换](#2)
3. [new的原理](#3)
4. [如何正确判断this](#4)
5. [addEventlistener和onClick的区别](#5)



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
