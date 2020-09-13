# miniprogram-lifesaver

1. **CSS 变量 `env(safe-area-inset-bottom)` 的非常规表现**

	在 Android 平台中，该变量并非不支持，而是为 0。这意味着你无法使用 `@supports (padding-bottom: env(safe-area-inset-bottom))` 这样的 CSS 特性检测功能以及 `env(safe-area-inset-bottom, 20px)` 这样的CSS 变量 fallback 功能。
	为什么需要这些功能？ 有时你需要在 `env(safe-area-inset-bottom)` 值的基础上进行修改（比如 `env(safe-area-inset-bottom) - 10px` ）从而得出另一个 CSS 变量。可是在 Android 平台中该变量将无法避免的成为最终表现为 `-10px` 的变量，这将对 UI 的适配造成影响。CSS `max()` 或者 `min()` 仅支持 iOS 平台的兼容局限性也使得它在这个场景中无法发挥作用。

Todo:

1. 根级滚动容器与 `<scroll-view/>` 的优劣对比
2. _iOS_ `<scroll-view/>` 特定情况下卡死
3. `<scroll-view/>` 自动滚动功能特定情况下偶现失效
4. _iOS_ `<video/>` 同层渲染失败条件与监听处理
   5. _Android_ 竖屏视频横向拉伸 (metadata width 属性错误)
6. 使用 custom tabbar 时的性能差异、created 预先触发逻辑、维度数据获取
7. 使用纯 CSS 逻辑来计算与给页面制造 custom tabbar 的预留空间以及相比之下使用 `getboundingClientRect` API 的劣势
8. _iOS_ CSS `transition-delay` 偶现闪烁
9. `<input/>` 激活时滚动试图 placeholder 偏移
10. `<input/>` autofocus 功能在页面初始加载之后偶现失效
11. _iOS_ `<input/>` 激活时特殊情况下内部区域可滚动
12. `wx.onKeyboardHeightChange` 在键盘收起时不触发
13. `styleIsolation: shared` 时 `externalClasses` 属性无效
14. _Android_ 数字键盘触发唤起系统原生键盘导致键盘重叠
15. `virtualHost: true` 时组件不保留实例
16. `this.animate` 单独动画某些属性无效（如 `background-color`）
17. `font-family` 不全导致中英文混杂时的字重异常
18. 动态的阻止事件冒泡与默认行为
19. _Android_ 三星 Samsung 机型 `windowHeight` 异常
20. _iOS_ Flexbox 部分表现反常
21. `<image/>` `widthFix` 与 `heightFix` 模式性能较差，或导致图片拉伸闪烁
22. _iOS_ 在 `<page-meta/>` 中动态改变 `<navigation-bar/>` 的 `front-color` 属性在 iOS 7.0.15 以及之后的版本中不生效
23. Video 组件自动横屏导致在安卓上该页面横屏
24. iOS 14 Video 组件可被 overflow 滚动
25. CSS pointer-events: none 作用于安卓端的原生组件无效