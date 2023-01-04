# Tomcat

[TOC]

## <u>文档标识</u>

| 文档名称 | Tomcat   |
| -------- | -------- |
| 版本号   | <V1.0.0> |

## <u>文档修订历史</u>

| 版本   | 日期      | 描述   | 文档所有者 |
| ------ | --------- | ------ | ---------- |
| V1.0.0 | 2022.12.5 | create | 杨丝雨     |
|        |           |        |            |
|        |           |        |            |

## <u>端口规划</u>

| 端口 | 协议 | remrks                                                       |
| ---- | ---- | ------------------------------------------------------------ |
| 8080 | tcp  | 默认的http监听端口                                           |
| 8005 | tcp  | 关闭tomcat进程所用                                           |
| 8009 | tcp  | httpd等反向代理tomcat时就可以使用使用ajp协议反向代理到该端口 |
| 8443 | tcp  | 默认的https监听端口                                          |

## <u>相关文档参考</u>

[Tomcat官网]: https://tomcat.apache.org/


## Tomcat 简介

------

   Tomcat是开源的 Java Web 应用服务器，实现了 Java EE(Java Platform Enterprise Edition)的部 分技术规范，比如 Java Servlet、Java Server Page、JSTL、Java WebSocket。Java EE 是 Sun 公 司为企业级应用推出的标准平台，定义了一系列用于企业级开发的技术规范，除了上述的之外，还有 EJB、Java Mail、JPA、JTA、JMS 等，而这些都依赖具体容器的实现。

### 其他Web服务器

------

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/9/25/16d68afe437359cc~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.image)

### 目录结构

------

**/ bin**：Startup, shutdown和其他脚本。windows为`*.bat`文件，linux为 `*.sh`文件。

**/ conf**：配置文件和相关的DTDs。这里最重要的文件是server.xml。它是容器的主要配置文件。

**/ logs**：日志文件默认位于此处。

**/ webapps**：这是您的webapp所在的位置。

### 工作流程

------

   当客户请求某个资源时，Servlet 容器使用 ServletRequest 对象把客户的请求信息封装起 来，然后调用 Java Servlet API 中定义的 Servlet 的一些生命周期方法，完成 Servlet 的执行， 接着把 Servlet 执行的要返回给客户的结果封装到 ServletResponse 对象中，最后 Servlet 容 器把客户的请求发送给客户，完成为客户的一次服务过程。

### 组织架构

------

Tomcat是一个基于组件的服务器，它的构成组件都是可配置的，其中最外层的是Catalina servlet容器，其他组件按照一定的格式要求配置在这个顶层容器中。

Tomcat的各种组件都是在Tomcat安装目录下的/conf/server.xml文件中配置的。

```xml
<Server>                                     //顶层类元素，可以包括多个Service   
    <Service>                                //顶层类元素，可包含一个Engine，多个Connecter
        <Connector>                          //连接器类元素，代表通信接口
                <Engine>                     //容器类元素，为特定的Service组件处理客户请求，要包含多个Host
                        <Host>               //容器类元素，为特定的虚拟主机组件处理客户请求，可包含多个Context
                                <Context>    //容器类元素，为特定的Web应用处理所有的客户请求
                                </Context>
                        </Host>
                </Engine>
        </Connector>
    </Service>
</Server>
```

tomcat的体系结构如下：

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/9/25/16d68b04783ec00a~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.image)

由上图可看出Tomca的心脏是两个组件：Connecter和Container。一个Container可以选择多个Connecter，多个Connector和一个Container就形成了一个Service。Service可以对外提供服务，而Server服务器控制整个Tomcat的生命周期。

### 容器

------

Servlet容器处理客户端的请求并填充response对象。Servlet容器实现了Container接口。在Tomcat中有4种级别的容器：Engine，Host，Context和Wrapper。

Engine：表示整个Catalina Servlet引擎；

Host：包含一个或多个Context容器的虚拟主机；

Context：表示一个Web应用程序，可以包含多个Wrapper；

Wrapper：表示一个独立的Servlet；

