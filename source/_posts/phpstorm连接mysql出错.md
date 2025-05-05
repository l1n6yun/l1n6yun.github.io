title: phpstorm连接mysql出错
categories: []
tags: 
 - PHPStorm
 - MySQL
 - JDBC
 - 配置
 - 时区支持
date: 2018-04-11 14:47:00
---

错误代码

```
java.lang.RuntimeException: com.mysql.cj.exceptions.InvalidConnectionAttributeException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
  at com.mysql.cj.exceptions.ExceptionFactory.createException(ExceptionFactory.java:61)
  at com.mysql.cj.exceptions.ExceptionFactory.createException(ExceptionFactory.java:85)
  at com.mysql.cj.util.TimeUtil.getCanonicalTimezone(TimeUtil.java:132)
  at com.mysql.cj.protocol.a.NativeProtocol.configureTimezone(NativeProtocol.java:2241)
  at com.mysql.cj.protocol.a.NativeProtocol.initServerSession(NativeProtocol.java:2265)
  at com.mysql.cj.jdbc.ConnectionImpl.initializePropsFromServer(ConnectionImpl.java:1319)
  at com.mysql.cj.jdbc.ConnectionImpl.connectWithRetries(ConnectionImpl.java:868)
  at com.mysql.cj.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:830)
  at com.mysql.cj.jdbc.ConnectionImpl.&lt;init&gt;(ConnectionImpl.java:455)
  at com.mysql.cj.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:240)
  at com.mysql.cj.jdbc.NonRegisteringDriver.connect(NonRegisteringDriver.java:199)
  at com.intellij.database.remote.jdbc.impl.RemoteDriverImpl.connect(RemoteDriverImpl.java:41)
  at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
  at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
  at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
  at java.lang.reflect.Method.invoke(Method.java:498)
  at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:357)
  at sun.rmi.transport.Transport$1.run(Transport.java:200)
  at sun.rmi.transport.Transport$1.run(Transport.java:197)
  at java.security.AccessController.doPrivileged(Native Method)
  at sun.rmi.transport.Transport.serviceCall(Transport.java:196)
  at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:573)
  at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:834)
  at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:688)
  at java.security.AccessController.doPrivileged(Native Method)
  at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:687)
  at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
  at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
  at java.lang.Thread.run(Thread.java:748) (no stack trace).
com.mysql.cj.exceptions.InvalidConnectionAttributeException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
```

解决方法

在 `advanced` 选项中找到 `serverTimezone` 设置为 `UTC`