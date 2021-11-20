## JavaScript 的诞生

### 诞生之前

- 万维网的概念和基础技术是由 Tim Berners-Lee 于 1989-1991 年间，在 CERN （欧洲核子研究组织）里开发的。
- 1992-1993 年由本科生 Marc Andreessen 和伊利诺伊大学香槟分校超算中心（NCSA）的 Eric Bina 开发了 Mosaic。这款的应用，本质上定义了「Web 浏览器」这一全新软件类别。
- 在 1994 年 4 月，他们共同创立了一家公司。这家公司最终定名为 Netscape（网景通讯），目标是推出世界上最流行的浏览器来替代 Mosaic。为此，Netscape 从零开始研发了下一代 Mosaic 式浏览器 Netscape Navigator，它于 1994 年 10 月起开始发行。到 1995 年初，Netscape Navigator 达到了初始目标，正在迅速地取代 Mosaic。

### Brendan Eich 加入网景

- 1995 年，网景招募了布兰登·艾克，目标是把 Scheme 语言嵌入到 Netscape Navigator 浏览器当中。
- 网景决定发明一种与 Java 搭配使用的辅助脚本语言并且语法上有些类似，这个决策导致排除了采用现有的语言，例如 Perl、Python、Tcl 或 Scheme。为了在其他竞争提案中捍卫 JavaScript 这个想法，公司需要有一个可以运作的原型。
- 布兰登·艾克在 1995 年 5 月仅花了十天时间就把原型设计出来了。

#### 设计思路

> 1. 借鉴 C 语言的基本语法；
> 2. 借鉴 Java 语言的数据类型和内存管理；
> 3. 借鉴 Scheme 语言，将函数提升到"第一等公民"（first class）的地位；
> 4. 借鉴 Self 语言，使用基于原型（prototype）的继承机制。

所以，Javascript 语言实际上是两种语言风格的混合产物----（简化的）函数式编程+（简化的）面向对象编程。这是由 Brendan Eich（函数式编程）与网景公司（面向对象编程）共同决定的。

#### JS 的命名

- Mocha（摩卡）=> LiveScript => JavaScript
- 浏览器一开始同时支持 Java 和 JavaScript
- 后来，JS 在浏览器中赢得了胜利

### 浏览器大战

#### 微软的跟进

- 1996 年 8 月 IE3 发布，支持 JScript(微软实现的 JS)
- 浏览器大战开始，每家浏览器的支持都不太一样

#### 网景的反击

- 1996 年 11 月，网景向 ECMA 提交语言标准，由于版权问题，JS 语言标准不叫 JavaScript，而叫 ECMAScript。

### 网景之死（被收购）

- 微软的 IE 浏览器由于捆绑到 Windows，很快就超越了网景浏览器
- 1998 年，网景浏览器节节败退，公司陷入了内忧外患
- 同年，公司打算将浏览器开源，来提升占有率
- 然而，市场并没有因为开源而重新青睐网景
- 年底，有过在线 AOL 宣布收购网景
- 收购后，网景团队的程序员纷纷被解雇
- 布兰登·艾克之后一直维护着 Firefox，到 2014 年为止。

部分资料来源： &copy;饥人谷和阮一峰的[Javascript 诞生记](http://www.ruanyifeng.com/blog/2011/06/birth_of_javascript.html)
