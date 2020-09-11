# miniprogram-lifesaver

Todo:
1. `--variable: env(safe-area-inset-bottom)` 的非常规表现
2. 根级滚动容器与 `<scroll-view/>` 的优劣对比
3. [iOS]() `<scroll-view/>` 特定情况下卡死
4. `<scroll-view/>` 自动滚动功能特定情况下偶现失效
5. [iOS][2] `<video/>` 同层渲染失败条件与监听处理
   6. [Android]() 竖屏视频横向拉伸 (metadata width 属性错误)
7. 使用 custom tabbar 时的性能差异、created 预先触发逻辑、维度数据获取
8. 使用纯 CSS 逻辑来计算与给页面制造 custom tabbar 的预留空间以及相比之下使用 `getboundingClientRect` API 的劣势
9. [iOS][4] CSS `transition-delay` 偶现闪烁
10. `<input/>` 激活时滚动试图 placeholder 偏移
11. `<input/>` autofocus 功能在页面初始加载之后偶现失效
12. [iOS][5] `<input/>` 激活时特殊情况下内部区域可滚动
13. `wx.onKeyboardHeightChange` 在键盘收起时不触发
14. `styleIsolation: shared` 时 `externalClasses` 属性无效
15. [Android]() 数字键盘触发唤起系统原生键盘导致键盘重叠
16. `virtualHost: true` 时组件不保留实例
17. `this.animate` 单独动画某些属性无效（如 `background-color`）
18. `font-family` 不全导致中英文混杂时的字重异常
19. 动态的阻止事件冒泡与默认行为
20. [Android]() 三星 Samsung 机型 `windowHeight` 异常
21. [iOS][8] Flexbox 部分表现反常
22. `<image/>` `widthFix` 与 `heightFix` 模式性能较差，或导致图片拉伸闪烁
23. [iOS]() 在 `<page-meta/>` 中动态改变 `<navigation-bar/>` 的 `front-color` 属性在 iOS 7.0.15 以及之后的版本中不生效
24. Video 组件自动横屏导致在安卓上该页面横屏
25. iOS 14 Video 组件可被 overflow 滚动
26. CSS pointer-events: none 作用于安卓端的原生组件无效

[2]:	%20%20
[4]:	%20
[5]:	%20
[8]:	%20
