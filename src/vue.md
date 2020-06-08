#### mvvm ####

**nextTick**
>浏览器包含了浏览器主进程，第三方插件进程和GPU进程（浏览器渲染进程），其中gpu进程包括了
    - gui渲染线程
    - js引擎线程 
    - 事件触发线程（和eventLoop密切相关）
    - 定时触发器线程
    - 异步http请求线程
GUI渲染线程和js引擎线程是互斥的，其中一个执行另一个就会被挂起
和vue的nexttick相关的是js引擎线程和事件触发线程。
**双向绑定proxy和defineProperty**
proxy应该被称为*代理*，而非*劫持*，是Object.defineProperty的加强版