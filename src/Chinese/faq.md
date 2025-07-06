# 常见问题

## 这本书什么时候完成？
Soon™。开源是一份礼物；所以随时我想写的时候。

## 如何扩展大型应用程序？
您将应用程序拆分为多个屏幕，然后使用简单的组合。

[袖珍指南] 有[一个展示这种方法的特定部分](https://docs.rs/iced/0.13.1/iced/#scaling-applications)。

## 我的应用程序如何从通道接收更新？
您可以使用 [`Task::run`] 从异步 [`Stream`] 生成消息。

或者，如果您控制通道的创建；您可以使用 [`Subscription::run`]。

[袖珍指南]: https://docs.rs/iced/0.13.1/iced/index.html#the-pocket-guide
[`Task::run`]: https://docs.rs/iced/0.13.1/iced/task/struct.Task.html#method.run
[`Subscription::run`]: https://docs.rs/iced/0.13.1/iced/struct.Subscription.html#method.run
[`Stream`]: https://docs.rs/futures/latest/futures/stream/trait.Stream.html

## Iced 支持从右到左的文本和/或 CJK 脚本吗？
还不是很好！

您可能能够使用带有 [`Shaping::Advanced`] 的 [`Text::shaping`] 渲染一些脚本，
但这些脚本的文本编辑尚不受支持；[输入法编辑器] 也不受支持。

不过，这些功能在 [`ROADMAP`] 中！

[`Text::shaping`]: https://docs.rs/iced/0.13.1/iced/widget/text/type.Text.html#method.shaping
[`Shaping::Advanced`]: https://docs.rs/iced/0.13.1/iced/widget/text/enum.Shaping.html#variant.Advanced
[输入法编辑器]: https://en.wikipedia.org/wiki/Input_method
[`ROADMAP`]: https://whimsical.com/roadmap-iced-7vhq6R35Lp3TmYH4WeYwLM

## `view` 和 `subscription` 函数何时被调用？
在每批消息和 `update` 调用之后。但这是一个实现细节；
永远不应该依赖这一点。

尝试将这些函数视为声明性的、无状态的函数。

## Iced 一直在重绘吗？！
是的！iced 目前在每个运行时事件后重绘；包括微小的鼠标移动。

有计划通过检测组件状态变化来减少重绘频率，但性能到目前为止还不是优先考虑的。

渲染器确实执行了相当多的缓存；所以重绘相当便宜。因此，
对于大多数用例来说，这很少是问题！

## 我收到一个恐慌，说没有运行反应器。这是怎么回事？
您可能正在使用 `Task` 来执行需要 `tokio` 执行器的 `Future`：

```text
there is no reactor running, must be called from the context of a Tokio 1.x runtime
```

您应该能够通过在 `iced` crate 中启用 [the `tokio` feature flag] 来解决这个问题：

```toml
iced = { version = "0.13", features = ["tokio"] }
```

[the `tokio` feature flag]: https://docs.rs/crate/iced/latest/features#tokio