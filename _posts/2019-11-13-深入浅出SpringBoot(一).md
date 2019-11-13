---
layout:             post
atitle:               "深入浅出SpringBoot(一)"
title:                 "2019-11-13-深入浅出SpringBoot(一)"
date:                2019-11-13
author:            "millzhang"
catalog:           true
header-style:   text
tags:
    - java
---

## SpingBoot简介

#### 优点

- 创建独立的Spring应用程序
- 嵌入`Tomcat`无需部署`war`文件
- 允许通过`Maven`来获取`starter`
- 自动装配的`spring`
- 提供生产就绪型功能，如指标、健康检查和外部配置
- 绝对没有代码生成，对XML没有配置要求

#### 加载顺序

- 命令行参数
- 来自`java:comp/env`的`JNDI`属性
- 