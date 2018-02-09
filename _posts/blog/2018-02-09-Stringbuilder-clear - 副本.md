---
layout:  post
title: Stringbuilder清空效率
category: blog
description: 编写代码测试
---


> 		StringBuilder builder = new StringBuilder();
        long time = System.currentTimeMillis();
        for(int i=0;i<10000000;i++){
            builder = new StringBuilder();
            builder.append("aa");
            builder.append("bb");
            builder.append("cc");
            builder.append("dd");
            builder.append("ee");
        }
        System.out.println("new 耗时：" + (System.currentTimeMillis() - time));
        long time1 = System.currentTimeMillis();
        for(int i=0;i<10000000;i++){
            builder.delete(0, builder.length());
            builder.append("a");
            builder.append("b");
            builder.append("c");
            builder.append("d");
            builder.append("e");
        }
        System.out.println("delete 耗时：" + (System.currentTimeMillis() - time1));
        long time2 = System.currentTimeMillis();
        for(int i=0;i<10000000;i++){
            builder.setLength(0);
            builder.append("1a");
            builder.append("1b");
            builder.append("1c");
            builder.append("1d");
            builder.append("1e");
        }
>       System.out.println("setLenth=0 耗时：" + (System.currentTimeMillis() - time2));
