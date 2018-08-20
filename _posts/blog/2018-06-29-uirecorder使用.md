---
layout:  post
title: uirecorder自动化测试
category: blog
description: 过程记录
---


1、npm  
下载安装包进行安装，配置系统环境  
全局安装uirecorder和mocha模块>npm install uirecorder mocha -g  
卸载模块>npm uninstall uirecorder mocha -g  
查看所有全局安装的模块>npm list --depth=0 -global  
查看全局安装路径>npm config get prefix  
查看全局安装目录>npm root -g  
修改npm的缓存目录>npm config set cache "XXX\nodejs\node_cache"  
修改npm的模块安装目录>npm config set prefix "XXX\nodejs\node_global"

步骤：  
1、下载nodejs进行安装  
2、配置系统环境变量path增加你的nodejs安装目录。这个是node的配置  
3、通过命令行修改npm的缓存目录与模块安装目录  
4、配置系统环境变量path增加你的nodejs\node_global模块安装目录  