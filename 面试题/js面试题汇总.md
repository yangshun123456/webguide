## js面试题汇总

### 讲一下 var、let、const 的区别?
> - var 声明的变量有变量提升的特性，而 let、const 没有
> - var 声明的变量会挂载到 windows 对象上，所以使用 var 声明的是全局变量，而 let 和 const 声明的变量是局部变量, 块级作用域外不能访问
> - 同一作用域下 let 和 const 不能重复声明同名变量，而var可以
> - const声明的是常量，必须赋初值，一旦声明不能再次赋值修改，如果声明的是复合类型数据，可以修改其属性

### js中的基础数据类型有哪几种? 了解包装对象吗？
> - 六种，string, number, boolean, undefined, null, symbol
> - 基础数据类型临时创建的临时对象，称为包装对象。其中 number、boolean 和 string 有包装对象，代码运行的过程中会找到对应的包装对象，然后包装对象把属性和方法给了基本类型，然后包装对象被系统进行销毁。
> - 基本类型直接把变量名和值储存在栈中，引用数据类型值存在堆中，变量名和堆对应的地址存储在栈中。

### 如何判断this指向？箭头函数的this指向什么？
> - 普通函数直接调用中的this，this 指向 window 对象，严格模式下为 undefined。
> - 在对象里调用的this指向调用函数的那个对象，this: 谁调用就指向谁
> - 在构造函数以及类中的this通过new关键字指向实例对象
> - 绑定事件函数的this谁调用就指向谁。
> - 定时器中的this指向 window，因为定时器中采用回调函数作为处理函数，而回调函数的 this 指向 window。
> - 箭头函数中的this会继承其父作用域的 this。

### call apply bind 的作用与区别？
> - 作用：改变函数内部 this 的指向
> - 区别：call 和 apply 会调用函数，而 bind 不会调用。call 和 bind 的参数是 参数列表逐个传入，而 apply 的参数必须为数组形式

### 什么是闭包？
> - 闭包是指能够访问另一个函数作用域中的变量的一个函数。 在js中，只有函数内部的子函数才能访问局部变量， 所以闭包可以理解成 “定义在一个函数内部的函数”。
> - 利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部，让外部函数可以访问到内部函数的变量和方法。
> - 正常的函数，在执行完之后，函数里面声明的变量会被垃圾回收机制处理掉。但是形成闭包的函数在执行之后，不会被回收，依旧存在内存中。
> - 因为变量不会被回收，所以内存中一直存在，耗费内存。

### js同步任务和异步任务
> - 同步任务是指在主线程上排队执行的任务，只有前一个任务执行完毕，才能继续执行下一个任务。
> - 异步任务分为 宏任务 和 微任务，异步任务指的是，不进入主线程、而进入"任务队列"的任务，只有等主线程任务执行完毕，"任务队列"的任务才会进入主线程执行。
> - 事件循环：先执行同步任务，再执行当前所有的微任务，然后执行一个宏任务，然后再执行所有的微任务。再执行一个宏任务。再执行所有的微任务·······依次类推到执行结束。
> - 常见的宏任务：settimeout setInterval script(最外层的script标签)，会压入到调用栈中，宏任务会等到调用栈清空之后再执行
> - 常见的微任务：promise (async await)，会在调用栈清空时立即执行(优先级大于宏任务), 调用栈中加入的微任务会立马执行

### 如何理解 script（整体代码块）是个宏任务呢
> - 实际上如果同时存在两个 script 代码块，会首先在执行第一个 script 代码块中的同步代码，如果这个过程中创建了微任务并进入了微任务队列，第一个 script 同步代码执行完之后，会首先去清空微任务队列，再去开启第二个 script 代码块的执行。所以这里应该就可以理解 script（整体代码块）为什么会是宏任务。

### 什么是回调函数，回调函数存在什么问题
> - 回调函数就是一个通过函数指针调用的函数。把函数的指针 (内存地址) 作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，指向的函数就是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。
> - 使用回调函数有一个很大缺点就是造成回调地狱，回到地狱是为了实现某些逻辑出现函数的层层嵌套，可以通过promise代替。

### 什么是高阶函数
> - 高阶函数是一个 **接收函数作为参数或将函数作为输出返回** 的函数

### 浏览器为什么要阻止跨域请求？每次跨域请求会达到服务端吗？
> - 浏览器阻止跨域请求的原因是因为"同源政策" , "同源政策"主要解决浏览器的安全问题，同源策略会阻止一个域的javascript脚本和另外一个域的内容进行交互，减少被攻击的媒介，"同源"是 协议、域名 和 端口都相同，非同源是 协议 域名 和 端口 只要有一个不同都是非同源，就会有跨域问题
> - 服务端不会拦截跨域请求，拦截是在浏览器端的，请求一定是先发出去，在返回来的时候被浏览器拦截了，如果请求是有返回值的，会被浏览器隐藏掉。
> - 通常浏览器在发送请求时会发送一个预检请求（options方法）询问服务端是不是允许这次请求。服务端会在返回的请求上返回一些header，从而告诉客户端是否要发送真正的请求，如果可以，浏览器才会发送真正的请求。
> - 但是简单请求不会发送预检请求，只有复杂请求会。

