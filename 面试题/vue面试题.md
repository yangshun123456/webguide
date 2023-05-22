## vue面试题汇总

###  MVC 和 MVVM 区别
> - mvc是controller负责把model的数据在view中显示出来，
    而mvvm新增了vm层，让view和model实现了自动同步，让视图和模型进行了双向绑定，当model改变时，我们不用手动去改变view，而是自动取修改dom。
    mvvm比mvc精简了很多，简化了数据更新操作，不用重新使用选择器去操作dom更新数据。

### 为什么 data 是一个函数
> - 因为在vue中组件是可以复用的，如果写在对象中，则多个调用的地方会共用一个data，不同地方会相互影响，而使用函数返回的话，每一次调用都会返回新的data，就不会互相影响。

### Vue 组件通讯有哪几种方式
> - 父组件通过props向子组件传递数据，子组件通过props接收数据。这是一种单向数据流的方式，父组件可以通过props将数据传递给子组件，但子组件不能直接修改props的值，可以通过emit调用自定义事件方法修改。
> - 组件可以通过$emit方法触发自定义事件，并且父组件可以通过在子组件上使用v-on监听这些事件。通过自定义事件，子组件可以向父组件发送消息。
> - Event Bus（事件总线）创建一个空的Vue实例作为事件总线，然后通过$emit触发事件和$on监听事件来进行组件之间的通信。任何组件都可以订阅事件和触发事件，这种方式适用于跨级组件通信或非父子组件之间的通信。
> - 可以通过vuex进行组件通讯
> - $refs：通过在组件上使用ref属性，可以在父组件中直接引用子组件的实例。这样就可以通过访问子组件的属性和方法来实现通信。
> - $attrs和$listeners：可以通过v-bind="$attrs"将父组件的所有属性传递给内部的根元素。子组件通过$attrs.属性获取。$listeners属性也是一个对象，包含了父组件传递给子组件的所有监听器。通常，监听器是通过v-on或@指令在父组件中定义的，用于捕获子组件触发的事件，子组件通过$listeners.方法调用。
> - provide和inject：Vue中用于跨层级组件通信的高级特性它们允许父组件向其所有子孙组件提供数据，而无需显式地通过组件props进行传递。
```
// 父组件
export default {
  provide: {
    message: 'Hello from parent'
  }
};
// 子组件、孙组件
export default {
  inject: ['message']
};
// 调用
{{ message }}
```

### 为何v-for要用key
> - 当Vue重新渲染列表时，它会使用key属性来对比新旧节点，并尽可能地复用已存在的元素而不是重新创建。如果没有提供key，Vue只能使用默认的比对策略，它会按照元素在数组中的顺序进行比对。这样可能导致一些意外的行为，例如当数组中的元素位置发生变化时，Vue可能会错误地认为某些元素需要重新创建，而实际上它们只是位置变化了。

### 描述vue组件声明周期
> - beforeCreate：在实例初始化之后、数据观测之前被调用。此时，组件的选项和初始数据尚未被创建或设置。
> - created：在实例创建完成后被调用。此时，组件的选项和初始数据已经准备就绪。
> - beforeMount：在组件挂载到DOM之前被调用。此时，模板编译完成，但尚未将组件插入到DOM中。
> - mounted：在组件挂载到DOM之后被调用。此时，组件已经被插入到DOM中，可以进行DOM操作、请求数据等操作。
> - beforeUpdate：在组件更新之前被调用。在此阶段，组件的数据已经发生改变，但DOM尚未更新。
> - updated：在组件更新完成后被调用。此时，组件的数据已经更新，并且DOM已经重新渲染。
> - beforeDestroy：在组件销毁之前被调用。此时，组件仍然完全可用，可以进行清理工作、取消事件监听等操作。
> - destroyed：在组件销毁之后被调用。此时，组件已经被销毁，所有的事件监听器和定时器都已经被移除。

### 父子组件生命周期
> - 挂载： parent beforeCreate => parent created => parent beforeMount => child beforeCreate => child created => child beforeMount => child mounted => parent mounted
> - 更新： parent beforeUpdate => child beforeUpdate => child updated => parent updated
> - 销毁： parent beforeDestroy => child beforeDestroy => child destroyed => parent destroyed

### 描述组件渲染和更新的过程
> 1. 初始渲染：
> - Vue通过组件的选项和模板创建组件实例。 
> - 调用组件的生命周期钩子函数（beforeCreate和created）。
> - Vue对模板进行编译，生成渲染函数。
> - 调用组件的生命周期钩子函数（beforeMount）。
> - 执行渲染函数，将组件的虚拟DOM（Virtual DOM）转化为真实的DOM，并插入到页面中。
> - 调用组件的生命周期钩子函数（mounted）。
> 2. 更新过程：
> - 当组件的数据发生变化时，Vue会触发更新过程。
> - 调用组件的生命周期钩子函数（beforeUpdate）。
> - Vue比较新旧虚拟DOM，找出需要更新的部分。
> - 根据更新的内容，更新对应的DOM节点。
> - 调用组件的生命周期钩子函数（updated）。
> 3. 销毁过程：
> - 当组件被销毁时，Vue会触发销毁过程。
> - 调用组件的生命周期钩子函数（beforeDestroy）。
> - 移除组件的DOM节点。
> - 清理组件的事件监听器、定时器等资源。
> - 调用组件的生命周期钩子函数（destroyed）。

