---
title: JS判断Android、iOS或浏览器的多种方法(四种方法)
---

## 第一种：通过判断浏览器的 userAgent，用正则来判断是否是 ios 和 Android 客户端

```javascript
  var u = navigator.userAgent; var isAndroid = u.indexOf('Android') > -1 ||
  u.indexOf('Adr') > -1; //android终端 var isiOS = !!u.match(/\(i[^;]+;( U;)?
  CPU.+Mac OS X/); //ios终端 alert('是否是Android：'+isAndroid);
  alert('是否是iOS：'+isiOS);
```

## 第二种：检查是否是移动端（Mobile）、ipad、iphone、微信、QQ 等

代码如下：

```javascript
//判断访问终端
var browser = {
  versions: (function() {
    var u = navigator.userAgent,
      app = navigator.appVersion;
    return {
      trident: u.indexOf("Trident") > -1, //IE内核
      presto: u.indexOf("Presto") > -1, //opera内核
      webKit: u.indexOf("AppleWebKit") > -1, //苹果、谷歌内核
      gecko: u.indexOf("Gecko") > -1 && u.indexOf("KHTML") == -1, //火狐内核
      mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
      ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
      android: u.indexOf("Android") > -1 || u.indexOf("Adr") > -1, //android终端
      iPhone: u.indexOf("iPhone") > -1, //是否为iPhone或者QQHD浏览器
      iPad: u.indexOf("iPad") > -1, //是否iPad
      webApp: u.indexOf("Safari") == -1, //是否web应该程序，没有头部与底部
      weixin: u.indexOf("MicroMessenger") > -1, //是否微信 （2015-01-22新增）
      qq: u.match(/\sQQ/i) == " qq" //是否QQ
    };
  })(),
  language: (navigator.browserLanguage || navigator.language).toLowerCase()
};
```

使用方法

```javascript
//判断是否IE内核
if(browser.versions.trident){ alert(“is IE”); }

//判断是否webKit内核
if(browser.versions.webKit){ alert(“is webKit”); }

//判断是否移动端
if(browser.versions.mobile||browser.versions.android||browser.versions.ios){ alert(“移动端”); }
```

检测浏览器语言

```javascript
var currentLang = navigator.language; //判断除IE外其他浏览器使用语言
if (!currentLang) {
  //判断IE浏览器使用语言
  currentLang = navigator.browserLanguage;
}
alert(currentLang);
```

## 第三种：判断 iPhone|iPad|iPod|iOS|Android 客户端

```javascript
if (/(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent)) {
  //判断iPhone|iPad|iPod|iOS
  //alert(navigator.userAgent);
  window.location.href = "iPhone.html";
} else if (/(Android)/i.test(navigator.userAgent)) {
  //判断Android
  //alert(navigator.userAgent);
  window.location.href = "Android.html";
} else {
  //pc
  window.location.href = "pc.html";
}
```

## 第四种：判断 pc 还是移动端

```javascript
//判断是否手机端访问
var userAgentInfo = navigator.userAgent.toLowerCase();
var Agents = [
  "android",
  "iphone",
  "symbianos",
  "windows phone",
  "ipad",
  "ipod"
];
var ly = document.referrer; //返回导航到当前网页的超链接所在网页的URL
for (var v = 0; v < Agents.length; v++) {
  if (userAgentInfo.indexOf(Agents[v]) >= 0 && (ly == "" || ly == null)) {
    this.location.href = "[http://m.](http://m./)***.com"; //wap端地址
  }
}
```

### JS 判断 iPad

```javascript
var is_iPad =
  navigator.userAgent.match(/(iPad)/) /* iOS pre 13 */ ||
  (navigator.platform === "MacIntel" && navigator.maxTouchPoints > 1);
```
