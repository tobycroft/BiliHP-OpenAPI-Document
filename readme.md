# BiliHP-2020-2028闪电网络接入文档

## 闪电网络介绍

闪电网络是一个TCP-Multicast/UDP-Multicast网络，采用了TCP/UDP双架构，是一个去中心化网络

## C2C（星火节点）

+ 网络的接入点以C2CGo为单位，只要还有C2C服务器在，闪电网络就会一直在线，参考了区块链技术
+ C2CGo节点使用Go编写，性能极高，因为采用了新版算法，理论性能可以达到1200-1800个监听每秒
+ C2C星火节的任务由主网调配，全网数据广播共享，如未开启本地Storm节点，那么该星火节点将会脱离主网，自主执行任务
+ 星火节点承担HTTP任务，每个星火节点都会开放API端口和WS端口，对外提供数据，服务器主可以选择开启或关闭



依托闪电网络，

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