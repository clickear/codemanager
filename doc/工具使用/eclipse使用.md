#官方文档
[http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.wst.webtools.doc.user%2Ftopics%2Fcpdjsps.html](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.wst.webtools.doc.user%2Ftopics%2Fcpdjsps.html "eclipse官方说明")
#创建动态web项目

1. 选择j2ee的工作窗口
![](http://i.imgur.com/nFMSQXZ.png)
2. 创建javaweb项目,在file-->new中选择danymic web project.
3. 填写项目名称,及web模块
3. 创建成功,工程目录如下

# danymic web project目录解析 #
src文件夹:存放Java源文件
WebRoot:Web应用的根目录
META-INF:系统自动生成，存放系统描述信息
WEB-INF:该目录中内容不能对外发布
lib文件夹:存放以jar/zip形式表现的库文件
web.xml:Web应用的初始化配置文件
## eclipse中跳转到tomcat源码部分 ##
## eclipse 中 j2ee源码 中关于tomcat 的 servlet包提示has no source attachment##
- 下载tomcat源码,解压缩.
- 添加源码路径,选择external location.

##eclipse中阅读tomcat源码(未完成,偏离主题) ##



- 下载源码。servlet应该是个接口,不同的的服务器实现的方式不同。
目前我们采用tomcat服务器,所以需要去tomcat官网下载对应版本的源码.
- 使用ant编译tomcat.下载ant,设置ant的环境变量.
- 进入tomcat源码路径,配置tomcat 文件。

>依赖包下载到什么地方。在Linux或MacOX下，会默认下载到 “/usr/share/java” 目录，当然该目录普通用户是没有权限写的；在Windows下，默认下载到 "某个磁盘:\usr\share\java" ，这的磁盘可能是C、D或其它，这一般取决于你把Tomcat源码放在哪个盘了，比如我的放在D盘，默认就下载在 "D:\usr\share\java" 下。如果我想自己定义下载路径怎么办？
如果用户是通过代理上网的，那么下载过程中就会出错。怎么解决？
熟悉ant的人应该知道怎么解决。就是通过配置文件build.properties来 设置。该配置文件在Tomcat源码路径下为 “build.properties.default” 。我们可以去掉.default后缀或直接新建一个build.properties都可以。当然我选择了前者。将 “build.properties.default” 修改为 “build.properties” 打开， 修改里面的base.path属性值为我们希望的下载路径并添加proxy代理配置，格式如下
    
    # ----- Proxy setup -----
    # Uncomment if using a proxy server
    proxy.host=proxy.domain
    proxy.port=8080
    proxy.use=on
    
    # ----- Default Base Path for Dependent Packages -----
    # Replace this path with the directory path where dependencies binaries
    # should be downloaded
    base.path=/home/me/some-place-to-download-to
    
>根据自己的需要进行设置，注意如果不需要某项设置需要用#注释掉。

- 使用ant 编译。
>依赖包下载成功后.如果下载失败可以多试几次.执行ant即可编译，编译成功后当前路径下回多出个output文件夹，就是我们的编译结果。
    ant 

- 对于编译中的错误
```
Source中的抽象方法getParentLogger()
    [javac] public class DriverAdapterCPDS
    [javac]        ^
    [javac] G:\temp_data\tomcat_packet\tomcat7-deps\dbcp\src\java\org\apache\tomcat\dbcp\dbcp\datasources\PerUserPoolDataSource.java:60: 错误: PerUserPoolDataSource不是抽象的, 并且未覆盖CommonDataSource中的抽象方法getParentLogger()
    [javac] public class PerUserPoolDataSource
    [javac]        ^
    [javac] G:\temp_data\tomcat_packet\tomcat7-deps\dbcp\src\java\org\apache\tomcat\dbcp\dbcp\datasources\SharedPoolDataSource.java:52: 错误: SharedPoolDataSource不是抽象的, 并且未覆盖CommonDataSource中的抽象方法getParentLogger()
    [javac] public class SharedPoolDataSource
    [javac]        ^
    [javac] 注: 某些输入文件使用或覆盖了已过时的 API。
    [javac] 注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
    [javac] 注: 某些输入文件使用了未经检查或不安全的操作。
    [javac] 注: 有关详细信息, 请使用 -Xlint:unchecked 重新编译。
    [javac] 15 个错误
    [javac] 1 个警告
```
>根据日志错误推测，依赖的dbcp相关的jar版本不对。本机安装的jdk版本是1.7的，于是在本机又装了一个1.6的jdk，并修改相关环境变量让JAVA_HOME指向jdk 1.6目录。再次执行ant成功。