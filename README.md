# miniprogram-lifesaver


1. **_iOS_ `<video/>` 等原生组件同层渲染失败条件与监听处理**

	众所周知小程序实际上是一种 hybrid app，无法避免的涉及到 native 组件与 web 组件在同一视口中的叠加渲染。理论上 native 组件在垂直于屏幕的图层优先级上肯定是要高于 web 组件，因为 webview 本身就处于 native 应用提供的容器里。有关小程序可以实现在 hybrid 应用中实现不同视图层组件的同层渲染的技术细节介绍很少，这篇 [来自官方社区的文章][1] 也仅能解答部分疑惑。总结于这篇文章中的有价值的内容主要是：

	1. 尽量避免在支持同层渲染的原生组件上用 CSS 属性做影响文档结构的变化。
	2. `bind:rendererror` 事件是唯一可以用来监听同层渲染失败的事件，你可以用这个事件来做亡羊补牢式的体验优化，即错误已经发生，如何最大程度挽回已经丢失的用户体验？从我的个人经验来说可以做两点：1. 向后台发送错误统计信息；2. 用且仅能用 `wx.showModal` 方法触发原生 modal 组件向用户解释目前到底发生了什么并且提供按钮引导用户 `relaunch` 到当前页面来试探此次同层渲染错误是否为偶然发生（实际上的确有偶然发生的情况）。

	还有一点不成系统的个人经验是就减少原生组件在所在自定义组件的嵌套层级，或者干脆避免在自定义组件中使用原生组件而是直接在页面组件中直接使用，不然这会使同层渲染变得非常难以捉摸，即使是同一平台的不同机型，同一机型的不同元素尺寸… 都会成为导致同层渲染失败的因素。而发生了这种情况你也可以试试替换或去除一些对排版影响比较重的 CSS 属性（如将 `left` / `top` 替换为 `transform` 来达到相同的视觉效果）来努力争取一下好的结果。


2. **CSS 变量 `env(safe-area-inset-bottom)` 的非常规表现**

	在 Android 平台中，该变量并非不支持，而是为 0。这意味着你无法使用 `@supports (padding-bottom: env(safe-area-inset-bottom))` 这样的 CSS 特性检测功能以及 `env(safe-area-inset-bottom, 20px)` 这样的CSS 变量 fallback 功能。

	为什么需要这些功能？有时你需要在 `env(safe-area-inset-bottom)` 值的基础上进行修改从而得出另一个 CSS 变量，比如 `env(safe-area-inset-bottom) - 10px` 。可是在 Android 平台中该变量将无法避免的成为最终表现为 `-10px` 的变量，这将对 UI 的适配造成负面的影响。CSS `max()` 或者 `min()` 仅支持 iOS 平台的兼容局限性也使得它在这个场景中无法发挥作用。针对这个问题，我的解决方案是用 `min-height` CSS 属性来防止以上例子中的 `-10px` 对垂直方向上的布局造成负面影响，但应用的场景非常有限。


2. **_iOS_ CSS `transition-delay` 偶现闪烁**

	可以使用 CSS animation 创建空白的 keyframes 填充在真正要表现的动画之前来绕过该问题。

	但该解决方案的缺点是他无法从视觉上真实的表现出 easing-function 的结果，因为空白关键帧也属于动画的一部分，但这部分人眼不可见的动画却占用了 easing 效果的时间轴，使得真正要表现的动画却不是从 easing-function 的起始点按照曲线输出效果的，而是从中途开始。所以这就需要另外对 easing-function 进行调整，尽量弥补这一遗憾。


3. 声明 `virtualHost: true` 时自定义组件实例不具备被选择以及被观测的特性

	在 Component 的 Options 字段中声明 `virtualHost: true` 是为了使该组件更加轻量化，从而免去外部组件容器的无法直接作用于该组件内部根节点的麻烦。

	但官方文档中并没有注明的是，如果你使用了 `virtualHost` ，同时也就意味着你的组件将无法在被 `this.selectComponent` 获取到实例对象，也无法在 Devtool 的 Inspector 中被观测到组件内部的数据情况。


4. 声明 `styleIsolation: shared` 时 `externalClasses` 属性无效

	小程序对于组件样式的管理策略原本是一个不与外界产生任何作用的孤岛，但在自定义组件的 JSON 配置中声明 `styleIsolation: shared` 或者 `styleIsolation: apply-shared` 会对该组件以及外界的 CSS 作用域产生不同的影响。而仅当你使用 `shared` 去开启一个自定义组件最大程度的样式作用域时 `externalClasses` 所起到的自定义外部 class 名的作用是无效的。

Todo:

1. 根级滚动容器与 `<scroll-view/>` 的优劣对比
2. _iOS_ `<scroll-view/>` 特定情况下卡死
3. `<scroll-view/>` 自动滚动功能特定情况下偶现失效
   4. _Android_ 竖屏视频横向拉伸 (metadata width 属性错误)
5. 使用 custom tabbar 时的性能差异、created 预先触发逻辑、维度数据获取
6. 使用纯 CSS 逻辑来计算与给页面制造 custom tabbar 的预留空间以及相比之下使用 `getboundingClientRect` API 的劣势
7. `<input/>` 激活时滚动试图 placeholder 偏移
8. `<input/>` autofocus 功能在页面初始加载之后偶现失效
9. _iOS_ `<input/>` 激活时特殊情况下内部区域可滚动
10. `wx.onKeyboardHeightChange` 在键盘收起时不触发
11. _Android_ 数字键盘触发唤起系统原生键盘导致键盘重叠
12. `this.animate` 单独动画某些属性无效（如 `background-color`）
13. `font-family` 不全导致中英文混杂时的字重异常
14. 动态的阻止事件冒泡与默认行为
15. _Android_ 三星 Samsung 机型 `windowHeight` 异常
16. _iOS_ Flexbox 部分表现反常
17. `<image/>` `widthFix` 与 `heightFix` 模式性能较差，或导致图片拉伸闪烁
18. _iOS_ 在 `<page-meta/>` 中动态改变 `<navigation-bar/>` 的 `front-color` 属性在 iOS 7.0.15 以及之后的版本中不生效
19. Video 组件自动横屏导致在安卓上该页面横屏
20. iOS 14 Video 组件可被 overflow 滚动
21. CSS pointer-events: none 作用于安卓端的原生组件无效

[1]:	https://developers.weixin.qq.com/community/develop/article/doc/000c4e433707c072c1793e56f5c813