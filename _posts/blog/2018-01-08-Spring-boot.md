---
layout:     post
title: 开始spring boot
category: blog
description: 初步了解
---
 
 网上有很多教程，一步步带你创建一个Springboot项目
 
 启动报错
建好的Springboot工程需要我们引入相应的依赖。
会 自动配置数据库，启动报错，需要我们禁用自动引入
> @SpringBootApplication(exclude = {
> 		DataSourceAutoConfiguration.class,
> 		DataSourceTransactionManagerAutoConfiguration.class,
> 		HibernateJpaAutoConfiguration.class})
