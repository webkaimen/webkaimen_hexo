---
title: Fluid 页脚增加网站运行时长
author: Robbin
index_img: /images/cover.jpg
banner_img: /images/common/default_index_img.jpg
default_index_img: /images/common/default_index_img.jpg
date: 2022-03-07 23:03:42
updated: 2022-03-07 23:03:42
tags: ["#Fluid", "#页脚", "#基础"]
categories: 杂项
---

Fluid 1.8.4 版本支持自定义页脚内容了，群友常改的网站运行时长，这次无需再修改源代码。

只需要在主题配置中的 `footer: content` 添加：

```XML
footer:
  content: '
    <a href="https://hexo.io" target="_blank" rel="nofollow noopener"><span>Hexo</span></a>
    <i class="iconfont icon-love"></i>
    <a href="https://github.com/fluid-dev/hexo-theme-fluid" target="_blank" rel="nofollow noopener"><span>Fluid</span></a>
    <div style="font-size: 0.85rem">
      <span id="timeDate">载入天数...</span>
      <span id="times">载入时分秒...</span>
      <script src="/js/duration.js"></script>
    </div>
  '
```

`content` 前三行是 Fluid 原有的页脚内容，建议不要删除，可稍作修改，保留 Fluid 的超链接，用于向更多人推广主题。

`duration.js` 包含的才是功能代码，我们在博客目录下创建 `source/js/duration.js`，内容如下：

```JavaScript
!(function() {
  /** 计时起始时间，自行修改 **/
  var start = new Date("2022/02/02 00:00:00");

  function update() {
    var now = new Date();
    now.setTime(now.getTime()+250);
    days = (now - start) / 1000 / 60 / 60 / 24;
    dnum = Math.floor(days);
    hours = (now - start) / 1000 / 60 / 60 - (24 * dnum);
    hnum = Math.floor(hours);
    if(String(hnum).length === 1 ){
      hnum = "0" + hnum;
    }
    minutes = (now - start) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
    mnum = Math.floor(minutes);
    if(String(mnum).length === 1 ){
      mnum = "0" + mnum;
    }
    seconds = (now - start) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
    snum = Math.round(seconds);
    if(String(snum).length === 1 ){
      snum = "0" + snum;
    }
    document.getElementById("timeDate").innerHTML = "本站安全运行&nbsp"+dnum+"&nbsp天";
    document.getElementById("times").innerHTML = hnum + "&nbsp小时&nbsp" + mnum + "&nbsp分&nbsp" + snum + "&nbsp秒";
  }

  update();
  setInterval(update, 1000);
})();
```

不要忘记把上面注释的时间改为自己的时间，至此这项功能就引入到`<footer>` 里了。
