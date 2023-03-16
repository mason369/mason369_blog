---
title: Vue都使用那么久了，还不了解它的生命周期吗
tags: [Vue, Vue生命周期]
style: secondary
color: success
comments: true
description: 我记得尤大曾经说过，你看Vue源码干嘛？你使用Vue又不需要它的源码，你只需要会用就行了！但是我们得卷啊，不卷怎么脱颖而出。
---

## 前言

我记得尤大曾经说过，你看 Vue 源码干嘛？你使用 Vue 又不需要它的源码，你只需要会用就行了！

但是我们得卷啊，不卷怎么脱颖而出 😥，我还记得在今年的蓝桥杯群里，有一同届的还不知道哪个大学的哥们，已经在读 Vue/React/Node 的源码了.....作为小菜鸡的我看着大佬侃侃而谈，在群角落里瑟瑟发抖。

**这篇文章会讲到：**

- Vue 的生命周期到底是什么
- Vue 生命周期的执行顺序
- 生命周期的每个阶段适合做什么
- 我们的请求放在哪个生命周期会更合适

## Vue 的生命周期到底是什么？

与其说是 Vue 的生命周期，我觉得不如说是其内组件的生命周期。 简单来说，它的生命周期就是用来描述一个组件从引入到退出的全过程。 那复杂来说呢？就是一个组件从*创建*开始经历了*数据初始化*，*挂载*，*更新*等步骤后，最后被*销毁*。

## Vue 生命周期的执行顺序

他整体是分为三个大阶段的，在三个大阶段中，有细分为若干的小阶段。我们可以在不同的阶段去做不同的事情，后文也会讲到不同的阶段适合我们去做具体什么事情的。

<center><img style="width:70%;" src="https://masonosam.top/assets/2022-11-04-img/1.webp">
<span style="color:orange; border-bottom: 1px solid #d9d9d9;display: inline-block;color: #999;padding: 2px;"></span></center>

我们先来看看它的执行顺序吧：

