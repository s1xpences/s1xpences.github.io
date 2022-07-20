---
title: 前端框架之vue
categories:
  - 前端学习
tags:
  - vue3
date: 2022-07-20 11:43:01
index_img: https://img2.baidu.com/it/u=1405034453,4094362351&fm=253&fmt=auto&app=138&f=JPEG?w=1000&h=500
---
**版本答案`<script setup> + typescript + vite + vloar + pinia`**

### vue3项目目录
1. 主入口文件main.ts
2. main.ts中create方法，创建对应的应用，产生应用的实例对象
3. 组件模板可以没有根标签
4. 组件中defineComponent方法，定义组件，内部可以传入该组件的配置对象，配置对象和vue2的写法一样 

### Composition API(常用部分)
1. setup：只在初始化时执行一次，函数中如果返回对象(包括属性和方法)，则当前组件的模板中可以直接使用

2. ref：定义一个**基本类型的响应式的数据**，返回的是ref对象.对象中有一个value，如果要对数据进行操作需要通过ref对象.value的方式
>组件模板中展示数据是不需要ref对象.value的方式,内部解析模板是自动添加.value
>类型是Ref类型

3. reactive：
>定义多个数据的响应式,**复杂类型的响应式数据**.接收一个普通对象(目标对象)然后返回该目标对象的代理对象
>直接更改普通对象中的成员值，是不响应的
>类型是Proxy类型
>响应式的影响是深层次的，会影响内部嵌套对象的属性
>内部基于es6的Proxy实现

4. vue3响应式原理，通过Proxy和Reflect

5. setup细节问题：
>setup在beforeCreate之前执行,且只执行一次,当前组件实例还没有创建，组件实例对象this不能用
>返回对象中的属性会与data函数返回对象的属性合并成为组件对象的属性
>返回对象中的方法会与methods中的方法合并成为组件对象的方法
>如果有重名setup优先(实际使用报错:有重复属性)
>methods可以访问setup中的方法，但是setup不能访问methods，一般不要混合使用
>setup加了async，返回promise对象，则模板中不能使用promise对象中的属性或方法

6. setup(props,context)参数：
>props：是一个对象，父向子传的并且在子中用props接收的数据
>
>context：
>attrs：当前组件标签上且没有用props接收的数据
>emit：分发事件
>slots：插槽

7. ref和reactive细节：
>如果用ref代理对象/数组，内部会自动将value转换为reactive的代理对象
>ref实现原理：通过给value添加getter/setter实现数据响应式
>reactive实现原理：递归深度响应式，通过Proxy实现目标对象数据的劫持，通过Reflect操作对象内部数据

#### 计算属性和watch
1. 计算属性如果值传入一个回调，则表示get
2. 计算属性返回的是一个ref对象
3. watch可以监视多个数据(数据必须是响应式数据)，监视非响应式数据需要回调函数写法

#### 生命周期对比
1. vue3.x中使用组合式api
2. beforeCreate和Created使用setup代替
3. 3.x中的同级生命周期要比2.x中的执行更快

#### 自定义hook函数
1. 类似于mixin
2. 请求的json数据需要放在public中，请求时/data/data.json不用补全相对路径

#### toRefs的使用
1. toRefs可以把一个响应式对象转换为普通对象，该普通对象的每一个属性都是ref对象

#### ref获取元素

### Composition API(其他部分)
#### shallowReactive与shallowRef
1. shallowReactive：只处理代理对象最外层属性的响应式，浅响应式(实际使用失效)
2. shallowRef：只处理value的响应式，不进行ref代理对象的响应式(实际使用失效)
3. 一般使用reactive和ref即可

#### readonly和shallowReadonly
1. readonly：深度只读，接收一个响应式对象
2. shallowReadonly：浅只读，深层的属性可以更改

#### toRaw和markRaw
1. toRaw：返回由reactive和readonly方法转换成响应式的普通对象
2. markRaw：标记一个对象，使其永远不会转换为代理对象，返回对象本身

#### toRef的特点及使用
1. toRef与响应式数据是关联的，不管改变哪个数据都会改变；将普通数据变为Ref类型数据
2. ref是拷贝一份当前响应式数据，互不影响

#### customRef的使用
1. 防抖案例

#### provide和injec
1. 跨层级组件通信

#### 响应式数据的判断
1. isRef：判断是否为ref对象
2. isReactive：是否为reactive创建的响应式代理
3. isReadonly：是否为readonly创建的只读代理
4. isProxy：检查是否为reactive和readonly创建的代理

### 新组件
1. Fragment(片段)：
>组件可以没有根标签，内部会将多个标签包含在Fragment虚拟元素中
>减少标签层级，减少内存占用
2. Teleport(瞬移)：提供了一种方法，让组件在父组件外的标签下显示
3. Suspense(不确定的)：应用程序在等待异步组件时渲染一些后备内容，可以让我们创建一个平滑的用户体验

### 组件通信
1. 父向子传值props
2. 子向父传值emit
3. 后代调用父的方法，父通过props将自己的方法传给后代，后代调用

### 总结
1. 2020年9月发布正式版
2. Vue3支持大部分Vue2的特性
3. Vue3设计了一套强大的Composition API代替了Vue2中的Option API，复用性更好
4. 更好的支持ts
5. Vue3使用了Proxy配合Reflect代替了Vue2中的Object.defineProperty()方法数据响应式
6. 重写了虚拟DOM，速度更快
7. 新的组件：Fragment(片段)、Teleport(瞬移)、Suspense(不确定)
8. 设计了新的脚手架工具vite
---
官方文档：[vue](https://v3.cn.vuejs.org/)

参考视频：[BV1ra4y1H7ih](https://www.bilibili.com/video/BV1ra4y1H7ih)  [BV19P4y1g7Vp](https://www.bilibili.com/video/BV1BA4y1X7bp?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=1b26e7eb1ec7cfea25ba7eb77782eb66)
