---
layout: post
title: 'SpringMVC笔记'
date: 2021-11-08
author: Futari
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: Java SpringMVC
---



Controller：控制器对象，把创建的对象作为控制器使用，并把该对象放入SpringMVC容器中，可以接受请求显示处理结果，当作Servlet来使用，但是使用@Controller注解创建的是一个普通类的对象，不是Servlet，但是SpringMVC赋予了其一些特殊功能
中央调度器：DispatcherServlet是SpringMvc中的一个Servlet对象，负责接收用户的所有请求之后，把请求转发给Contriller对象

web实现步骤:
jsp文件---->DispatcherServlet（即Servlet进行接受）----->转发，分配----->Controller对象



最后总结创建SpringMVC项目的创建流程：
1）新建Maven工程
2）加入依赖（Spring-webmvc依赖、jsp、Servlet）

3）重点：在web.xml中注册核心对象DispatcherServlet（前端控制器）

4）创建一个发起请求的jsp文件
5）创建Controller类：在类上加入Controller注解，创建对象并放入SpringMVC容器中，同时在类的方法上加入@RequestMapping注解
6）创建一个显示最终结果的jsp文件
7）创建SpringMVC配置文件:
    1)因为使用了注解，所以一定使用组件扫描器
    2）声明视图解析器处理视图

PS：统一资源标识符（Uniform Resource Identifier，URI)是一个用于标识某一互联网资源名称的字符串。
URL是URI的子类，URLI的层级更高

``` java
/***    使用方法来处理用户的请求，方法自定义
     * @RequestMapping ：请求映射，作用是把一个请求地址和一个方法绑定在一起，一个请求指定一个方法
     * 下面的some.do是一个请求地址，所以前面要加上/，对应下面的方法
     * 属性：String value 必须是唯一的
     *
     * 返回值类型MedolAndView代表数据和视图
     */
/***
 * 能处理请求的都是控制器，Controller处理请求，他是后端控制器
 * DispatcherController是前端控制器
 */

```



