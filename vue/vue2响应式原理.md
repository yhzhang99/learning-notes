# vue2响应式原理
响应式，也即是说，数据发生改变的时候，视图会重新渲染，匹配更新为最新的值

1. 使用Object.defineProperty可以为对象中的每一个属性，设置get和set方法

    get 值是一个函数，当属性被访问时，会触发get函数

    set 值同样是一个函数，当属性被赋值时，会触发set函数

2. data中的声明的每个属性，都拥有一个数组，依赖收集器subs，保存着谁依赖（使用）了 它

    当页面A读取了name时，会触发name的get函数，此时，name就会在subs保存页面A的watcher

    当name改变时，触发name的set函数，name会遍历自己的依赖收集器subs，逐个通知watcher，让watcher完成更新。这里name会通知页面A，页面A重新读取新的name ，然后完成渲染

3. Object.defineProperty缺点
   
   深度监听时，需要递归到底，一次性计算量大

    无法监听新增属性/删除属性（所以开发中需要使用Vue.set和Vue.delete这两个API来增删data的属性）

    无法原生监听数组，需要特殊处理

# vue3响应式原理
proxy