---  
layout: post  
title: "Spring-RabbitMQ"  
date: 2015-08-15 15:19:01  
categories: spring  
---  


##一、安装
***

###1、安装ncurses-devel
***
安装前，首先查看本地ncurses版本，安装的ncurses-devel版本要与本地ncurses版本对应。

查看本地ncurses版本：

```xml
    <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx" 
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:rabbit="http://www.springframework.org/schema/rabbit"
	xsi:schemaLocation="  
		http://www.springframework.org/schema/beans   
	   	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       	http://www.springframework.org/schema/context 
       	http://www.springframework.org/schema/context/spring-context-4.0.xsd
       	http://www.springframework.org/schema/tx 
      	 http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
       	http://www.springframework.org/schema/aop 
       	http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
       	http://www.springframework.org/schema/rabbit
       	http://www.springframework.org/schema/rabbit/spring-rabbit-1.4.xsd">
	
	<rabbit:connection-factory id="connectionFactory"
	host="10.73.50.38" port="5672" username="pki402" password="pki402dcs"
	virtual-host="/" />
	
	<rabbit:template id="amqpTemplate" connection-factory="connectionFactory"
    exchange="commandExchange" />
    
	<rabbit:admin connection-factory="connectionFactory" />
	
	<!-- 定义queue，Exchange中绑定queue和routekey -->
	<rabbit:queue name="commandQueue" />
	
	<rabbit:topic-exchange name="commandExchange">
		<rabbit:bindings>
			<rabbit:binding queue="commandQueue" pattern="command.*" />
		</rabbit:bindings>
	</rabbit:topic-exchange>
	
	<rabbit:listener-container connection-factory="connectionFactory" concurrency="10" > 
    	<rabbit:listener ref="commandListener" queue-names="commandQueue" />
	</rabbit:listener-container>
	
</beans>   
```

安装方式一，通过rpm安装文件安装ncurses-devel：

    rpm -ivh ncurses-devel-5.7-3.20090208.el6.i686.rpm    
    
安装方式二，通过yum安装文件安装ncurses-devel：

    yum install ncurses-devel
    
###2、安装Erlang

下载地址：[http://www.erlang.org/download.html](http://www.erlang.org/download.html)

    wget http://www.erlang.org/download/otp_src_18.0.tar.gz //下载Erlang安装包
    tar -zxf otp_src_18.0.tar.gz    //解压安装包
    cd otp_src_18.0 //进入安装路径
    ./configure  //如果报错，安装NCURSES
    make    
    make install

###3、安装RabbitMQ
***
下载地址：[http://www.rabbitmq.com/](http://www.rabbitmq.com/)

    wget http://www.rabbitmq.com/releases/rabbitmq-server/current/rabbitmq-server-3.5.4-1.noarch.rpm //下载RabbitMQ安装包
    rpm -ivh --nodeps rabbitmq-server-3.5.4-1.noarch.rpm

 注意：如果直接用rpm -i rabbitmq-server-3.5.4-1.noarch.rpm，会提示缺少erlang 13B的环境，前面安装了erlang 18所以加上--nodeps则能安装成功。
 
##二、使用
***
启动

    rabbitmq-server start
    
停止

    rabbitmq-server stop
    
添加到启动项

    chkconfig rabbitmq-server on
    
开启管理页面

    rabbitmq-plugins enable rabbitmq_management
    
访问管理页面，默然账号guest，默认密码guest。

    http://localhost:15672/

##三、其它

###配置YUM源
***
下载地址：[http://mirrors.163.com/.help/centos.html](http://mirrors.163.com/.help/centos.html)

备份源文件，下载源文件，修改源文件中的版本号。

    yum clean all
    yum makecache






