
##  [淘宝开放平台](http://open.taobao.com/index.htm "淘宝开放平台TOP")API调用Nodejs版本实现  ##

淘宝开放平台（Taobao Open Platform以下简称TOP）API调用方式可以通过[此处](http://open.taobao.com/doc/detail.htm?spm=0.0.0.0.XtOdmR&id=111)查看。

本代码为调用TOP API的Nodejs实现版本。可以在此基础上进行开发.

**你可以通过如下的步骤，轻松进入开发状态**


1，获得代码

> `git clone https://github.com/mz121star/taobao.git`

2，进入项目目录，执行如下命令安装项目的依赖包
> `npm install`

3，修改appconfig.js文件中的Appkey和Appsecret的值（该值从你的应用证书页面获得）

> `AppKey:"从淘宝获得的Appkey",`
>       
> `AppSecret:"从淘宝获得的AppSecret"`


**到此为止**，你已经完成了项目运行起来所需要的全部步骤，你可以通过如下命令启动web服务.

> `node app.js`

此时，你可以通过浏览器访问***http://localhost:3000***查看效果，本项目实现了一个简单的获取卖家当前出售商品的demo。至此，所有演示已经结束，你可以通过如下的介绍，开始进行项目的继续开发。

## **开始开发** ##

开发前首先用你喜欢的编辑器打开项目，此处我推荐使用[webstorm](http://www.jetbrains.com/webstorm/)。

项目结构

 |-public  `/*用来存放网站的静态资源，包括css js images等`

 |-routes  `/*存放controller文件`
 |-SDK
    
    |-index.js  /*提供了前端通过js调用API的功能 通过$.ajax("/rest")访问到的及时此文件    

 |-test        `/*存放测试文件，项目测试采用Mocha`

 |-util      调用淘宝API的核心功能

    |- sign.js 用于签名JSSDK
    |- TopAPI.js api调用的核心文件
    |- TopHelper.js一些工具类，如加密等
 |-views 存放前端HTML文件，项目使用**handlebars**模板引擎，所以文件后缀为**hbs**
 

**API签名流程**

  程序登陆页为index。

  登录成功后会回调到/success处理（此处为你在开发中心配置的回调地址）。在此处理中需要做如下判断。
  将成功验证的客户端session分配到cookie中，然后将页面跳转向**/main**(具体功能)页面.


>          if (TopHelper.VerifyTopResponse(qstring.top_parameters, qstring.top_session, qstring.top_sign, config.AppKey, config.AppSecret)){
>
>             var nick=TopHelper.GetParameters(qstring.top_parameters,"visitor_nick");
>             res.cookie(nick,qstring.top_session);
>             res.cookie("client_session",qstring.top_session);
>             res.send("验证成功");
>             res.redirect("/main");
> 
>         }




   
## 如何在nodejs中调用API？ ## 
 
  你只需通过如下一行代码即可调用（[具体API](http://open.taobao.com/doc/category_list.htm?spm=0.0.0.0.t3GHlD&id=102)）

>      TopAPI.Execute(method, options, function (data) {}）
>      /*method 需要调用API的名称，如：“taobao.item.get”
>      /*API需要传入的参数

## 如何利用jquery调用API？ ##

   和上面大体相同，你可以通过如下方式
   
>       $.post("/rest", {method:"taobao.items.onsale.get", options:options }, function (data) {}）
 
##API测试工具
   登录后访问/apitest，可以进入API测试工具