### 什么是简单请求？
> 是否为简单请求要同时满足以下四个条件：
> - 使用下列方法之一： GET HEAD POST
> - 只使用了安全 Header（如：multipart/form-data Accept-Language Content-Type），不得人为设置其他 Header
> - 请求中的任意 XMLHttpRequest 对象均没有注册任何事件监听器
> - 求中没有使用 ReadableStream 对象（可读的字节数据流）。

### 如何解决跨域？
> - 使用JSONP，通过script标签的src属性获取get请求  
![img.png](跨域.png/img.png)
> - 前端代理转发，devServer中配置代理，让请求通过同源地址代理到真实地址
> - nginx反向代理（生产常用）
> - 服务端过滤器配置过滤器，给响应头加上Access-Control-Allow-Origin属性声明允许跨域访问
> - 浏览器安装Allow CORS插件绕过同源策略
> - 使用postman请求

### 对内存泄漏的了解
> - 定义：程序中已在堆中分配的内存，因为某种原因未释放或者无法释放的问题
> - 导致内存泄漏的原因：意外的全局变量、DOM元素清空时，还存在引用，DOM元素清空时，还存在引用、闭包、遗忘的定时器

### js事件模型
> js事件模型分为原始事件模型、标准事件模型、IE事件模型（不用）
> - 原始事件模型：事件绑定监听函数比较简单， 有两种方式，只支持冒泡，不支持捕获且同一个类型的事件只能绑定一次，后绑定的事件会覆盖之前的事件  
    1. HTML代码中直接绑定  
    ```
    <input type="button" onclick="fun()">
    ```  
    2. 通过JS代码绑定  
    ```
     var btn = document.getElementById('.btn')
     btn.onclick = fun;
    ```  
> - 标准事件模型：在该事件模型中，一次事件共有三个过程:  
    1. 事件捕获阶段：事件从document一直向下传播到目标元素, 依次检查经过的节点是否绑定了事件监听函数，如果有则执行  
    2. 事件处理阶段：事件到达目标元素, 触发目标元素的监听函数  
    3. 事件冒泡阶段：事件从目标元素冒泡到document, 依次检查经过的节点是否绑定了事件监听函数，如果有则执行  
    事件绑定监听函数的方式如下:  
    ```addEventListener(eventType, handler, useCapture)```  
    事件移除监听函数的方式如下:
    ```removeEventListener(eventType, handler, useCapture)```  
    **useCapture**一个boolean用于指定是否在捕获阶段进行处理，一般设置为false与IE浏览器保持一致

### new 操作符具体干了什么?
> - 创建一个空对象，并且把 this 指向这个对象，同时还继承了该对象的原型
> - 属性和方法被加入到 this 引用的对象中
```javascript
function _new(constructer, ...arg) {
   // 创建一个空的对象
   let resultObj = {};
   // 链接该对象到原型,这样新对象就能访问到原型上面的方法
   resultObj.__proto__ = constructer.prototype;
   // 然后实现步骤3，将新创建的对象作为this的上下文
   let result = constructer.call(resultObj, ...arg);
   // 实现步骤4：如果该函数没有返回对象（即result不是一个对象），则返回this（即resultObj)
   return typeof result === 'object' ? result : resultobj
}
```

### typeof用法
> - typeof用于判断数据类型，返回值有6个字符串：string、number、undefined、boolean、object、function
> - 数字类型、typeof 返回的值是 number。比如说：typeof(1)，返回值是 number
> - 字符串类型，typeof 返回的值是 string。比如 typeof(“123”返回值是 string)
> - 布尔类型，typeof 返回的值是 boolean。比如 typeof(true)返回值是 boolean
> - 对象、数组、null 返回的值是 object。  
    比如 typeof(window)，typeof(document)，typeof(null) 返回的值都是 object
> - 函数类型，返回的值是 function  
    比如：typeof(eval)，typeof(Date)返回的值都是 function。
> - 不存在的变量、函数或者 undefined，将返回 undefined。  
    比如：typeof(abc)、typeof(undefined) 都返回 undefined

