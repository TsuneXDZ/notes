# Java 广度

- #### 前端部分

  - 前端三剑客
  - VUE
  - Axios



# 协议：

### 三层模型 controller service Mapper

![image-20230504001742963](C:\Users\mycen\AppData\Roaming\Typora\typora-user-images\image-20230504001742963.png)

### 会话：

1、cookie （快速、低耗、不安全、无法跨域

2、session（安全、负载高、服务器集群无法使用

3、令牌 （json web token https://jwt.io) head部分使用base64编码 有效载荷 数字签名 令牌生成 令牌校验

### - Filter（过滤器  拦截所有请求

### - Intercepter(拦截器)  只拦截spring环境

执行流程，浏览器 -> Filter -> DispatcherServlet -> Intercepter -> Controller 

![image-20230504000831638](C:\Users\mycen\AppData\Roaming\Typora\typora-user-images\image-20230504000831638.png)

### AOP 面向切面编程 @Aspect

**AOP的执行流程**

为每个切入点创建一个代理对象，在执行原始方法时调用。

![image-20230504003055500](C:\Users\mycen\AppData\Roaming\Typora\typora-user-images\image-20230504003055500.png)



