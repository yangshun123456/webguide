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

### vue双向绑定的原理
> - 通过Object.defineProperty方法劫持数据，在数据修改的时候调用对应的update方法

### vue2.0源码解析
1. 数据劫持
> - data通过Object.defineProperty挂载到Vue示例上，然后使用observe观察者模式对data内部的数据进行观察，此时要区分对象和数组，对象通过Object.defineProperty劫持，数组通过重写push，unshift，splice等方法，判断是否有新增属性，如果有则递归调用observe方法去监视，从而实现数据劫持。

2. 模板编译
> - render > template > el
> - 获取template -> AST（抽象语法树）树 -> render函数 -> 虚拟节点 -> 设置patch（比对新旧节点）-> 打补丁到真实dom

### 为何vue采用异步渲染？
> - 为了提升性能，如果同步更新，每次更新数据都会让组件重新渲染，所以为了性能考虑，vue会在本轮数据更新完成之后再去异步更新视图。



## vue3面试题
### Vue2.0 和 Vue3.0 有什么区别？
> 1. 性能的提升
> - 打包大小减少41%
> - 初次渲染快55%, 更新渲染快133%
> - 内存减少54%
> 2. 使用Proxy代替defineProperty实现响应式
> 3. 新的内置组件Teleport，Fragment
> 4. Vue3可以更好的支持TypeScript
> 5. Composition API（组合API）
> 6. 新的生命周期钩子
> 7. 移除过滤器（filter）

### Teleport内置组件
> - 可以将teleport组件中包裹着的元素传送到外层的位置中去```<Teleport to="body"></Teleport>```
> - 禁用 Teleport```<Teleport :disabled="isMobile">```
> - 多个 Teleport 共享目标 ```<Teleport to="#modals"><div>A</div></Teleport><Teleport to="#modals"><div>B</div></Teleport>```

### Fragment
> - 在Vue2中: 组件必须有一个根标签
> - 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中

### vue3双向绑定性能提升
> - vue2使用Object.defineProperty进行数据劫持，但是这个方法只能对属性进行监听，无法监听对象，如果要监听需要遍历对象的key进行属性劫持。
> - vue3使用es6的proxy代理实现的，通过指定代理对象操作目标对象，可以监听对象内的属性变化，不需要遍历key。

### proxy的使用
> 1. get(target, propKey, receiver)
> 2. set(target, propKey, value, receiver)
> 3. has(target, propKey)
> 4. deleteProperty(target, propKey)
> 5. apply(target, object, args) 拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
```javascript
let isPrime = function (number) {
  return number
}
let proxy = new Proxy(isPrime, {
  apply: (target, object, args) => {
    console.time('aaa')
    const result = target.apply(object, args)
    console.timeEnd('bbb')
    return result
  }
})
proxy(666)
```

### vue3生命周期
> - vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名
> 1. beforeDestroy改名为 beforeUnmount
> 2. destroyed改名为 unmounted
> - Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：
> 1. beforeCreate===>setup()
> 2. created=======>setup()
> 3. beforeMount ===>onBeforeMount
> 4. mounted=======>onMounted
> 5. beforeUpdate===>onBeforeUpdate
> 6. updated =======>onUpdated
> 7. beforeUnmount ==>onBeforeUnmount
> 8. unmounted =====>onUnmounted

### Vue3.0中的响应式原理是什么？vue2的响应式原理是什么？
> - vue2.x的响应式
> 1. 对象类型：通过Object.defineProperty()对属性的读取、修改进行拦截（数据劫持）。
> 2. 数组类型：通过重写更新数组的一系列方法来实现拦截如：push、pop、shift、unshift、splice、sort、reverse共七个。（对数组的变更方法进行了包裹）。
> - vue3.0的响应式
> 1. 通过Proxy（代理）: 拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。
> 2. 通过Reflect（反射）: 对源对象的属性进行操作。
```javascript
new Proxy(data, {
    // 拦截读取属性值
    get (target, prop) {
        return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
        return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
        return Reflect.deleteProperty(target, prop)
    }
})
```

### vue3响应式数据的判断？
> - isRef: 检查一个值是否为一个 ref 对象
> - isReactive: 检查一个对象是否是由 reactive 创建的响应式代理
> - isReadonly: 检查一个对象是否是由 readonly 创建的只读代理
> - isProxy: 检查一个对象是否是由 reactive 或者 readonly 方法创建的代理

### vue3的常用 Composition API有哪些？
> - reactive：用于创建一个响应式的对象，将普通对象转换为响应式对象
> - ref：用于创建一个包装器对象，将普通数据类型转换为响应式对象。
> - computed：计算属性
> - watch：监听属性 ```watch(sum,(newValue,oldValue)=>{
    console.log('sum的值变化了',newValue,oldValue)
    },{immediate:true,deep:true})```
