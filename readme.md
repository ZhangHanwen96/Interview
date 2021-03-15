# Summary for interview questions
1. 判断数据类型:typeof, instanceof, Object.prototype.toString(), constructor
2. [类数组与数组的区别，转换](#2)
3. [new的原理](#3)
4. [如何正确判断this](#4)
5. [addEventlistener和onClick的区别](#5)
6. [new 和 Object.create的区别](#6)
7. [Js 底层 Array实现机制]: v8 engine < 10 插入，> 10快排
8. [Dom 常见操作方式](#7)
9. [模块加载比较ES6 和 CommonJs](#8)


## 计算机网络
1. [TCP和UCP]()
2. [cookie 相关首部字段](#n2)
3. [谈一谈浏览器的缓存机制](#n3)
4. [XSS和CSRF攻击](#n4)

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





### 模块加载比较ES6 和 CommonJs
<span id='8'></span>
1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本**静态分析**的时候，遇到模块加载命令 import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。

2. CommonJS 模块是**运行时加载**，ES6 模块是**编译时输出接口**。CommonJS 模块就是**对象**，即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

































### cookie 相关首部字段
<span id='n2'></span>
* NAME: 赋予cookie的名称和值
* domain： 作为cookie适用对象的域名
* expires：有效期
* httpOnly： 加上限制，使cookie不能被script脚本访问，防止跨站脚本攻击
* secure: 近在https通信才会发送cookie
* path：限定指定cookie发送范围的文件目录


## 计算机网络方面
### 谈一谈浏览器的缓存机制？
<span id='n3'></span>
>防止重复请求相同的资源

浏览器的缓存机制指的是通过在一段时间内保留已接收到的 web 资源的一个副本，如果在资源的有效时间内，发起了对这个资源的再一次请求，那么浏览器会**直接使用缓存的副本**，而不是向服务器发起请求。使用 web 缓存可以有效地提高页面的打开速度，减少不必要的网络带宽的消耗。

web 资源的缓存策略一般由服务器来指定，可以分为两种，分别是**强缓存策略**和**协商缓存**策略。

使用强缓存策略时，如果缓存资源有效，则直接使用缓存资源，不必再向服务器发起请求。强缓存策略可以通过两种方式来设置，分别是 http 头信息中的** Expires 属性和 Cache-Control 属性。**

服务器通过在响应头中添加 Expires 属性，来指定资源的过期时间。在过期时间以内，该资源可以被缓存使用，不必再向服务器发送请求。这个时间是一个绝对时间，它是服务器的时间，因此可能存在这样的问题，就是客户端的时间和服务器端的时间不一致，或者用户可以对客户端时间进行修改的情况，这样就可能会影响缓存命中的结果。

Expires 是 http1.0 中的方式，因为它的一些缺点，在 http 1.1 中提出了一个新的头部属性就是 Cache-Control 属性，
它提供了对资源的缓存的更精确的控制。它有很多不同的值，常用的比如我们可以通过设置 max-age 来指定资源能够被缓存的时间
的大小，这是一个相对的时间，它会根据这个时间的大小和资源第一次请求时的时间来计算出资源过期的时间，因此相对于 Expires
来说，这种方式更加有效一些。常用的还有比如 private ，用来规定资源只能被客户端缓存，不能够代理服务器所缓存。还有如 n
o-store ，用来指定资源不能够被缓存，no-cache 代表该资源能够被缓存，但是立即失效，每次都需要向服务器发起请求。

一般来说只需要设置其中一种方式就可以实现强缓存策略，当两种方式一起使用时，Cache-Control 的优先级要高于 Expires 。

使用**协商缓存策略**时，会先向服务器发送一个请求，如果资源没有发生修改，则返回**一个 304 状态，让浏览器使用本地的缓存副本**。
如果资源发生了修改，则返回修改后的资源。协商缓存也可以通过两种方式来设置，分别是 http 头信息中的** Etag 和 Last-Modified **属性。

服务器通过在响应头中添加 Last-Modified 属性来指出资源最后一次修改的时间，当浏览器下一次发起请求时，会在请求头中添加一个 If-Modified-Since 的属性，属性值为上一次资源返回时的 Last-Modified 的值。当请求发送到服务器后服务器会通过这个属性来和资源的最后一次的修改时间来进行比较，以此来判断资源是否做了修改。如果资源没有修改，那么返回 304 状态，让客户端使用本地的缓存。如果资源已经被修改了，则返回修改后的资源。使用这种方法有一个缺点，就是 Last-Modified 标注的最后修改时间只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，那么文件已将改变了但是 Last-Modified 却没有改变，
这样会造成缓存命中的不准确。

因为 Last-Modified 的这种可能发生的不准确性，http 中提供了另外一种方式，那就是 Etag 属性。服务器在返回资源的时候，在头信息中添加了 Etag 属性，**这个属性是资源生成的唯一标识符，当资源发生改变的时候，这个值也会发生改变。在下一次资源请求时**，浏览器会在请求头中添加一个 If-None-Match 属性，这个属性的值就是上次返回的资源的 Etag 的值。服务接收到请求后会根据这个值来和资源当前的 Etag 的值来进行比较，以此来判断资源是否发生改变，是否需要返回资源。通过这种方式，比 Last-Modified 的方式更加精确。

当 Last-Modified 和 Etag 属性同时出现的时候，Etag 的优先级更高。使用协商缓存的时候，服务器需要考虑负载平衡的问题，因此多个服务器上资源的 Last-Modified 应该保持一致，因为每个服务器上 Etag 的值都不一样，因此在考虑负载平衡时，最好不要设置 Etag 属性。

**强缓存策略和协商缓存策略在缓存命中时都会直接使用本地的缓存副本**，区别只在于协商缓存会向服务器发送一次请求。它们缓存不命中时，都会向服务器发送请求来获取资源。在实际的缓存机制中，强缓存策略和协商缓存策略是一起合作使用的。浏览器首先会根据请求的信息判断，强缓存是否命中，如果命中则直接使用资源。如果不命中则根据头信息向服务器发起请求，使用协商缓存，如果协商缓存命中的话，则服务器不返回资源，浏览器直接使用本地资源的副本，如果协商缓存不命中，则浏览器返回最新的资源给浏览器。
