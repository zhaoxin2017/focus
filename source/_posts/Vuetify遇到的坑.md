---
title: Vuetify遇到的坑
---

整理代码过后发现`select`组件的事件失灵了找了一圈发现原来是把`change`事件前面的`v-on`删掉了。加上后功能恢复，小坑一个。

![select绑定change事件](https://i.imgur.com/Wjy075n.png)