### 双向数据绑定v-model的实现原理
> 1. 实现原理浅层理解：触发input事件来修改value值
> - v-model其实是个语法糖，它实际上是做了两步动作：
> - v-bind:绑定响应式数据
> - 触发 input 事件 并传递数据 (核心和重点)input标签有一个oninput事件，该事件类似于onchange事件，不同之处在于 oninput 事件在元素值发生变化是立即触发， onchange 在元素失去焦点时触发。
> - 通过事件将e.target.value赋值给绑定值
> 2. 深层理解：利用Object.defineProperty()数据劫持来实现。
> - 通过重写Object.defineProperty()中set和get方法，将value赋值给节点，让显示

### props和data谁的优先级高
> - props > methods > data > computed > watch

### nextTick作用
> - Vue.nextTick可以在DOM更新完成后执行一些操作，以确保操作在DOM更新后进行。
> - 在某些情况下，我们需要获取更新后的DOM状态，例如在某个元素的样式发生变化后执行一些操作。通过将操作放在Vue.nextTick的回调函数中，可以确保在DOM更新完成后执行操作，以获取到最新的DOM状态。

### scoped原理
> - scope原理是将style中存在样式的对应标签加一个属性data-v-xxx，然后在style中全部加上一个属性选择器，这样就只有这一个页面中的style会受到影响不会影响到别的。

###  vue中样式穿透
> - less 和 sass 用的都是/deep/进行穿透，stylus用>>>

###  Vue中keep-alive
> - keep-alive是一个用于缓存的标签，是一个抽象组件，渲染时不会生成一个dom节点，有三个属性，include, exclude, max。
> - include，exclude可以用正则匹配，和逗号隔开，分别是需要缓存的和不需要缓存的组件，max是缓存的组件最高值。
> - keep-alive 匹配的name先根据组件的name属性判断，如果没有，则匹配路由中的注册名称。

### vuex中action和mutation有何区别？
> - action中处理异步，mutation不可以。
> - mutation做原子操作，action2可以整合多个mutation。

### vue常见性能优化方式？
> - 合理使用v-if和v-show
> - v-for加key
> - 自定义事件，dom事件及时销毁
> - 合理使用异步组件（动态import语法 () => import('@/component/xxx.vue')）
> - 合理使用keepalive
> - 前端通用性能优化（如图片懒加载/减少 HTTP请求数/合理设置 HTTP缓存/资源合并与压缩/合并 CSS图片/将 CSS放在 head中/避免重复的资源请求/切分到多个域名）
> - 使用ssr

### vue.js的两个核心是什么？
> 1. 数据驱动（Data-driven）：Vue.js采用了数据驱动的方式，即通过建立组件与数据之间的响应关系，使得数据的变化能够自动反映在组件的视图上。当数据发生变化时，Vue会自动更新对应的视图部分，保持视图与数据的同步。这种数据驱动的方式使得开发者可以专注于处理数据逻辑，而无需手动操作DOM，提高了开发效率和代码的可维护性。
> 2. 组件化（Component-based）： Vue.js将应用程序划分为多个组件，每个组件都是一个独立的、可复用的模块，包含了自己的模板、样式和逻辑。通过组件化开发，可以将复杂的应用程序拆分为多个小的、可维护的组件，提高了代码的可复用性和可测试性。组件之间可以进行嵌套和组合，形成组件树结构，最终构成整个应用程序的UI。

### vue中子组件调用父组件的方法?
> - 直接在子组件中通过this.$parent.event来调用父组件的方法。
> - 在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了。

### vue中父组件调用子组件的方法?
> - 通过ref绑定后，用refs调用。

### vue页面级组件之间传值?
> - 使用vue-router通过跳转链接带参数传参。
> - 使用本地缓存localStorge。
> - 使用vuex数据管理传值

### 说说vue的动态组件。
> - 使用动态组件，可以根据需要动态地加载和渲染不同的组件，而无需预先在模板中定义所有可能的组件。这对于处理条件渲染、组件复用以及根据用户操作动态展示不同的组件等场景非常有用。
> - Vue中实现动态组件的主要方式是通过`<component>`标签和动态的is属性。具体的使用方式如下：
```
<div>
    <button @click="toggleComponent">Toggle Component</button>
    <component :is="currentComponent"></component>
</div>
<script>
import ComponentA from './ComponentA.vue';
import ComponentB from './ComponentB.vue';

export default {
  data() {
    return {
      currentComponent: 'ComponentA'
    };
  },
  methods: {
    toggleComponent() {
      // 根据条件切换当前组件
      this.currentComponent = this.currentComponent === 'ComponentA' ? 'ComponentB' : 'ComponentA';
    }
  },
  components: {
    ComponentA,
    ComponentB
  }
};
</script>
```

### $route和 $router的区别是什么？
> - $router为VueRouter的实例，是一个全局路由对象，包含了路由跳转的方法、钩子函数等。
> - $route 是路由信息对象||跳转的路由对象，每一个路由都会有一个route对象，是一个局部对象，包含path,params,hash,query,fullPath,matched,name等路由信息参数。

### v-on可以监听多个方法吗？
> - v-on指令可以监听多个方法。<input type="text"v-on="{ input:onInput, focus:onFocus, blur:onBlur, }"

### vue常用的修饰符
> - .stop - 调用 event.stopPropagation()，禁止事件冒泡。
> - .prevent - 调用 event.preventDefault()，阻止事件默认行为。
> - .capture - 添加事件侦听器时使用 capture 模式（通常，事件是从内向外（由子组件到父组件）触发的，但使用capture修饰符可以改变事件的触发顺序，使加了capture的事件先触发。
> - .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
> - .native - 监听组件根元素的原生事件。
> - .once - 只触发一次回调。

### vue如何做强制刷新？
> - location.reload()
> - $router.go(0)

### 
