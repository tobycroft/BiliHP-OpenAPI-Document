# BiliHP-2020-2028闪电网络接入文档

## 文档地址

### [文档地址](/doc/list.md)

## 闪电网络介绍

闪电网络是一个TCP-Multicast/UDP-Multicast网络，采用了TCP/UDP双架构，是一个去中心化网络

## C2C（星火节点）

+ 网络的接入点以C2CGo为单位，只要还有C2C服务器在，闪电网络就会一直在线，参考了区块链技术
+ C2CGo节点使用Go编写，性能极高，因为采用了新版算法，理论性能可以达到1200-1800个监听每秒
+ C2C星火节的任务由主网调配，全网数据广播共享，如未开启本地Storm节点，那么该星火节点将会脱离主网，自主执行任务
+ 星火节点承担HTTP任务，每个星火节点都会开放API端口和WS端口，对外提供数据，服务器主可以选择开启或关闭


## 目的

- 验证技术可行性
- 丰富软件语言，让所有语言开发者都可以参与开发（目前预计开发Go版本，PHP版本，E语言版本闪电网络）
- 为大家准备从HTML/JS级别难度的到PHP难度再到GO难度的项目练手
- 想用免费软件挑战付费软件（老版本哔哩哔哩助手从17年9月到2020年2月7日4点项目下线，2年多来没有任何广告）
- 想让普通用户在使用的同时能参与到助手网络开发中来
- 人人为我，我为人人就是C2CGo的思想（前年的项目是基于.Net的所以叫C2CMono，Mono是跨平台的一个技术方案）


## 关于闪电网络开发相关问题

#### 为什么要基于闪电网络开发而不是自己做一个？
- 要很闲，维护工作比较多
- 有一定技术难度，你的兴趣不是源源不断的
- 服务器要续费维护，自己不玩了，大家一起跪

#### 为什么不插件化，开发，让别人基于你的版本做插件？
- 首先，这样主程序难度很高（其他语言而言），如果这样有难度的作品不加密……不可能加密了就报毒，你想挑战谁的信任
- E语言做这类项目没有开源的，二开图什么……别人的用户数吗233333
- 基于E开发，势必增加插件开发难度，你至少要学E吧，如果底层用API对接，类似CoolQ那样的，人家疯了不自己做哦
- 收费问题，动脑子想，不说了……
- 最关键的，大家都是程序员，我为你的平台开发插件，然后呢，我是XX平台插件的开发者，做程序开发的

### 为什么使用闪电网络？
- 你可以基于闪电网络开发自己的应用（你可以完全决定你的项目怎么来，是否收费等
- 你可以用你熟悉的语言开发，PHP，Python，Node，Go，或者Ruby，甚至是猴油脚本都可以
- 你的软件可以通过BiliHP的闪电网络进行转发，你可以大大减少你架设服务器的成本，或者直接采用无服务器架构
- 全平台……不说了目前除了WinPhone没支持，连开发版都支持了……WinPhone请用GO模拟器吧，或者跑PHP也行
- 软件整合，你可以做自己的客户端，或者合并到BIHP（BiliHP的缩写）


## 主播网络
- 弹幕姬网络：

    - 如果你想开发一个弹幕姬
    - 假设，用户注册你的站点，绑定账号之后，他开播了，闪电网络就会给你推送这个直播间的弹幕，非常方便
    - 主播在BiliHP主播助手上就能看到他直播间的弹幕
    - 用户可以直接在逼理逼理助手中，与他的主播展开弹幕对话而不用连接到直播间，这对没条件看直播的用户来说是福利
  

- 录像网络：
    - 如果你想开发一个录播回放，或者全站自动录像系统
    - 同上，闪电网络会自动给你推RID的开播信息，你只要匹配这个用户是否在你系统中，然后执行FFMPEG即可
    - 你还可以反馈录像状态，录像当前大小的内容到主网，用户在手机上就能看到了
    
    
## BiliHP客户端介绍
手机设备
- BiliHP-Android
- BiliHP-IOS
 
PC设备
- Windows
- Linux
- Mac

智能硬件
- Openwrt系统：
    - 小米路由器
    - Newifi新路由
    - 各类OpenWRT路由器

专有硬件
- BiliHP星火硬件



# 开发文档

[TCP文档](/doc/list.md)