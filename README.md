# react-track

基于React的声明式PV及用户行为采集框架。

## 为什么自研

在NPM上有若干个类似的包，但它们存在着一些缺陷，这其中主流的两类是：

- [react-tracking](https://www.npmjs.com/package/react-tracking)：偏向于声明式，但使用HOC的形式限制了使用的场景，且通过拦截类方法而臧`props`来进行数据的采集，与React的数据流形式略有不和。
- [react-tracker](https://www.npmjs.com/package/react-tracker)：采用类似`react-redux`的思想，使用`connect`和`Provider`的形式将功能联系起来。但是这种做法更偏向于命令式，从使用的角度来说繁琐之余也不易追踪。

除此之外，这些包均没有提供PV采集的能力。而PV采集中，有一个非常关键的问题至今没有得到很好的解决：

> 当URL中包含参数时，如`/posts/123`与`/posts/456`在PV上会被认为是两个页面，但事实上它们对应的路由均是`/posts/:id`，是相同的。

这一问题导致如果需要将包含参数的URL进一步的汇总与分组来更精确地计算“页面”的PV，则会需要额外的数据分析成本。因此我们希望从源头，即在数据采集的时候就解决这一问题，这也导致需要与`react-router`进行关联。

我们的目标是：

- 使用声明式的形式进行数据采集。
- 与React的组件树结构进行整合，可在JSX中形象地表达。
- 提供PV采集的能力，且**能够获取`react-router`的配置**。
- 尽可能小的移除成本，当一个行为或页面PV不再需要采集时，可以用最简单的手段移除而不影响已有组件的逻辑。
