v-model：双向绑定， data中的值改变，v-model的值也变，v-model的值改变，data的值也变。都会重新渲染。



v-on:click（简写 @click）：绑定方法，需要在vue 中的methods中定义方法。



v-bind:someprop（简写 :someprop）：绑定变量，组件中的变量赋予后面的值。



Vue.componet，全局定义组件



style的 scope 属性 ：只对本组件有效





data he props 的区别是什么？



# 一、组件

## 1. 组件Prop写法

### a. 数组的形式：

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

### b. 对象形式：

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

## 2. 监听子组件

在vue中组件间的数据传递是单向的，由父组件传递给子组件，传递方式如下：

```js
<div>
    <sub-comp
      v-bind:key="post.id"
      v-bind:post="post"
    ></sub-comp>
</div>
```

只要不是字符串 ，传递值的时候都需要用v-bind绑定子组件中的属性。

既然是单向传递，那么子组件要让父组件改变一些子组件的内容，如何做到？这就需要子组件绑定一个emit，然后父组件监听到这个事件后按要求修改并刷新子组件，子组件绑定事件代码如下：

```js
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

父组件监听其定义emit，代码如下：

```
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

父级组件就会接收该事件并更新 `postFontSize` 的值。