4个层级接口的标准实现分别是：StandardEngine类，StandardHost类，StandardContext类和StandardWrapper类。它们在org.apache.catalina.core包下。

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/9/25/16d68b07cda037d6~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.image)

## Tomcat版本

| **Servlet规范** | **JSP规范** | **EL规格** | **WebSocket规范** | **身份验证 (JASPIC) 规范** | **Apache Tomcat 版本** | **最新发布版本**  | **支持的 Java 版本**                            |
| :-------------- | :---------- | :--------- | :---------------- | :------------------------- | :--------------------- | :---------------- | :---------------------------------------------- |
| 6.0             | 3.1         | 5.0        | 2.1               | 3.0                        | 10.1.x                 | 10.1.2            | 11 岁及以后                                     |
| 5.0             | 3.0         | 4.0        | 2.0               | 2.0                        | 10.0.x（已取代）       | 10.0.27（已取代） | 8 及以后                                        |
| 4.0             | 2.3         | 3.0        | 1.1               | 1.1                        | 9.0.x                  | 9.0.69            | 8 及以后                                        |
| 3.1             | 2.3         | 3.0        | 1.1               | 1.1                        | 8.5.x                  | 8.5.84            | 7 及以后                                        |
| 3.1             | 2.3         | 3.0        | 1.1               | 不适用                     | 8.0.x（已取代）        | 8.0.53（已取代）  | 7 及以后                                        |
| 3.0             | 2.2         | 2.2        | 1.1               | 不适用                     | 7.0.x（存档）          | 7.0.109（存档）   | 6 及更高版本 （对于 WebSocket 为 7 及更高版本） |
| 2.5             | 2.1         | 2.1        | 不适用            | 不适用                     | 6.0.x（存档）          | 6.0.53（存档）    | 5及以后                                         |
| 2.4             | 2.0         | 不适用     | 不适用            | 不适用                     | 5.5.x（存档）          | 5.5.36（存档）    | 1.4 及更高版本                                  |
| 2.3             | 1.2         | 不适用     | 不适用            | 不适用                     | 4.1.x（存档）          | 4.1.40（存档）    | 1.3 及更高版本                                  |
| 2.2             | 1.1         | 不适用     | 不适用            | 不适用                     | 3.3.x（存档）          | 3.3.2（存档）     | 1.1 及更高版本                                  |

## Tomcat安装

------

> Linux上安装tomcat的系统环境只需要安装好jdk即可，然后下载对应的tomcat即可，
> 官网地址:<http://tomcat.apache.org/>

![image-20221205111908945](../../../Library/Application Support/typora-user-images/image-20221205111908945.png)

### 上传文件到服务器

```shell
wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.84/bin/apache-tomcat-8.5.84.tar.gz
```

### 解压

```shell
tar xf apache-tomcat-8.5.84.tar.gz
```

### 启动

```shell
# 进入bin 目录

# 启动
./startup.sh
# 关闭
./shutdown.sh
```

### 创建Systemd服务单元

```shell
vim /etc/systemd/system/tomcat.service
[Unit]
Description=Tomcat 8.5 servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat
Environment="JAVA_HOME=/usr/local/java/1.8.0/"
ExecStart=/opt/tomcat/latest/bin/startup.sh
ExecStop=/opt/tomcat/latest/bin/shutdown.sh
ExecReload=/bin/kill -s HUP $MAINPID
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

```shell
sudo systemctl daemon-reload
sudo systemctl enable --now tomcat
sudo systemctl restart tomcat
sudo systemctl status tomcat
```

### 创建tomcat Web用户

```shell
vim /opt/tomcat/latest/conf/tomcat-users.xml

<tomcat-users>
<!--
    Comments
-->
   <role rolename="admin-gui"/>
   <role rolename="manager-gui"/>
   <user username="admin" password="admin_password" roles="admin-gui,manager-gui"/>
