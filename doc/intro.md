# 前言

如果你确认要接入BiliHP网络，那么你需要具备以下的能力

1.本地软件编写能力
2.网络编程能力

以上两个基础能力是你能完成接入的基础技能，如果你不了解TCP，那么请先查看相关的文档，学习如何使用你的语言打开并接入BiliHP


如果你是打算使用浏览器方案来连接的话，那么你将遇到很多的困难，因为bilihp主网是基于json-tcp协议的，一般的浏览器仅支持websocket，所以你只能使用外围基于http接口的功能


我们为接入者提供了两套技术方案，一套是http的，如果你是用http接入，那么你将可以完成的功能是"聊天、控制"这两大功能


如果你具备TCP/HTTP那么你将可以接入包括上面两个功能在内的"挂机、瓜子……"等等功能，具体功能列表，请参考BiliHP-APP项目


## 主网介绍

那么BiliHP的主网运行模式是基于模板解析+任务下发，所以你只需要在本地做好任务解析即可，数据分发已经在C2C和主网上完成了，客户端仅需解析即可

本文档将会在编写的时候放出"伪代码"写法，你可以根据自己的语言进行修改



## 数据类型

主网的数据类型为json，数据流顶针续麻，你只需要解析json即可


## 关于SuperCurl，CURL等概念说明

SuperCurl是一个底层连接发送器，这个发送器不带返回功能，不对程序内部的发起负责，执行后不管成功失败，这个SuperCurl的返回信息仅需要转发给TCP即可，SuperCurl依赖下面的Net模块完成

SuperCURL的执行方法看各位程序中的执行方式，每个语言都有类似但是不同的SuperCURL写法，所以这里不固定，请参考Go或者C#版本的Demo




CURL，CURL是一个带有返回功能的发送器，需要对程序负责，一般用于本地显示以及登录模块中，CURL模块本身不具有任何的Header/Cookie把控能力，是最初级的发送功能，返回的是最基础的JSON数据

CURL的执行方法是：

CURL(地址,参数)


Net，Net是一个高级的发送器，在C#中使用HttpWebRequest实现，在Go中使用TuuzGo的NetPost实现，在PHP中使用curl_opt实现，其他程序请自行找到对应的执行器

Net执行器中分为两种，一个是Net.Post一个是Net.Get，分别对应Post和Get方法，还有一个是Net.Combi，在Combi方法中，可以通过传入method方法，动态调用Post或者Get方法

Net的设定方法为：

    Net.SetHeader()//设定头部
    
    Net.SetCookie()//设定Cookie
    
    Net.Method()//使用netcombi方法的时候需要设定的参数
    
    Net.Post/Net.Get/Net.Combi(地址,参数)


Browser，Browser是一个浏览器方法，执行体为Browser(url)



## 关于ActionRoute

ActionRoute是程序中用来接收处理TCP消息的转发器，将对应的功能转发到对用的function模块中，用于识别tcp的路由是做转发处理还是做解析处理