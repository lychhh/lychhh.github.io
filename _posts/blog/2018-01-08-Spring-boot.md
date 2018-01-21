---
layout:     post
title: 开始spring boot
category: blog
description: 初步了解
---
 
 网上有很多教程，一步步带你创建一个Springboot项目
 
 启动报错
 
项目会自动配置数据库，启动报错，需要我们禁用自动配置数据库
> @SpringBootApplication(exclude = {
> 		DataSourceAutoConfiguration.class,
> 		DataSourceTransactionManagerAutoConfiguration.class,
> 		HibernateJpaAutoConfiguration.class})

 
 默认配置
* application.properties
> server.context-path=/helloboot 
> server.port=8081

* application.yml

> server:  
>   port: 8090  
>   session-timeout: 30  
>   tomcat.max-threads: 0  
>   tomcat.uri-encoding: UTF-8 
