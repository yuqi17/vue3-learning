
### [参考](https://cn.vuejs.org/guide/quick-start.html) 
```sh
   npm create vue@lates
```
经过一系列选择, 最后生成一个脚手架. 然后到目录里安装, 执行npm i; npm run format; npm run dev;
要升级到 node 18, 否则启动会报错.

编译是用[]vite](https://cn.vitejs.dev/guide/)

### vue3 目前还是用Vue-Router 做路由配置;而状态管理则由之前的Vuex换成了Pinia(菠萝)
[Pinia demo](https://stackblitz.com/github/piniajs/example-vue-3-vite?file=src%2FApp.vue)
[Pinia 文档](https://pinia.vuejs.org/zh/core-concepts/)

## 代码的两种编写风格: 
```ts
//选项式 和 组合式(包含<script setup> 
//和 
export default{  setup(){ 
    return { value, func }// 将一些值暴露给模板使用, 比如ref, 函数
}})
```

## 基础 1
1. 最基本的createApp, 模板语法
2. 响应式 的 ref 有点像 react 的 useRef 
3. 响应式 的 reactive 有点想 react 的useState
4. 计算属性 computed 是把ref, reactive 等值进行计算,抽离出来,用于逻辑的复用
5. 条件渲染 v-if v-show ...
6. 列表渲染 <li v-for="item in items">{{ item.message }}</li>
7. 事件: <button @click="handle(124)">click</button> 事件修饰符, 按键修饰符
8. 表单输入绑定: 用v-model 代替:value 和  @input="event => text = event.target.value", 修饰符
9.  类和样式绑定:   
```html <MyComponent :class="{ active: isActive }" />
<p :class="$attrs.class">Hi!</p>
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

## 基础 2
### 生命周期
[周期钩子](https://cn.vuejs.org/api/composition-api-lifecycle.html)
常用: onMounted、onUpdated 和 onUnmounted

### 侦听器
有点像 react 的 useEffect 钩子
```
计算属性允许我们声明性地计算衍生值。然而在有些情况下，我们需要在状态变化时执行一些“副作用”：
例如更改 DOM，或是根据异步操作的结果去修改另一处的状态。
在组合式 API 中，我们可以使用 watch 函数在每次响应式状态发生变化时触发回调函数
```
在官方给的示例中可以看到,文本框输入发生了变化,然后引起了一个查询,并把查询的值赋值给另外一个变量

### 模板引用
跟react 的标签的ref一样, 也需要ref()绑定上

### 组件基础
1. 组件可以用<template>标签来定义, 可以用类的方式书写(这种已经渐渐没人用了)
2. 可以在选项式风格的 template字段中用字符串的方式书写(这种一般很少用)

## 深入组件

### 插件
插件可以在createApp 示例上用use的方式导入使用

### 组件
1. 组件可以在createApp 示例上.component()的方式应用到全局
2. 也可以局部的components:{} 使用
3. defineProps(['title']) 或者 props:['title']

### 指令 v-xxx
1. ```一个指令的任务是在其表达式的值变化时响应式地更新 DOM```
2. 可以在createApp 示例上加上全局指令
3. 也可以局部创建,指令可以是

### 属性
1. props
2. defineProps()

### 事件
1. @xxx="func"
2. 组件对外的回调跟react 不同,react是直接把函数传入调用函数,vue 用$emit('increaseBy', 1)

### v-model
1. defineModel() 返回的值是一个 ref
2. 提供双向绑定

### 依赖注入
其实就是相当于react 的context 在上层provide(), 使用的下层inject()

### 异步组件
相当于从服务器下载一个组件到浏览器,需要用import, 最好是搭配suspense标签使用

### 插槽
相当于react 的 children 属性

## 逻辑复用: 自定义钩子, 指令, 插件(一般是用第三方)
跟react的 自定义钩子类似
```js
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

## 内置组件
1. 目前有5个内置组件,两个是跟动画相关(一个是列表动画)
2. 1个是跟组件的缓存相关(可能类似于react memo)
3. 一个是Teleport 类似于react的 Portal
4. Suspense 跟 react 类似, 主要用于渲染费时间的组件

## SSR & SSG(静态站点生成SSG)
vue nuxt VS react next

