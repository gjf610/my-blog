## Canvas 和 SVG 的区别是什么？

* Canvas 主要是用笔刷来绘制 2D 图形的。
* SVG 主要是用标签来绘制不规则矢量图的。

相同点：都是主要用来画 2D 图形的。

不同点：
1. Canvas 画的是位图，SVG 画的是矢量图。
2. SVG 节点过多时渲染慢，Canvas 性能更好一点，但写起来更复杂。
3. SVG 支持分层和事件，Canvas 不支持，但是可以用库实现。