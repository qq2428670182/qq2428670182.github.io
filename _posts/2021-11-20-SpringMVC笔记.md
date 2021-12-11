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





请求的处理过程：调用Servlet的service方法
protected void service(....)
protected void doService(....)
protected void doDispatch(request,response){
  调用myController的.doSome()方法
}

框架的视图解析器可以帮助程序员设置视图文件的路径，在类上加注解来区分不同的模块，不同的模块下有不同的视图文件。在简化的路径下就不能写全路径访问了
因为要简写路径，所以有前缀和后缀（扩展名）设置，声明的是视图解析器对象，使用bean和property设置



不加模块，那么Controller的请求默认是/（根目录）开始



关于@RequesuMapping：
放在类的上面：是所有请求的公共部分，叫做模块名称，在方法上的RequestMapping控制的是属于该模块的具体的请求，RequestMapping的属性是value和method

> 属性：
>
> value：指定请求的名称，/开头
>
> method：指定请求的方式，它的值RequestMethod类的枚举值
>
> 例如表示get请求的RequestMethod.GET



控制器对象：把创建的对象作为控制器使用，并把该对象放入SpringMVC容器中，可以接受请求显示处理结果，当作Servlet来使用，但是使用@Controller注解创建的是一个普通类的对象，不是Servlet，但是SpringMVC赋予了其一些特殊功能
中央调度器：DispatcherServlet是SpringMvc中的一个Servlet对象，负责接收用户的所有请求之后，把请求转发给Contriller对象

###### web实现步骤:
jsp文件---->DispatcherServlet（即Servlet进行接受）----->转发，分配----->Controller对象

##### 最后总结创建SpringMVC项目的创建流程：
1）新建Maven工程
2）加入依赖（Spring-webmvc依赖、jsp、Servlet）

3）重点：在web.xml中注册核心对象DispatcherServlet（前端控制器）

4）创建一个发起请求的jsp文件
5）创建Controller类：在类上加入Controller注解，创建对象并放入SpringMVC容器中，同时在类的方法上加入@RequestMapping注解
6）创建一个显示最终结果的jsp文件
7）创建SpringMVC配置文件:
    1)因为使用了注解，所以一定使用组件扫描器
    2）声明视图解析器处理视图

###### 统一资源标识符（Uniform Resource Identifier，URI)是一个用于标识某一互联网资源名称的字符串。
###### URL是URI的子类，URLI的层级更高

直接接受用户参数：
逐个接受:处理器方法的形参名必须和请求的参数名name一致



``` java
/***
*当请求中参数名和Controller的形参不一致时，需要@RequestParam
*@RequestParam属性：1)value：请求中的参数名称
*2)required：boolean类型，默认是true，表示请求中必须包含此参数，不包含则会报错
*位置：位于方法内形参定义的前面
*例如public void doSome(@RequestParam("rname") String name)){}
*/
使用对象接请求参数，需要请求参数名和对象的属性名同名
```