### instanceof能得到哪些类型
> - instanceof 用来判断对象的具体类型  object instanceof objectType
> - 其原理就是检测函数的prototype是否在被检测对象的原型链上
```javascript

console.log([] instanceof Array) // true
console.log({} instanceof Object) // true
console.log((()=>{}) instanceof Function) // true
 
// instanceof用来判断数组存在误区，原因是Array.prototype.__proto__ === Object.prototype
console.log(Array.prototype.__proto__ === Object.prototype) // true
console.log([] instanceof Object) // true
 
// instanceof用来判断函数存在误区，原因是Function.prototype.__proto__ === Object.prototype
console.log(Function.prototype.__proto__ === Object.prototype) // true
console.log((()=>{}) instanceof Object) // true
```

### 谈一谈箭头函数与普通函数的区别?
> - 不会进行函数提升
> - 没有自己的 this，this指向的是所在作用域指向的对象
> - 不能使用 new 关键字
> - 不可以使用 arguments 对象

### 函数提升和变量提升
> - 使用var声明的变量，函数的声明会进行声明提升，在最顶部执行，且函数优先于var声明
    的变量，函数内部如果用 var 声明了相同名称的外部变量，函数将不再向上寻找，
    函数作用域中var变量会提升到函数最顶部执行，普通函数也会提升到最顶部执行，
    匿名函数不会进行函数提升
> - 变量声明提升会将变量声明提升到作用域顶部，**但只会提升声明部分，不会提升赋值部分**

### == 和 ===的区别
> - == 只比较数值是否相等（比较对象和 === 一样需要判断引用地址）
> - === 要求数值相等，类型相同
> - Object.is()和 === 差不多，多一个比较 +0和 -0不相等，两个NAN相等

### attribute 和 property 的区别是什么
> - attribute是作为html中的属性名称，且可以自定义属性
> - property是作为在js中的属性名称，不能获取自定义属性
> - 对于 html 的标准属性来说，attribute 和 property 是同步的，是会自动更新的
    但是对于自定义的属性来说，他们是不同步的

### 数组(array)方法
> 1. forEach() 方法，用于遍历数组
> 2. map() 方法，用于处理数组，并返回一个数组
> 3. filter() 方法，用于过滤不需要的元素，返回需要的一个数组
> 4. some() 方法，有一个符合条件就返回true
> 5. every() 方法，全部符合才返回true
> 6. indexOf() 方法，查找元素下标，没有返回-1
> 7. push() 方法，在数组最后插入一个元素
> 8. unshift() 方法，在数组前插入一个元素
> 9. pop() 方法，取出第最后一个元素，并删除
> 10. shift() 方法，去除第一个元素，并删除
> 11. sort() 排序
> 12. slice(start, end) 返回截断后的数组，不改变原数组
> 13. splice(start, number, value) 返回删除元素组成的数组，value 为插入项，改变原数组
> 14. concat()拼接多个数组，返回拼接后的数组

### 字符串（String）方法
> 1. toLowerCase() 此方法用于把字符串转为小写，并返回新的字符串。
> 2. toUpperCase() 此方法用于把字符串转为小写，并返回新的字符串。
> 3. concat() 拼接字符串
> 4. substr(start, number) 从start开始截取多少位
> 5. substring(Start, end) 从start开始到end - 1
> 6. replace() 替换
> 7. trim() 去除开头和结尾的空格
> 8. split() 根据符号分割字符串，返回一个数组，会改变原数组
> 9. slice(start, end) 返回截断后的字符串
> 10. indexOf() 返回第一次出现的位置

### number方法
> 1. isNaN() 是否数字
> 2. parseInt()
> 3. toFixed(a) a: 0-20 保留几位小数并四舍五入
> 4. toPrecision(a) a:1-100 保留有效数字从第一个不为0开始
> 5. toLocaleString()

### Js中Math常用方法
> - Math.abs()函数，返回一个数的绝对值 Math.abs(-10) = 10
> - Math.ceil()函数，返回大于或等于一个给定数的最小整数。Math.ceil(5.4) = 6
> - Math.cos()函数，返回一个值的余弦值。Math.sin(90 * Math.PI / 180) = 1
> - Math.floor()方法，返回小于或等于一个给定数字的最大整数 Math.floor(5.7) = 5
> - Math.round()，返回的是一个数字四舍五入的整数。Math.round(5.7) = 6
> - Math.min()方法，是可以返回指定一组数据中最小值。Math.min( 0, 100, -200, -140) = -200
> - Math.max()方法，是可返回指定数据中最大值。Math.max(0, 100, -200, -140)  = 100
> - Math.sqrt()方法，返回的是一个数的平方根。 Math.sqrt(4) = 2
> - Math.random()函数，返回一个浮点，伪随机数范围从0到小于1，从0往上不包括1
> - Math.trunc()函数，返回的是一个数的整数部分，不管正数还是负数，直接去掉小数点及之后的部分。

### JavaScript 原型，原型链 ? 有什么特点？
