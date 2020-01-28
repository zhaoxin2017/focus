---
title: CSS3开启硬件（GPU）加速的方法和坑
---

## 什么叫硬件加速？先来个图片直观的感受下

![硬件加速](https://techie-note.199101.xyz/wp-content/uploads/2019/11/EzfRLUfxd6WaNJxy.gif)
[图片来源](https://kknews.cc/zh-my/news/v9v3eb4.html)

## 什么叫开启 CSS3 的硬件加速

话说烈日高照，汗流浃背的你赶着汗流浃背的 CP 牛在浏览器这块肥沃的土地上艰难的耕耘着，一扭头看见你家的狗(GPU)躺在树下的阴凉处悠闲的闭着眼睛，伸着舌头，脸上似乎还带着刚舔过冰淇凌的满足感，那神态好不舒爽。但是你心里很不爽啊，还是这副不喊不动的德性，然后就对 GPU 吼道：GPU 你过来，给我上。GPU 一个激灵坐了起来，揉了揉自己惺忪的钛合金眼睛，吸回去了刚流到舌尖似乎还带着草莓冰淇凌味儿的哈喇子，然后伸了个懒懒懒懒懒，懒到死的懒腰。1234，5678，2234，5678……经过 0.00000000000001 秒那几乎可以忽略不计的无影热身操后，瞬息间扛起缰绳站在了 CP 牛旁边。你一边扭着头一边心里暗骂：果然这世界上最懒的人是曹操啊，不喊就不动。
GPU 仰着高昂的狗头，眼角的余光中还带着一丝连你都能看得懂的鄙视之意，既然你能看懂我就不说了，咳咳！CP 牛低头。
你扯动了已经被局势渲染成 PK 形状的缰绳，用那股已经激动无比即将按耐不住的情绪所酝酿出的爆发力撞击在你那铿锵有力，弹性十足的声带上。要知道亢虫有悔啊，结果我就直接告诉你吧，一声细细的像皇宫里的娘娘一样的声音，夹杂着惊慌失措的

CSS3 动画默认是不会触发开启硬件加速的，都是使用浏览器缓慢的软件渲染引擎来执行。比如有时候写的移动端网页的动画（比如最简单的模态框）在安卓手机上会出现卡帧的现象，有很大可能就是使用浏览器软件渲染引擎来执行，性能跟不上导致的。
既然是性能跟不上那就加性能呗，就如上面的图片一样速度不够就来个强劲的飞机引擎，走你！妥妥的。
那么 CSS3 的硬件加速就是你看着慢的像牛一样的动画时对 GPU 大吼一声：GUP 你过来，给我上。GPU 说来嘞，然后拉着 CPU 的小手唱着：隐形的翅膀，让我带你去飞翔，思想有多远，咱们就能飞多远，啦儿啦，啦儿啦，啦儿啦儿啦……
其实所谓硬件加速就是告诉浏览器，让它使用 GPU 进行渲染，切换到 GPU 模式，发挥 GPU 的一系列功能。

> **举个例子**：
> CSS 的 `animations, transforms` 以及 `transitions` 不会自动开启 GPU 加速，而是由浏览器的缓慢的软件渲染引擎来执行。为了性能，这个时候或许你就需要开启硬件加速功能。那我们怎样才可以切换到 GPU 模式呢，很多浏览器提供了某些触发的 CSS 规则。
> Chrome, FireFox, Safari, IE9+和最新版本的 Opera 都支持硬件加速，当它们检测到页面中某个 DOM 元素应用了某些 CSS 规则时就会开启，最显著的特征的元素的 3D 变换。

在其他的文章上看到的几个可以切换到 GPU 模式的几个 3d 属性：

```CSS
.isaax{
   -webkit-transform: translate3d(250px,250px,250px)
   rotate3d(250px,250px,250px,-120deg)
   scale3d(0.5, 0.5, 0.5);
}
```

在 iscroll 的文档上看到的是下面这个：

![option.HWCComposition](https://techie-note.199101.xyz/wp-content/uploads/2019/11/2838289-ea6e59359fc1c30e.jpg)

```CSS
.isaax {
   -webkit-transform: translateZ(0);
   -moz-transform: translateZ(0);
   -ms-transform: translateZ(0);
   -o-transform: translateZ(0);
   transform: translateZ(0);
}
```

据说使用以上样式触发硬件加速后会出现 “页面可能会出现闪烁的效果“ 的问题，我是还没有发现，在网上是找到两个可以解决的方法：

### 方法一

```CSS
.isaax {
   -webkit-backface-visibility: hidden;
   -moz-backface-visibility: hidden;
   -ms-backface-visibility: hidden;
   backface-visibility: hidden;

   -webkit-perspective: 1000;
   -moz-perspective: 1000;
   -ms-perspective: 1000;
   perspective: 1000;
}
```

- `backface-visibility` （ie10+）是用来隐藏被旋转元素的背面，translateZ 导致的？;
- 而当为元素定义 `perspective` 属性时，其子元素会获得透视效果。
  换言之，并不是去掉闪烁，而是设成透明[技术太渣根本不敢说话]

### 方法二

如果是 webkit 内核，还有一种方式可以解决：

```CSS
.isaax {
   -webkit-transform: translate3d(0, 0, 0);
   -moz-transform: translate3d(0, 0, 0);
   -ms-transform: translate3d(0, 0, 0);
   transform: translate3d(0, 0, 0);
}
```

## 什么叫坑？也来个图片直观的感受下吧

![什么叫坑](https://techie-note.199101.xyz/wp-content/uploads/2019/11/pNCbbrfF5XUG8mpF.gif)

看了文章才知道，这东西也不是万金油啊，用得不好会适得其反。原文在这里，让大大牛告诉你坑在哪：<http://www.th7.cn/web/html-css/201509/121970.shtml>
然后说一下怎么打开查看【复合层】元素选项的方式，好像上面文章提到的方法有点过时：

```diff
打开控制台
```

![打开控制台](https://techie-note.199101.xyz/wp-content/uploads/2019/11/J8vhi5qSqPvB7aKT.jpg)

```diff
勾选Layer Borders选项，你会发现世界突然清晰了许多（说实话，试了没看明白，多了些线条）
```

![勾选Layer Borders选项](https://techie-note.199101.xyz/wp-content/uploads/2019/11/NNo3o7gW38aq68CX.jpg)

## 最后，附上跳过坑的方法

> 使用 3D 硬件加速提升动画性能时，最好给元素增加一个 z-index 属性，人为干扰复合层的排序，可以有效减少 chrome 创建不必要的复合层，提升渲染性能，移动端优化效果尤为明显。

```CSS
.isaax{
  position: relative;
  z-index: 1;  // 可以设大点，尽量设得比后面元素的z-index值高
}
```

> 作者：issac\_宝华
> 链接：<https://www.jianshu.com/p/9596c82086d5>
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
