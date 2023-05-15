## html面试题汇总

### 说一下对语义化的理解
> 1. 对开发者友好，让人更容易读懂，便于代码的可读性。  
> 2.对机器友好，让搜索引擎容易读懂，利于seo（搜索引擎优化）。

### 说一下HTML5有哪些更新/新增？
> 1. 语义化标签 header（头部）nav（导航）footer（页脚）article（文章）section（定义文章的段落）  
2. 媒体标签 audio（音频） video（视频） source（视频源嵌套在video或者audio中，让浏览器根据媒体类型和编码器支持进行选择，如果都支持任选一个）  
3. 数据存储 localStorage、sessionStorage  
4. 其他控件 canvas（画布）websocket（通信协议）  
5. history API go（加载 history 列表中的某个具体页面）、forward（加载历史列表中的下一个 URL）、back（加载历史列表中的上一个 URL）  

### 说一下iframe有哪些优点和缺点
> - 优点：展现嵌入的网页；加载速度较慢的内容，如广告；可以跨子域通信；  
> - 缺点：iframe会阻塞主页面onload事件；不利于搜索引擎识别；增加http请求；当有滚动条时会很拥挤；

### 行内，块级，空元素，替换元素有哪些？
> - 行内（无法设置宽高内边距外边距）：button span input img  
> -块级（可以设置宽高内边距外边距）：p div ul li h1-h6  
> -空元素（它没有子元素，也没有结束标签） br（换行） hr（水平线） link meta img input  
> -替换元素（通过修改某个属性值呈现的内容就可以被替换的元素就称为“替换元素”）img video audio link meta  

**注意**  img不是块级元素，但是可以块级显示，也拥有块级元素的特点（宽高，内外边距）。
```javascript
// 如果并列，两个图片是块级显示，图片位置上下  
<img src=''/>
<img src=''/>
// 如果写在一行内，两个图片就在一行显示
<img src=''/> <img src=''/>
```

### DOCTYPE的作用？严格模式和混杂模式的区别？
>- < !DOCTYPE>告诉浏览器以HTML5标准解析页面，如果不写，则进入混杂模式  
>- 严格模式（标准模式）：以w3c标准解析代码  
>- 混杂模式（怪异模式/兼容模式）：浏览器用自己的方式解析代码，混杂模式通常模拟老式浏览器的行为，以防止老站点无法工作  
>- HTML5 没有 DTD ，因此也就没有严格模式与混杂模式的区别，HTML5 有相对宽松的方法，实现时，已经尽可能大的实现了向后兼容(HTML5 没有严格和混杂之分)。  
> 区别：  
> 1. **盒模型的高宽包含内边距padding和边框border**   
     在W3C标准中，如果设置一个元素的宽度和高度，指的是元素内容的宽度和高度  
     而在IE5.5及以下的浏览器及其他版本的Quirks模式下，IE的宽度和高度还包含了padding和border。  
> 2. **可以设置行内元素的高宽**   
>    标准模式下给span等行内元素设置width和height都不会生效  
>    而在混杂模式模式下，则会生效。
> 3. **可设置百分比的高度**  
>    在标准模式下，一个元素的高度是由其包含的内容来决定的，如果父元素没有设置高度，子元素设置一个百分比的高度是无效的。  
>    混杂模式是可以的，会一直向上找设置了高度的节点来分配百分比高度  
> 4. **混杂模式设置图片的padding会失效**  
> 5. **在混杂模式下，表格中的字体不会继承它祖先元素**  
> 6. **在标准模式下，如果一个单元格中包含的内容只有图片时，图片底部默认有3像素的空白。而在混杂模式下，图片底部没有空白。**  
>    3像素空白，与img vertial-align的默认值baseline有关，其实大多数时候我们并不希望它保留空白，去除的方法也很简单，设置vertial-align为一个不是baseline的合法值即可  

### 优雅降级和渐进增强区别？
>- **渐进增强 progressive enhancement**： 针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。  
>- **优雅降级 graceful degradation**： 一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。  
>- **区别**： 渐进增强是向上兼容，优雅降级是向下兼容，现在互联网发展迅速，一般采用优雅降级的方式  
> 浏览器前缀  
> ![img.png](img/img.png)

### 说一下常见的meta标签属性和作用?
> **meta标签是html语言中，head区的一个辅助性标签**  
> 1. name属性和content属性：name属性主要用于描述网页，分别有：  
> Keywords（关键字，用来告诉搜索引擎你网页的关键字是什么），  
> description（网站内容描述，用来告诉搜索引擎你的网站主要内容），  
> robots（机器人向导，用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引）content的参数有all,none,index,noindex,follow,nofollow。默认是all  
> author（作者）  
> 2. http-equiv属性，相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，分别有：  
> Expires（期限，可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输）  ，如＜meta http-equiv="expires" content="Fri, 12 Jan 2001 18:18:18 GMT"＞  
> Pragma（cache模式， 禁止浏览器从本地计算机的缓存中访问页面内容），如＜meta http-equiv="Pragma" content="no-cache"＞，这样设定，访问者将无法脱机浏览  
> Refresh（刷新，自动刷新并指向新页面）， 如＜meta http-equiv="Refresh" content="2；URL=http://www.root.net"＞， 其中的2是指停留2秒钟后自动刷新到URL网址  
> Set-Cookie(cookie设定，如果网页过期，那么存盘的cookie将被删除。) ＜meta http-equiv="Set-Cookie" content="cookievalue=xxx; expires=Friday, 12-Jan-2001 18:18:18 GMT； path=/"＞  
> Window-target（显示窗口的设定，强制页面在当前窗口以独立页面显示） ＜meta http-equiv="Window-target" content="_top"＞ 用来防止别人在框架里调用自己的页面 ，-blank 在新窗口显示 -top 当前整个窗口显示 -parent 父容器显示，比如框架嵌套 -self 当前容器显示，比如框架嵌套  
> content-Type（显示字符集的设定） ＜meta http-equiv="content-Type" content="text/html; **charset**=gb2312"＞  

### 说一下HTML5的离线存储（了解）
> 离线缓存（Application Cache）就是web应用缓存方式的一种，可以使得用户在离线状态下，依然能够很完美的浏览网站。  
> HTML5离线缓存的优势？  
>- **离线浏览** 用户可在应用离线时使用它们。  
>- **速度** 已缓存资源加载得更快。  
>- **减少服务器负载** 浏览器将只从服务器下载更新过或更改过的资源。
>
> 原理：
> - HTML5离线缓存是基于manifest(缓存清单文件，后缀名为.appcatche)的缓存机制（不是存储技术），
> 通过这个文件上的清单解析存储离线资源，就像cookie一样被存在本地，之后当处于离线状态时，就直接使用离线存储的资源进行页面的展示。

### 说一下canvas和svg的区别？
>- canvas是画布，适合图形密集型的游戏，不支持事件处理，
>- svg是矢量图，不依赖分辨率，不适合游戏，适合大型渲染区域（地图），支持事件处理

### 说一下src和href的区别？
>- src是引入外部资源下载到文档，会暂停其他资源的下载，直到引入资源加载完毕
>- href是链接外部资源，不会暂停其他资源的下载

### 说一下音视频标签的使用
>- audio和video标签
>- controls 控制面板、autoplay 自动播放、loop=‘true’ 循环播放、muted 静音、 src 目标资源、 preload 是否在页面加载后载入视频。






