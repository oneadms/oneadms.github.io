# Servlet

servlet的创建默认是tomcat接收到请求后创建 默认load-on-up属性为 -1  如果设置为>0的值后 则 tomcat启动后就进行加载 优先级从值小到大 小的优先级最高

servlet 的servletrequest 和servletresponse 由tomcat传入

serveltrequest传入的是httpservletrequest的实现类 requestforce

servlet里面内置了线程池 每接收一个请求则从线程池中取出一个线程进行servlet的处理

servlet只创建一次  

request和response 每一个请求都会创建一个

# cookie

可以通过setMaxage进行设置  单位/s  为

时则删除该cookie  -1则持续到浏览器关闭后 

httponly可以通过服务端设置cookie js无法操作它

## session 

概念:sessionid由第一次请求时服务端创建 通过set-cookie属性设置到客户端 然后客户端每次带着cookie发送请求  用来标识会话

会话劫持 

如果地址栏携带了sessionid 那么服务端将不会创建sessionid 而使用传入的sessionid 

原理

将携带sessionid的链接发给别人 别人打开后登录

session是唯一的 

session默认过期时间为30分钟 如果中途进行操作则自动续签

直到没有任何操作30分钟后 会话失效

setMaxInactiveInterval  设置会话过期时间 单位 /s

使此会话无效，然后解除绑定到它的任何对象。
抛出：
IllegalStateException – 如果在已经失效的会话上调用此方法

销毁会话

public void invalidate();



# servletcontext

```
由于servletcontext是一个全局对象，一个web应用里面只有一个servletcontext对象 无论通过那种方式获取 获取的都是同一个servletcontext
```

```
//    获取项目的上下文地址 即项目的部署名称
    System.out.println("contextPath = " + contextPath);
//    获取项目的真实路径
    String realPath = sc1.getRealPath("/");
    System.out.println("realPath = " + realPath);
```



 