---
title: WordPress里移除插件和主题加载的CSS、JS
---

> WordPress 默认会加载一些前端资源（CSS），目的是为了提高兼容性但是这些东西对我们网站确是没有太大用处，反而增加负担，查过后是可以移出的。
> 例如：wp-block-library-css 就是 WordPress5.0+出的新版编辑器，默认在头部加载新版样式，没啥用还整站页面都有，果断移除。

## 目录

### [基本示例](#基本示例)

### 扩展用例

1. [移除脚本](#移除脚本)
2. [移除多个文件](#移除多个文件)
3. [添加样式和脚本](#添加样式和脚本)

### 基本示例

具体码，加在主题目录下面的`functions.php`里

```PHP
add_action( 'wp_enqueue_scripts', 'remove_front_end_asset', 99);
function remove_front_end_asset() { // 自定义函数，起个有意义的语义化名字即可
    //移除队列里的样式表，括号里的id好需要自己到页面源码里去找
    wp_dequeue_style('wp-block-library');
}
```

![样式表的id号](https://i.imgur.com/Qj0FEBw.png)

上面代码的作用就是`WordPress`在把脚本和样式表加入队列时调用了一个自定义函数，函数内部把`id`为`wp-block-library`的样式表移除了。

> 可以看下里面用到到两个函数的官方说明：
> [wp_enqueue_scripts()](https://developer.wordpress.org/reference/hooks/wp_enqueue_scripts/)
> [wp_dequeue_style()](https://developer.wordpress.org/reference/functions/wp_dequeue_style/)

### 移除脚本

若我们要移除 JavaScript 脚本呢？可以用下面的函数：

```PHP
wp_dequeue_script();    // 移除队列里的脚本
```

### 移除多个文件

若是我们有多个 CSS 需要移除怎么办，扩展下

```PHP
add_action( 'wp_enqueue_scripts', 'remove_front_end_asset', 99);
function remove_front_end_asset() {
    wp_dequeue_style('wp-block-library');
    wp_dequeue_style('wp-block-library2');
    wp_dequeue_style('wp-block-library3');
    ……  // 没逗你，这样感觉更直接看起来干净些
}
```

### 添加样式和脚本

既然能移除当然也能添加

```PHP
add_action( 'wp_enqueue_scripts', 'add_front_end_asset', 99);
function add_front_end_asset() { // 自定义函数，起个有意义的语义化名字即可
    wp_enqueue_style( 'style-name', get_stylesheet_uri() ); // 添加样式
    wp_enqueue_script( 'script-name', get_template_directory_uri() . '/js/example.js', array(), '1.0.0', true );  // 添加脚本
}
```

> 查看官方说明：
> [wp_enqueue_style()](https://developer.wordpress.org/reference/functions/wp_enqueue_style/) > [wp_enqueue_scripts()](https://developer.wordpress.org/reference/hooks/wp_enqueue_scripts/)

### (完)
