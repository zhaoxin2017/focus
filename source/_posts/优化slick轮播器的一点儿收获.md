---
title: 优化slick轮播器的一点儿收获
---

## `init`事件需要写在轮播器初始化之前

官方文档是这样说的：

> When Slick initializes for the first time callback. Note that this event should be defined before initializing the slider.

```JavaScript
// error
$(".slick-element").slick({
    // Your opation
}).on("init",()=>{
    // You can't do anthing I promise
})

// enter
$(".slick-element").on("init",()=>{
    // It's here do your want.
})
$(".slick-element").slick({
    // Your opation
})
```

## `easing`默认只支持 CSS3 的标准动画

[CSS3 标准动画 🔗](https://www.w3schools.com/cssref/css3_pr_animation-timing-function.asp "点击查看")

```JavaScript
$(".slick-element").slick({
    easing: "linear"    // 把下面的值放到这里可以看到效果
})
```

| value                 | Description                                                                                                                                                                                                                                                                                                                                                           |
| :-------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| linear                | The animation has the same speed from start to                                                                                                                                                                                                                                                                                                                        | end |
| ease                  | Default value. The animation has a slow start,                                                                                                                                                                                                                                                                                                                        | then fast, before it ends slowly |
| ease-in               | The animation has a slow start                                                                                                                                                                                                                                                                                                                                        |
| ease-out              | The animation has a slow end                                                                                                                                                                                                                                                                                                                                          |
| ease-in-out           | The animation has both a slow start and a                                                                                                                                                                                                                                                                                                                             | slow end |
| step-start            | Equivalent to steps(1, start)                                                                                                                                                                                                                                                                                                                                         |
| step-end              | Equivalent to steps(1, end)                                                                                                                                                                                                                                                                                                                                           |
| steps(int,start /end) | Specifies a stepping function, with two parameters. The first parameter specifies the number of intervals in the function. It must be a positive integer (greater than 0). The second parameter, which is optional, is either the value "start" or "end", and specifies the point at which the change of values occur within the interval. If the second parameter is | omitted, it is given the value "end" |
| cubic-bezier(n,n,n,n) | Define your own values in the cubic-bezier function Possible values are numeric values from 0 to 1 [可以在这里自己捏一个](https://cubic-bezier.com/#.44,.05,.55,.95)                                                                                                                                                                                                  |
| initial               | Sets this property to its default value. Read about initial                                                                                                                                                                                                                                                                                                           |
| inherit               | Inherits this property from its parent element. Read about inherit                                                                                                                                                                                                                                                                                                    |

### 网上说用`jQuery easing`也是可以滴（我还没尝试过）

**1. 引入 js 文件**
[jQery-easing](https://cdnjs.com/libraries/jquery-easing)

**2. 需要把`useCSS`设置成`false`**

```JavaScript
$('.slider1').slick({
    // ...
    useCSS: false,
    easing: 'easeOutElastic',
    // ...
});
```
