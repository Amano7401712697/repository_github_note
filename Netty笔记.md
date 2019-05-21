# Netty笔记

## WebSocket支持

​	在从标准的HTTP或者HTTPS协议切换到WebSocket时， 将会使用一种称为升级握手的机制。因此，使WebSocket的应用程序将始终以HTTP/S作为开始，然后再执行升级。 这个升级动作发生的确切时刻特定于应用程序；它可能会发生在启动时，也可能会发生在请求了某个特定的URL之后。 

​	**ep：**如果被请求的 URL 以/ws 结尾，那么我们将会把该协议升级为 WebSocket； 否则，服务器将使用基本的 HTTP/S。在连接已经升级完成之后，所有数据都将会使用 WebSocket 进行传输。 

![1554172635060](C:\Users\imwf\AppData\Roaming\Typora\typora-user-images\1554172635060.png)

