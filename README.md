# 跨iframe通信的demo

> [window.postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

## 语法

```js
// 发送消息
otherWindow.postMessage(message, targetOrigin, [transfer]);
```

- otherWindow 其他窗口的一个引用，比如iframe的contentWindow属性、执行window.open返回的窗口对象、或者是命名过或数值索引的window.frames。
- message 将要发送到其他 window的数据。它将会被结构化克隆算法序列化。这意味着你可以不受什么限制的将数据对象安全的传送给目标窗口而无需自己序列化。
- targetOrigin 通过窗口的origin属性来指定哪些窗口能接收到消息事件，其值可以是字符串"*"（表示无限制）或者一个URI。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。这个机制用来控制消息可以发送到哪些窗口；例如，当用postMessage传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的信息的预期接受者的origin属性完全一致，来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个有确切值的targetOrigin，而不是*。不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点。
- transfer 可选,是一串和message 同时传递的 Transferable 对象. 这些对象的所有权将被转移给消息的接收方，而发送一方将不再保有所有权。

```js
// 接受消息
window.addEventListener("message", receiveMessage, false);

function receiveMessage(event)
{
  // For Chrome, the origin property is in the event.originalEvent
  // object.
  // 这里不准确，chrome没有这个属性
  // var origin = event.origin || event.originalEvent.origin;
  var origin = event.origin
  if (origin !== "http://example.org:8080")
    return;

  // ...
}

```

message 的属性有:

- data 从其他 window 中传递过来的对象。
- origin 调用 postMessage  时消息发送方窗口的 origin . 这个字符串由 协议、“://“、域名、“ : 端口号”拼接而成。例如 “https://example.org (隐含端口 443)”、“http://example.net (隐含端口 80)”、“http://example.com:8080”。请注意，这个origin不能保证是该窗口的当前或未来origin，因为postMessage被调用后可能被导航到不同的位置。
- source 对发送消息的窗口对象的引用; 您可以使用此来在具有不同origin的两个窗口之间建立双向通信。

## 使用

```sh
cd ifr-parent
yarn 
yarn serve
```

```sh
cd ifr-child
yarn
yarn start
```

浏览器打开 `http://localhost:8080`
