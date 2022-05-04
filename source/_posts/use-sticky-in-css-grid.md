---
title: 在CSS grid中使用position:sticky
date: {{ date }}
tags: 
- CSS
- Code
---
我最近在项目中使用了grid，发现pisition:sticky似乎有时候会不起作用。
让我们来看一下吧。
### 1.介绍
我的主要代码是这样的：
```
<div class="wrapper">
  <main></main>
  <aside></aside>
</div>
```
```
.wrapper {
  display: grid;
  grid-template-columns: minmax(10px, 1fr) 250px ;
  grid-gap: 1rem;
}
```
这样就创建了一个宽度250px的侧边栏，剩余内容为主题内容的布局，很简单。

![a grid layout](https://pic3.zhimg.com/80/v2-e3d57c47d3ba96184e30f5c824d490aa_720w.jpg "a grid layout")
现在我要给侧边栏添加position: sticky;使其滚动到顶部后保持不动。
```
aside {
  position: sticky;
  top: 0;
}
```
但是这样并没有达到我的预期：
![a grid layout](https://pic3.zhimg.com/v2-602cf35e122b5a2ac5c521afad90399e_b.webp "a grid layout")
之所以会出现这种情况，是因为默认的align-items值为stretch。
### 2.align-items
接下来我们为侧边栏添加背景，可以看出侧边栏的高度是和主题内容的高度相同的。
![a grid layout](https://pic3.zhimg.com/80/v2-76b8f66c0be345b8efa374c161b05c8a_720w.jpg "a grid layout")
这时align-items的默认行为strech，网格中的列将会被拉伸到高度等于所有列中最高的那一列。
看一个更加清晰的例子：
![a grid layout](https://pic1.zhimg.com/80/v2-8f70ecb7d1b2607b84b8a7cfb71f0b40_720w.jpg "a grid layout")
```
.cards__list {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 1rem;
}
```
这是一个网格布局，默认情况下，所有的卡片高度相同。如果要覆盖这种行为，我们需要将align-items改为start。
```
.cards__list {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 1rem;
  align-items: start;
}
```
![a grid layout](https://pic3.zhimg.com/80/v2-8026044cacc0d2a80d586cefd1dfb936_720w.jpg "a grid layout")
回到我们刚开始的例子。我给侧边栏添加了一个显眼的背景，切换align-items的值，看看侧边栏的高度有什么变化。
![a grid layout](https://pic3.zhimg.com/v2-cc56716b1bb83c621cebcb3aaa055cb2_b.webp "a grid layout")
为了使侧边栏具有黏性，我们可以给aside添加css:
```
aside {
  align-self: start;
}
```
也可以在父容器上使用align-items，这将影响所有的子项。
```
.wrapper {
  display: grid;
  grid-template-columns: 250px minmax(10px, 1fr);
  grid-gap: 1rem;
  align-items: start;
}
```
这两者都可以实现我们想要的效果。
### 3.其他情况
不过有种情况，只能用在子项上来达到我们的预期。看下面这种布局。
![a grid layout](https://pic1.zhimg.com/80/v2-042976cdb8fa5643fe36eeb19006b7b4_720w.jpg "a grid layout")
我们希望页面滚动的时候，section title只在相应的section滚动的时候滚动到顶部固定住。这个时候，只有使用align-self:start才可以实现。
```
.section {
  display: grid;
  grid-template-columns: 100px 1fr;
  grid-gap: 2rem;
}

.section__title {
  position: sticky;
  top: 1rem;
  align-self: start;
}
```
![a grid layout](https://pic3.zhimg.com/v2-7d06100d4def94ee82dcf7c3eb08e14e_b.webp "a grid layout")
