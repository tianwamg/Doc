#### Tomcat解析

- HTTP服务器：接收并处理http请求；进行Socket通信(基于TCP/IP)，解析HTTP报文；
- Servlet容器：将请求转发具体的servlet处理；加载与管理Servlet，由Servlet负责处理request请求；

![image](https://user-images.githubusercontent.com/29136753/150928745-a0122e55-fbcc-4f05-af57-731a4c9238a6.png)


- Http处理过程：
  - 用户发起浏览器请求；
  - 由于HTTP协议底层数据传输使用TCP/IP协议，因此浏览器项服务端发出TCP链接请求；
  - 服务器接受连接请求，建立TCP三次握手连接；
  - 浏览器将请求数据打包成HTTP协议格式的数据包
  - 服务端获取数据包，根据HTTP协议格式进行解析；
  - 处理结果；
  - 将响应结果按照HTTP协议进行打包；
  - 服务器将数据包推入网络，经网络传输最终到达浏览器；
  - 浏览器获取数据包，按照HTTP协议谢谢；
  - 数据展示；
![image](https://user-images.githubusercontent.com/29136753/150928820-7f23bbf1-fdbb-4e35-8a78-646ab69f6ccd.png)


- servlet容器工作流程
  - HTTP服务器会把请求信息通过ServletRequest对象封装；
  - 进一步调用Servlet容器中某个具体的Servlet；
  - 在上一步中，根据URL与Servlet映射关系，找到相应的Servlet；
  - 如果Servlet还没有被加载，就使用反射机制创建，并调用init方法完成初始化；
  - 调用具体Servlet的service方法处理请求，并将结果使用ServletResponse对象封装；
  - 将ServletResponse对象返回给HTTP服务器，HTTP服务器将响应发生给客户端；
![image](https://user-images.githubusercontent.com/29136753/150928939-69f61c83-6ee0-4a8d-8bde-f3f10f80b1d7.png)


- Tomcat组件设计
  - 连接器Connector，负责完成HTTP服务器功能；
  - 容器Container，完成Servlet容器功能；
![image](https://user-images.githubusercontent.com/29136753/150929915-23e8bb1c-375c-4aa9-af6b-7803d033ac86.png)


  - service组件
    - server：Tomcat实例，包含了servlet容器及其它组件，负责组装并启动Servlet引擎、Tomcat连接器；
    - Service ：一个server包含多个service，将若干个Connector组件绑定到一个Container中；
    - Container：负责处理用户的Servlet请求，并返回对象给Web用户的模块；
  - 连接器
    - socket通信；解析处理应用层协议，封装Request对象；将Request对象转换为ServletRequest对象，Response对象转换为ServletResponse对象；
    - EndPoint：负责提供请求字节流给Processor；
    - Processor：负责提供Tomcat定义的Request对象给Adapter；
    - Adapter：负责提供标准的ServletRequest对着给Servlet容器；
![image](https://user-images.githubusercontent.com/29136753/150930027-9d54ddc3-db75-45f4-953b-6dfbe4eecd30.png)


  - 容器
    - Engine：表示整个Catalina的Servlet引擎，用来管理多个虚拟站点，一个Service最多有一个Engine，但是一个Engine可包含多个Host；
    - Host：代表一个虚拟主机，一个虚拟站点可包含多个Context；
    - Context：表示一个Web应用程序，一个Web应用程序可包含多个Wrapper；
    - Wrapper：表示一个Servlet，负责管理整个Servlet的生命周期，包括装载，初始化，资源回收等；
![image](https://user-images.githubusercontent.com/29136753/150930055-0f82ff7f-3508-4dc1-ba76-1af74634a29a.png)


  - Catalina
    - 负责解析Tomcat配置文件（Server.xml），以此创建服务器Server组件并进行管理；
- 总结
  - 整个Tomcat可以当做是一个Catalina实例，启动时会初始化这个实例，Catalina实例通过加载server.xml文件完成其它实例的创建，创建并管理一个server，Server创建并管理多个服务，每个服务Service又可以有多个Connector和一个Container；
 
 - [参考](https://www.jianshu.com/p/7c9401b85704?utm_campaign=haruki&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)
  