</tomcat-users>
```

默认情况下，Tomcat Web配置为仅允许从本地主机访问。如果您需要外部网络访问Web界面。

请打开/opt/tomcat/latest/webapps/manager/META-INF/context.xml文件并注释以下行。

通常，不建议从任何地方允许访问，因为这会带来安全风险。

```xml
<Context antiResourceLocking="false" privileged="true" >
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
```

如果您只想从指定IP访问Tomcat Web，则无需注释这些xml片段，而是将您的外网IP添加到列表中。

允许的IP地址列表是用竖线`|`分隔的列表。您可以添加单个IP地址或使用正则表达式。

假设您的公开IP为`41.41.41.41`，而您只想仅从IP访问Tomcat Web。完成后，重新启动Tomcat服务以使更改生效。

```xml
<Context antiResourceLocking="false" privileged="true" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|41.41.41.41" />
</Context>
```

/opt/tomcat/latest/webapps/manager/META-INF/context.xml

```bash
sudo systemctl restart tomcat
```

## Tomcat Yum安装

### 安装

1、命令

```shell
yum -y install tomcat
```

2、查询 tomcat 是否安装成功

```shell
rpm -q tomcat
```

3、信息查看

```shell
yum info tomcat
```

### 服务启动

1、启动

```shell
systemctl start tomcat.service
```

2、状态查看

```shell
systemctl status tomcat.service
```

### 安装管理包

安装Tomcat根页面（tomcat-webapps）和Tomcat Web应用程序管理器和Virtual Host Manager（tomcat-admin-webapps），请运行以下命令：

```shell
sudo yum install tomcat-webapps tomcat-admin-webapps
```

会安装如下的内容到 `/usr/share/tomcat/webapps` 文件夹下：

```shell
examples  host-manager  manager  ROOT  sample
```

重启服务：

```shell
sudo systemctl restart tomcat
```

## <u>附录：Tomcat安全配置与高并发优化</u>
[Tomcat配置优化]: https://blog.csdn.net/bluetjs/article/details/77449535



## <u>附录：Tomcat配置 SSL 证书</u>
[Tomcat配置SSL证书]: https://segmentfault.com/a/1190000009780545



## <u>附录：Tomcat配置基于域名和端口的虚拟主机</u>
基于域名的tomcat 虚拟主机

```shell
cd /opt/tomcat/config
cp server.xml server.xml_bak

#注意下面的内容要在<Engine>......</Engine> 之间添加 
<Engine>
  ......
  <Host name="www.21girl.cc"  appBase="/data/www" unpackWARs="true" autoDeploy="true">
            <Valve className="org.apache.catalina.valves.AccessLogValve" directory="/data/logs/"
            prefix="21girl_access_log." suffix=".txt"
            pattern="%h %l %u %t &quot;%r&quot; %s %b" />
            <Context path="" docBase="www.21girl.cc" />
  </Host>
</Engine>

## 项目说明：

name ：        需要配置的虚拟主机域名。 如：www.21girl.cc
appBase:       项目代码的父级目录.   如：/data/www/   下面会分目录存放不同工程代码  
unpackWARs：
autoDeploy：   自动部署，检测代码有变动直接刷新
directory：    项目的访问日志存放路径： 默认：tomcat/logs
prefix:        日志名称
suffix：     
pattern：      日志格式
path：       
docBase：      项目代码实际存放的目录，存在于appBase选项的目录之下. 如：  域名www.21girl.cc 的代码存放于/data/www/www.21girl.cc
               或者 对应项目文件夹或者项目的.war包 (如果是war包，就需要把unpackWARs设置为true)
```

基于端口的tomcat 虚拟主机
```shell

<server>
......

<Service name="Catalina2">
   <Connector port="8082" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
   <Engine name="Catalina2" defaultHost="bbs.21girl.cc">
     <Realm className="org.apache.catalina.realm.UserDatabaseRealm"  
            resourceName="UserDatabase"/>
     <Host name="bbs.21girl.cc"  appBase="/data/www" unpackWARs="true" autoDeploy="true"  
           xmlValidation="false" xmlNamespaceAware="false">
            <Valve className="org.apache.catalina.valves.AccessLogValve" directory="/data/logs/"
            prefix="bbs.21girl_access_log" suffix=".txt"
            pattern="%h %l %u %t &quot;%r&quot; %s %b" />
           <Context path="" docBase="bbs.21girl.cc" />
     </Host>
   </Engine>
 </Service>


# 以上部分
</server>

```