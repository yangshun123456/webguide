﻿

几道面试题

## 页面布局

### 如何实现一个三栏布局，要求两边固定宽度、中间自适应。

此题可以考察的知识点：

- 圣杯布局

- 双飞翼布局

- flex布局（css3）


### 让元素垂直居中

**方式一：**如果宽高已知，可以利用绝对定位。

**方式二：**用 translate 位移来做。（在宽高未知的情况下，也可以这样做）

```css
    div {
        background-color: red;
        position: absolute;       绝对定位的盒子
        top: 50%;               首先，让上边线居中
        transform: translateY(-50%);    然后，利用translate，往上走自己宽度的一半【推荐写法】
    }
```


**方式三：**flex 布局

```css
    parentElement{
        display: flex;/*Flex布局*/
        display: -webkit-flex; /* Safari */
        align-items: center;/*设置子元素在侧轴方向上的布局*/
    }
```


参考链接：

- <https://www.zhihu.com/question/20543196>

- [水平垂直居中方案与flexbox布局](https://www.cnblogs.com/coco1s/p/4444383.html)




## 变量提升


**问题**：说一下你对JavaScript变量提升的理解。

**定义**：

在函数体内部，声明变量，会把该变量提升到函数体的最顶端。注意：**只提升变量声明，不赋值**。

**代码1**：

```javascript
    fn();

    function fn() {
        var x = 1;
        var y = 2;
    }
```


上方代码中：

（1）给fn创建函数上下文，找到fn中**所有**用var声明的变量（即x和y）；

（2）将这些变量初始化为undefined；

（3）将x赋值为1，将y赋值为2。


**代码2**：


```javascript
    fn2();

    function fn2() {
        console.log(2);

    }
```


上方代码中：

（1）创建全局上下文，找到所有用function声明的变量，在环境中“创建”这些变量。

（2）将这些变量**初始化**，并**赋值**为 `function(){ console.log(2) }`（并不是undefined）

（3）开始执行代码`fn2();`

**代码3**：（let的出现）

```javascript
{
  let x = 1
  x = 2
}
```


上方代码中：

（1）找到所有用 let 声明的变量，在环境中「创建」这些变量

（2）开始执行代码（注意现在还没有初始化）

（3）执行 x = 1，将 x 「初始化」为 1（这并不是一次赋值，如果代码是 let x，就将 x 初始化为 undefined）

（4）执行 x = 2，对 x 进行「赋值」



代码4：

```javascript
let x = 'global'
{
  console.log(x) // Uncaught ReferenceError: x is not defined
  let x = 1
}
```

原因有两个：

- console.log(x) 中的 x 指的是下面的 x，而不是全局的 x

- 执行 log 时 x 还没「初始化」，所以不能使用（也就是所谓的暂时死区）

看到这里，你应该明白了 let 到底有没有提升：

- let 的「创建」过程被提升了，但是初始化没有提升。

- var 的「创建」和「初始化」都被提升了。

- function 的「创建」「初始化」和「赋值」都被提升了。


参考链接：

- [我用了两个月的时间才理解 let](https://zhuanlan.zhihu.com/p/28140450)


### this

问题：下方代码的打印结果是什么？

```javascript
    function A() {
        this.name = 'smyhvae';
    }

    A.prototype.test = function () {
        setTimeout(function () {
            console.log(this.name);
        }, 1)
    }

    var a = new A();
    a.test();
```

打印结果是window.name，但实际的结果是空的。

这个神奇的特性，被用来解决跨域数据传递。（网上可以查一下iframe相关）



**总结1**：this永远指向**函数运行时所在的对象**，而不是函数被创建时所在的对象。即：**谁调用**，指向谁。

**举例**：

```javascript
    var name = '全局';

    function getName() {
        var name = '局部';
        return this.name;
    };

    alert(getName());

```

上方代码的打印结果是：`全局`。

分析：`getName()`这个函数其实是window调用的，所以this指向的window，因为外部有name这个变量，所以打印结果为`全局`。


**总结2**：没有明确的当前对象时，this永远指向window。这个在setTimeout里比较常见。



### apply、call、bind的区别





## 链式调用

**问题**：如何实现类似jQuery的链式调用？

答案：一直return `this`就好了。


## Yslow和pageSpeed


Yslow和pageSpeed你知道怎么用吗？你记得其中多少规则？



## DNS的查询时间

**问题：**前端怎样拿到DNS的查询时间？

### H5中的方法：performance.timing

window.performance这个api可以用来做前端性能监控。其中，timing这个方法。。


参考链接：

- <https://github.com/fredshare/blog/issues/5>