有两种方法，一种就是[Vue 的官方文档](https://cn.vuejs.org/v2/guide/instance.html#%E5%AE%9E%E4%BE%8B%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)上面有一个图是专门解释生命周期的，但鉴于可能许多小伙伴们都是跟我一样，看英文文档都要伴随着翻译的水平，所以特意在网上找了个翻译过后的汉化版，放在这里给大家做参考：

<center><img style="width:70%;" src="https://masonosam.top/assets/2022-11-04-img/2.webp">

<span style="color:orange; border-bottom: 1px solid #d9d9d9;display: inline-block;color: #999;padding: 2px;">这个图详细的解释了一个 Vue 实例从创建到销毁的全过程。</span></center>

第二种方法，就是我们在 Vue 项目中打印一下，在控制台中我们就能清晰的看出，谁执行的早，谁执行的晚，甚至能看出他们有什么区别：

```javascript
beforeCreate: function () {
        console.group('------beforeCreate创建前状态------');
        console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
        console.log("%c%s", "color:red", "data   : " + this.$data); //undefined
        console.log("%c%s", "color:red", "message: " + this.message)
    },
    created: function () {
        console.group('------created创建完毕状态------');
        console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
        console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
        console.dir(this.$data)
        console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
    },
    beforeMount: function () {
        console.group('------beforeMount挂载前状态------');
        console.log("%c%s", "color:red", "el     : " + (this.$el)); //undefined
        console.dir(this.$el)
        console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
        console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
    },
    mounted: function () {
        console.group('------mounted 挂载结束状态------');
        console.log("%c%s", "color:red", "el     : " + this.$el); //已被初始化
        console.dir(this.$el)
        console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
        console.dir(this.$data)
        console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
    },
    beforeUpdate: function () {
        console.group('beforeUpdate 更新前状态===============》');
        console.log("%c%s", "color:red", "el     : " + this.$el);
        console.dir(this.$el)
        console.log("%c%s", "color:red", "data   : " + this.$data);
        console.dir(this.$data)
        console.log("%c%s", "color:red", "message: " + this.message);
    },
    updated: function () {
        console.group('updated 更新完成状态===============》');
        console.log("%c%s", "color:red", "el     : " + this.$el);
        console.dir(this.$el)
        console.log("%c%s", "color:red", "data   : " + this.$data);
        console.dir(this.$data)
        console.log("%c%s", "color:red", "message: " + this.message);
    },
    beforeDestroy: function () {
        console.group('beforeDestroy 销毁前状态===============》');
        console.log("%c%s", "color:red", "el     : " + this.$el);
         console.dir(this.$el)
        console.log("%c%s", "color:red", "data   : " + this.$data);
        console.dir(this.$data)
        console.log("%c%s", "color:red", "message: " + this.message);
    },
    destroyed: function () {
        console.group('destroyed 销毁完成状态===============》');
        console.log("%c%s", "color:red", "el     : " + this.$el);
         console.dir(this.$el)
        console.log("%c%s", "color:red", "data   : " + this.$data);
        console.dir(this.$data)
        console.log("%c%s", "color:red", "message: " + this.message)
    }
```

## 挂载阶段

其实这个代码的先后顺序就是他执行的先后顺序了，为了能有更新的状态，所以我找了个 todolist 的 demo，可以添加跟删除的，方便我们来看，首先是刚进入页面：

<center><img style="width:70%;" src="https://masonosam.top/assets/2022-11-04-img/3.webp">
<span style="color:orange; border-bottom: 1px solid #d9d9d9;display: inline-block;color: #999;padding: 2px;"></span></center>

我们可以看到*beforeCreate*是最先的，并且在此时的状态下，我们打印的信息什么都拿不到。

之后进入了*created*状态，在这个状态中我们的*el*，也就是 Dom 元素依旧是拿不到的，但是我们已经可以拿到*data*了，这意味着 *created*已经将数据加载进来了 ，已经为这个 Vue 实例开辟了内存空间。

*beforeMount*，挂载前状态，在我的理解挂载就是将虚拟 Dom 转变成真实 Dom 的过程，所以在这之前，我们的*el*当然还是拿不到的。

*mounted*，挂载结束，意味着虚拟 Dom 已经挂载在了真实的元素上，所以从此开始我们就可以拿到 el 了。我们可以用*console.dir*去打印一些我们需要的元素的属性。

至此，我们的挂载阶段就结束了。

## 更新阶段

下面我们删除一个 list，来看一下更新状态。

<center><img style="width:70%;" src="https://masonosam.top/assets/2022-11-04-img/4.webp">
<span style="color:orange; border-bottom: 1px solid #d9d9d9;display: inline-block;color: #999;padding: 2px;">每当我们去改变页面元素的时候，就会进入更新阶段，也就是beforeUpdate,updated这两个状态。</span></center>

## 销毁阶段

下面我们再来看一下最后的销毁阶段。

<center><img style="width:70%;" src="https://masonosam.top/assets/2022-11-04-img/5.webp">
<span style="color:orange; border-bottom: 1px solid #d9d9d9;display: inline-block;color: #999;padding: 2px;"></span></center>

*beforeDestroy*，销毁前状态，在销毁之前，所以我们的元素、data 都是如同挂载之后的阶段一样，都是可以打印出来的。
*destroyed*,其实最让我震惊的是这个，销毁完成的状态，我以为销毁了，那应该什么都打印不出来了，其实不然，他还是什么都可以打印出来的。
*beforeDestroy*和*destroyed*，都是我们离开这个组件才会被调用的生命周期。

## 生命周期的每个阶段适合做什么

下面我们来讲讲，在不同的阶段我们可以做些什么：

*created：*
在 Vue 实例创建完毕状态，我们可以去访问 data、computed、watch、methods 上的方法和数据，但现在还没有将虚拟 Dom 挂载到真实 Dom 上，所以我们在此时访问不到我们的 Dom 元素（el 属性，ref 属性此时都为空）。

> 我们在此时可以进行一些简单的 Ajax，并可以对页面进行初始化之类的操作

*beforeMount：*
它是在挂载之前被调用的，会在此时去找到虚拟 Dom，并将其编译成 Render
*mounted：*
虚拟 Dom 已经被挂载到真实 Dom 上，此时我们可以获取 Dom 节点，\$ref 在此时也是可以访问的。

> 我们在此时可以去获取节点信息，做 Ajax 请求，对节点做一些操作

*beforeupdate：*
响应式数据更新的时候会被调用，beforeupdate 的阶段虚拟 Dom 还没更新，所以在此时依旧可以访问现有的 Dom。

> 我们可以在此时访问现有的 Dom，手动移除一些添加的监听事件

*updated：*
此时补丁已经打完了，Dom 已经更新完毕，可以执行一些依赖新 Dom 的操作。

> 但还是不建议在此时进行数据操作，避免进入死循环（这个坑我曾经踩过）

*beforeDestroy：*
在 Vue 实例销毁之前被调用，在此时我们的实例还未被销毁。

> 在此时可以做一些操作，比如销毁定时器，解绑全局事件，销毁插件对象等

## 父子组件的生命周期

刚刚说的都是单页面，那么父子组件的生命周期会是什么样子呢？我们仅做一个简单的补充。  
不知道大家在刚刚的图中是否注意到了这两行：

<center><img style="width:70%;" src="https://masonosam.top/assets/2022-11-04-img/6.webp">
<span style="color:orange; border-bottom: 1px solid #d9d9d9;display: inline-block;color: #999;padding: 2px;"></span></center>
我们页面中的小li就是一个被嵌入在这个大页面内部的一个子组件，我们也打印了它的生命周期：

```javascript
created() {
        console.log('list created')
    },
    mounted() {
        console.log('list mounted')
    },
    beforeUpdate() {
        console.log('list before update')
    },
    updated() {
        console.log('list updated')
    },
    beforeDestroy() {
        // 及时销毁，否则可能造成内存泄露
        console.log('list beforeDestroy')
    },
    destroyed(){
        console.log('list Destroy')
    }
```

由此可得，在父组件挂载前阶段，子组件已经挂载完成了。
不光是挂载阶段，其他两个阶段我们也可以打印出来，但是在这里我就不细说了，直接上结论：

- 挂载阶段：父组件 beforeMount -> 子组件 created -> 子组件 mounted -> 父组件 mounted
- 更新阶段：父组件 beforeUpdate -> 子组件 beforeUpdate -> 子组件 updated -> 父组件 updated
- 销毁阶段：父组件 beforeDestroy -> 子组件 beforeDestroy -> 子组件 destroyed -> 父组件 destroyed

## 我们的请求放在哪个生命周期会更合适

那么至此我们已经对于 Vue 的生命周期有了一个基本的了解，现在我们来说一说，我们的请求应该放到哪个生命周期中才最为合适。

一般来说，会有两种回答：*created*和*mounted*。
上文已经讲了，这两个回答，前者是数据已经准备好了，后者是连 dom 也已经加载完成了，那么到底哪个才是正确答案呢？
其实，两个都是可以的，但是*mounted*会更好。

> 可能有人会说：*created*的时间会更早，早些调用不是会省很多时间吗？这样性能会不会更高一点
> 别急，我们一点一点来看

- 首先，它确实是早了，但是早不了几微秒，所以这其实没有提高性能
- 其次，我们在*created*阶段并没有去做渲染，所以在接下来我们会去做 Dom 渲染，但是如果此时我们还做了 Ajax 操作，在 Ajax 结束之后就会返回数据，我们就会将其插入到主线程中去运行，去处理数据，但是我们要知道，在浏览器机制中，渲染线程跟 js 线程是互斥的，所以有可能我们做渲染的同时，另一边可能要处理 Ajax 返回的数据了，这时候渲染就有可能被打断，在处理完数组后，去进行重新渲染。那如果在*created*中有多个 Ajax 呢？我们又要重新进行渲染，所以在*created*去做 Ajax 请求这明显不太合适。
- 还有，有的时候我们接到返回的数据的时候可能要在回调函数中去进行一些*Dom*的操作，可是*created*阶段我们还没有将真实 Dom 加载出来，所以相对而言我们还是在*mounted*去调用要好一些

> 如果是服务端渲染，我们将其放入*created*中进行，因为服务端不支持*mounted*。
