.. _dataShow_deploy:

第1章	部署环境
===========================
1.1	软硬件环境
--------------
 * 硬件
	主流PC级服务器即可，无特殊限制
 * 操作系统
	支持Java虚机即可，无特殊限制
 * JDK
	推荐使用Oracle JDK1.8。 兼荣JDK1.7
 * 浏览器
	仅支持现代浏览器，即：chrome、firefox、IE10以上

1.2	部署方式说明
-------------------
BICENTER-DataShow支持两种部署方式，对应两种部署包，都是以.zip格式发布，根据需求要选择按哪种方式部署。
 * 独立模式
可单独部署，作为一个Java进程启停。这种模式使用jetty内置容器方式启动，使用的安装包是“Bicenter_DataShow.zip”，解压以后如下：
  .. image :: _static/images/datashow/deploy/1.2.1.png 
 * 嵌入模式
可作为一个Servlet，嵌入其它于 JSP容器的应用。这种模式支持一般web应用服务器启动，例如tomcat，使用的安装包是“Bicenter_DataShow_war.zip”，解压以后文件文件如下：

  .. image :: _static/images/datashow/deploy/1.2.2.png 

第2章	独立部署
===========================

2.1	配置
-------------------
所有配置都在配置文件config.properties，需要配置如下几个方面
 * 访问的host和端口号（host，port）， 
 * 前端源文件存放目录（webcontent），
 * 报表文件存放目录（webroot）
 * 数据源，可配置多个数据源
例如：详细配置如下::

#############################
# nessensory setting
#############################
host=0.0.0.0              #部署服务器地址
port=8888					#端口号，任意指定（未被占用）
webroot=E:/datashow2.0/front/datashowfront  #前端项目地址，即部署包中webcontent文件夹坐在目录
workfolder=F:/tmp/boardbackend       #模板文件存放目录，即部署包中workroot文件夹所在目录；也可以任意指定，需要把workroot目录下的.js文件拷贝进去
##############################
#data source setting
##############################
#mysql数据源配置
itsm451.jdbc.driverClassName=com.mysql.jdbc.Driver
itsm451.jdbc.url=jdbc:mysql://192.168.9.115:3306/itsm_451?useUnicode=true&characterEncoding=utf8&autoReconnect=true
itsm451.jdbc.username=root
itsm451.jdbc.password=123456

#oracle数据源配置
orac.jdbc.url=jdbc:oracle:thin:@10.6.10.96:1521:oracl
orac.jdbc.username=oracl
orac.jdbc.password=oracl
sqlser.jdbc.url=jdbc:sqlserver://10.126.3.106:1433;databaseName=Northwind;SelectMethod=cursor

#sqlserver数据源配置
sqlser.jdbc.url=jdbc:sqlserver://10.126.3.106:1433;databaseName=Northwind;SelectMethod=cursor
sqlser.jdbc.username=sa
sqlser.jdbc.password=123456#orcle.jdbc.driverClassName=oracle.jdbc.driver.OracleDriver

#verbose only for inner simple logger
#off, error, warn, info, debug, all ;default info
log.verbose=debug



2.2	启动
-------------------
如果是Windows环境直接运行startup.bat，如果是Linux环境运行startup.sh。（注意Linux环境需要给startup.sh文件赋予可执行权限。前提是服务器安装并配置好java环境变量），启动之后使用http://[ip]:[port] 访问即可

第3章	嵌入模式
===========================
3.1	配置
-------------------
所有配置都在配置文件config.properties，相比独立部署，只需要配置
 * 报表文件存放目录（webroot）
例如：详细配置如下::

#############################
# nessensory setting
#############################
workfolder=F:/tmp/boardbackend       #模板文件存放目录，即部署包中workroot文件夹所在目录；也可以任意指定，需要把workroot目录下的.js文件拷贝进去
##############################
#data source setting
##############################
#mysql数据源配置
itsm451.jdbc.driverClassName=com.mysql.jdbc.Driver
itsm451.jdbc.url=jdbc:mysql://192.168.9.115:3306/itsm_451?useUnicode=true&characterEncoding=utf8&autoReconnect=true
itsm451.jdbc.username=root
itsm451.jdbc.password=123456

#oracle数据源配置
orac.jdbc.url=jdbc:oracle:thin:@10.6.10.96:1521:oracl
orac.jdbc.username=oracl
orac.jdbc.password=oracl
sqlser.jdbc.url=jdbc:sqlserver://10.126.3.106:1433;databaseName=Northwind;SelectMethod=cursor

#sqlserver数据源配置
sqlser.jdbc.url=jdbc:sqlserver://10.126.3.106:1433;databaseName=Northwind;SelectMethod=cursor
sqlser.jdbc.username=sa
sqlser.jdbc.password=123456#orcle.jdbc.driverClassName=oracle.jdbc.driver.OracleDriver

#verbose only for inner simple logger
#off, error, warn, info, debug, all ;default info
log.verbose=debug



3.2	集成其他应用
-------------------
将应用安装包\WEB-INF\lib目录下所有jar文件拷贝到本地应用的lib目录，将web.xml文件里的servlet配置加入本地的web.xml配置文件::

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>temp</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

#将以下sevlet加入本地web.xml配置文件
  <servlet>
  	<servlet-name>FileManageServlet</servlet-name>
  	<servlet-class>com.dcits.bicenter.backend.FileManageServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>FileManageServlet</servlet-name>
  	<url-pattern>*.ds</url-pattern>
  </servlet-mapping>  
</web-app>


3.3	启动
-------------------
直接启动部署的应用服务即可，例如使用tomcat部署，直接启动tomcat，启动以后使用http://[ip]:[port]/应用名称/datashow/index.html访问即可。

第4章	注册授权
===========================
访问BICENTER-DataShow首页，在右上角有个“授权”，点击以后将内容拷贝并发送给BICENTER项目组申请正式license，以便获得永久使用权。