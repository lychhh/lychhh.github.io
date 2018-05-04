---
layout:  post
title: 记录一些知识
category: dump
description: 积累
---



1、ArrayList与Vector的区别

同：

都是继承自List

异：

ArrayList在内存不够时默认是扩展50% + 1个，Vector是默认扩展1倍

Vector提供indexOf(obj, start)接口，ArrayList没有

Vector属于线程安全级别的，但是大多数情况下不使用Vector，因为线程安全需要更大的系统开销


2、Stack继承于Vector