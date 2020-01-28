---
title: Vue 定义data时的坑
---

先把坑亮出来，这两条报错信息很有意思

> [Vue warn]: Invalid prop: type check failed for prop "cats". Expected Array, got Object
> [Vue warn]: Invalid prop: type check failed for prop "cats". Expected Object, got Array

## 问题处理过程

上代码还原现场

```javascript
// list组件
data(){
    return {
        cats:{}
    }
},
async created(){
    const response = await this.axios.get(/xxx/xxx)
    this.cats = response.data
}

// menu组件
props:{
    cats: Object,
    requite: true
}
```

那么第一个问题来了

```bash
[Vue warn]: Invalid prop: type check failed for prop "cats". Expected Array, got Object
```

大眼一瞅原来数据类型定义错误，改过来完事呗，于是把 menu 组件里`cats`类型改成`Array`：

```javascript
props:{
    cats: Array,
    requite: true
}
```

有意思的事情发生了 😅

```bash
[Vue warn]: Invalid prop: type check failed for prop "cats". Expected Object, got Array
```

可以对比下这两条报错信息，我当时心里在想：Vue 你是在逗我吗？

经过一番折腾，也查了，也改了，还是没解决。

最后把数据的类型都打印出来看，慢慢找到了元凶

发现 `response.data` 返回的数据是个数组，但是`cats`定义的时候是`{}`对象，经过赋值`this.cats`就成了数组，`menu`组件里数据验证就死活不通过。
最后把 `cats:{}` 改成 **`cats:[]`** 完事。

事情还没有结束，为什么 `menu` 里的 `props` 数据验证通不过？

## 后面的反思

```JavaScript
let arr = {}
let obj = [1,2,3]
arr = obj
```

请问 arr 现在的数据类型是什么？
