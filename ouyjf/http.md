- 1
    - [请说说你对http中keep-alive的理解](https://github.com/haizlin/fe-interview/issues/5074)
    - http的keep-alive是干什么的？
        在早期–http1.0的协议都是建立在TCP协议基础上，其特点就是传输完数据后，立马就释放掉该TCP链接，也就是短连接。
        随着技术的发展，一个网页需要建立很多次短连接，这大大影响了消息的处理，所以http就提出了持续连接的概念，也就是让连接保存一段时间，后续的http消息可以复用这个连接继续传输消息，也就是Keep-Alive模式。
        当使用Keep-Alive模式（又称持久连接、连接重用）时，Keep-Alive功能使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。
        备注：持久连接的主要好处是避免了短连接的每次连接的三次握手和四次挥手的网络交互。
    - http的keep-alive如何使用？
        在http协议的Header头，有两个Tag可以控制这个keep-alive，Connection: Keep-Alive 和 Keep-Alive:timeout，它们表示的是保持持续连接状态的时间为timeout秒。
        http 1.0中默认是关闭的，需要在http头加入"Connection: Keep-Alive"，才能启用Keep-Alive；
        http 1.1中默认启用Keep-Alive，如果加入"Connection: close "，才关闭。
        目前大部分浏览器都是用http1.1协议，也就是说默认都会发起Keep-Alive的连接请求了，所以是否能完成一个完整的Keep-Alive连接就看服务器设置情况。
    - http如何区分多个请求, 如何知道当前请求已经结束？
        Content-Length
            当浏览器请求的是一个静态资源时，即服务器能明确知道返回内容的长度时，可以设置Content-Length来控制请求的结束
        Transfer-Encoding
            表示传输编码。
            还有一个类似的字段叫做：Content-Encoding。区别是Content-Encoding用于对实体内容的压缩编码（Content-Encoding: gzip）； Transfer-Encoding则改变了报文的格式。
            当服务端无法知道实体内容的长度时，可指定Transfer-Encoding:chunked (还可同时指定Transfer-Encoding: gzip)，表明实体内容数据不仅是gzip压缩的，还是分块传递的。最终当浏览器接收到一个长度为0的chunked时， 标识当前请求内容已全部接收。