> 1. 监听reactive定义的响应式数据，会强制开启深度监听（deep：true），无法获取正确的oldvalue（变化前的值）。
> 2. 监听reactive定义的响应式数据中的某个属性(对象形式)时，不会强制开启深度监听，需要自己手动设置（deep：true）才会有效果。
> - watchEffect：类似于watch，但不需要显式指定要监视的数据。它会自动追踪在其内部使用的响应式数据，并在其变化时执行回调函数，比较类似computed
> - toRef： 复制 reactive 里的单个属性并转成 ref，这样创建的 ref 与其源属性保持同步：改变源属性的值将更新 ref 的值，反之亦然。
> - toRefs：复制 reactive 里的所有属性并转成 ref，这个普通对象的每个属性都是指向源对象相应属性的 ref
```javascript
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2
  })

  // ...基于状态的操作逻辑

  // 在返回时都转为 ref
  return toRefs(state)
}

// 可以解构而不会失去响应性
const { foo, bar } = useFeatureX()
```
> - onMounted 和 onUnmounted 函数：onMounted 函数用于在组件挂载到DOM后执行一次性的副作用操作，而 onUnmounted 函数用于在组件卸载时执行清理操作。
> - defineProps：用于获取传递过来的props
> - defineEmit：用于定义当前组件含有的事件
> - useAttrs：获取attrs
> - toRaw：将一个由reactive生成的响应式对象转为普通对象。
> - markRaw：标记一个对象，使其永远不会再成为响应式对象。

### setup的几个注意点
> - 在beforeCreate之前执行一次，this是undefined。
> - setup的参数：
> 1. props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
> 2. context：上下文对象包括，attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 this.$attrs。
> slots: 收到的插槽内容, 相当于 this.$slots。emit: 分发自定义事件的函数, 相当于 this.$emit。

### reactive对比ref
> - ref用来定义：基本类型数据。reactive用来定义：对象（或数组）类型数据。ref也可以用来定义对象（或数组）类型数据, 它内部会自动通过reactive转为代理对象。
> - ref通过Object.defineProperty()的get与set来实现响应式（数据劫持）。reactive通过使用Proxy来实现响应式（数据劫持）, 并通过Reflect操作源对象内部的数据。
> - ref定义的数据：操作数据需要.value，读取数据时模板中直接读取不需要.value。reactive定义的数据：操作数据与读取数据：均不需要.value。

### 么是hook？什么是自定义hook函数？
> - 本质是一个函数，把setup函数中使用的Composition API进行了封装。
> - 类似于vue2.x中的mixin，引入后直接调用就可以。
> - 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。
```javascript
import {onBeforeMount, onBeforeUnmount, reactive} from 'vue'
export default function () {
    const point = reactive({
        x:0,
        y:0
    })
    function savePoint (event) {
        point.y = event.pageY
        point.x = event.pageX
        console.log('x,y', point.y, point.x)
    }
    onBeforeMount(() => {
        // 监听click事件
        window.addEventListener('click',savePoint)
    })
    onBeforeUnmount(() => {
        window.removeEventListener('click',savePoint)
    })
    return point
}
```
```html

<script>
    import {ref} from 'vue'
    import usePoint from '../hooks/usePoint'
    export default {
        name: 'Demo',
        setup(){
            //数据
            let sum = ref(0)
            // 引用公共hook函数
            let point = usePoint()
            return {
                sum,
                point
            }
        },
    }
</script>
```

### Options（选项式） API 存在的问题是什么？Composition（组合式） API 的优势有哪些？
> - 使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改。
> - 组合式API我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

### vue3 hook函数比vue2的mixins好在哪
> - 可以传参给hook，mixins不能
> - 多个hook里面的属性不会相互冲突，而引入多个mixins会覆盖。

## uni-app
### uni-app的生命周期
> - onload：次进入页面加载时触发，可以在 onLoad 的参数中获取打开当前页面路径中的参数。
> - onShow：加载完成后、后台切到前台或重新进入页面时触发
> - onReady：页面首次渲染完成时触发
> - onHide：从前台切到后台或进入其他页面触发
> - onUnload：页面卸载时触发
> - onPullDownRefresh：监听用户下拉动作
> - onReachBottom：页面上拉触底事件的处理函数
> - onShareAppMessage：用户点击右上角转发
> - onResize：监听窗口尺寸变化
> - onPullDownRefresh：下拉回调

### uni-app小程序分包
> - 在page.js里面配置subPackages
> - 建立分包根目录pagesB
```json
"subPackages": [{
    "root": "pagesB",  
    "pages": [
        {
            "path" : "line_otem_detail/line_otem_detail",
            "style" : {
                "navigationBarTitleText": "确认订单"
            }
        },
     ]
}]
```
> - 在进入页面前要加载这个资源
```json
"preloadRule": {
    "pages/index/index": {
        "network": "all",
        "packages": ["pagesB"]
    }
},
```

