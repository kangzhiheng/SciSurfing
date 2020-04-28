##  “免费”科学上网新姿势（一）基本动作（谷歌云+BBR加速+SSR）

这篇文章分为两大部分，[简介](#简介)和[方法](#方法)，即我们在国内“科学上网”的背景，以及“科学上网”的基本方法。想要直接获取方法的朋友请进[传送门](#方法)。

### 目录

- [简介](#简介)
  - [背景](#背景)
  - [健康上网行为准则](#健康上网行为准则)
  - [客观看待“墙”](#客观看待"墙")
  - [互联网是怎么工作的？](#互联网是怎么工作的？)
  - [入侵检测手段](#入侵检测手段)
    - [域名解析缓存服务污染（DNS劫持）](#**域名解析缓存服务污染**（DNS劫持）)
    - [IP地址或传输层端口封锁（IP封锁）](#IP地址或传输层端口封锁（IP封锁）)
  - [常用的“名词”](#常用的"名词")
    - [VPN](#VPN)
    - [VPS](#VPS)
    - [SS与SSR](#SS与SSR)
    - [V2ray](#V2Ray)
- [方法](#方法)
  - [基本原理](#基本原理)
  - [配置环境](#配置环境)
    - [硬件环境](#硬件环境)
    - [软件环境](#软件环境)
    - [其它环境](#其它环境)
  - [搭配方案](#搭配方案)
  - [win10配置步骤](#win10配置步骤)
    - [Step1 - 翻个小墙](#Step1 - 翻个小墙)
    - [Step2 - 注册谷歌账号](#Step2 - 注册谷歌账号)
    - [Step3 - 注册谷歌云服务](#Step3 - 注册谷歌云服务)
    - [Step4 - 创建虚拟机实例](#Step4 - 创建VPS服务器)
    - [Step5 - 修改谷歌云控制权限](#Step5 - 修改谷歌云控制权限)
    - [step6 - 登录VPS服务器](#step6 - 登录VPS服务器)
    - [step7 - 安装BBR加速内核](#step7 - 安装BBR加速内核)
    - [Step8 - 在VPS上安装SSR服务](#Step8 - 在VPS上安装SSR服务)
    - [Step9 - 关闭谷歌云防火墙](#Step9 - 关闭谷歌云防火墙)
    - [Step10 - 连接SSR](#Step10 - 连接SSR)
- [结语](#结语)
- [参考](#参考)

### 简介

#### 背景

国内广大研究群体，使用[Google](https://www.google.com)搜索和[百度](https://www.baidu.com)搜索，都有不同的体验，二者各有利弊，但是因为工作和学习的需要，需要使用境外的一些网站，比如[Google](https://www.google.com)。今天不谈政治，只谈技术。在正常情况下，国内是无法直接访问外网的，包括**Google全家桶**（除了谷歌翻译）、**Facebook**、**Twitter**、**Reddit**、**Youtube**（Google旗下）、**Netflix**、**Ins**等。为了方便研究和学习，在这篇文章，介绍一种健康良好的“科学上网方法”，其实不止在国内需要“科学上网”，世界上需要国家亦是如此。如果您利用下述方法可以“科学上网”，还请遵守以下**行为准则**。

#### 健康上网行为准则

“无规矩不成方圆”，工具可以方便人们生活，也可以破坏人们的生活，所以，如何正确使用我们手中的工具，需要我们自己把握。特此呼吁“科学上网的人们”，遵守以下约定，成就健康良好的上网环境！

- 拥护党的领导，不质疑、不攻击、不诋毁国家的做法；
- 维护国家利益，不信谣、不传谣；
- 明辨是非，相信祖国；
- 科学上网，禁止非法交易；

在阅读以下内容前，您需要首先阅读并认可上述**行为准则**；如果您不认同，还请Ctrl+F4。

#### 客观看待“墙”

为什么有些网站打不开呢？是因为国家建了一所防火墙——**防火长城**。

> **GFW**，Great Firewall，**防火长城**，也称**中国国家防火墙**。 是对我国在其互联网边界审查系统（包括相关行政审查系统）的统称。此系统起步于1998年。国内平时所说的“被墙”即指网站内容被**防火长城**所屏蔽或者指服务器的通讯被封阻，“翻墙”也被引申为[突破网络审查](https://zh.wikipedia.org/wiki/突破网络审查)浏览中国大陆境外被屏蔽的网站或使用服务的行为。 其实在国外，要查看国内的某些内容，比如腾讯、爱奇艺等国内版本，正常情况下在国外是使用不了的，如果要查看也是要从国外翻回来的。

虽然防火长城让我们无法使用Google、Facebook等流行网站，有时候确实显得不太方便。我刚开始对这所墙也是略有不满，但是随着近几年发生的事情，以及自己在国外的一些经历，慢慢发现GFW也不是对自己完全不利。我国正处于高速发展期间，互联网上存在着大量的内容，但很多人很难分辨这些内容的真假，在国内尚且如此，国外这些内容就更多了，西方国家动不动就批判我们的国家，打着民主自由的口号，干一些见不得人的勾当，想想就觉得可笑。我们国家有些人对这些谣言信以为真，一些谣言一旦传播开来，说严重点，就会影响国家的根基，带来社会的不稳定。这才是真正的罪过。**如果你没有一个真正平稳的心态，请不要涉足国外和政治有关的网站**。

![GFW](https://pic.downk.cc/item/5e7c9d84504f4bcb04b956b7.jpg)

技术无罪，人心叵测，在珍惜科技发展带来方便的同时，我们也需要一双明亮的大眼睛。

#### 互联网是怎么工作的？

知其然，知其所以然。我们要知道，在浏览器里，输入`https://www.google.com`，后台到底发生了什么？正常情况下，为什么输入`https://www.google.com`，会返回错误信息，而输入`https://www.baidu.com`，会出现渲染界面。有兴趣的读者可以观察这个[视频]( https://www.kenzhishi.com/1933.html )进行简单的了解。

在正常情况下，浏览器从输入一个域名地址，例如[https://www.baidu.com](https://ww.baidu.com)到页面呈现，[**大概分8步**]( https://segmentfault.com/a/1190000012092552 )：

- 第一步：把冰箱门打开，不是，第一步先进行DNS域名解析：把域名地址解析到对应的IP地址（**非常重要**）；
- 第二步： 建立TCP连接； 
- 第三步： 发送HTTP请求； 
- 第四步： 服务器处理请求； 
- 第五步： 返回响应结果； 
- 第六步： 关闭TCP连接； 
- 第七步： 浏览器解析HTML； 
- 第八步： 浏览器布局渲染； 

那么为什么有些网站打不开呢？请再看一个[视频]( https://www.kenzhishi.com/587.html )。

![GFW](https://source.thankjava.com/view/XDyk9qy)

**GFW**的存在就像疫情期间路口的卡子，夹在用户和服务之间，每当用户获取服务时，不得不经过GFW，它会查验你的“通行证”是否满足要求，不满足就会被阻拦，多次持有不合法的通行证，就会被列入黑名单。

通过查阅资料，猜想中的防火长城GFW至少有两种入侵检测手段是：

- **域名解析缓存服务污染**（DNS污染）
- **IP地址或传输层端口封锁**（IP封锁）

#### 入侵检测手段

##### **域名解析缓存服务污染**（DNS劫持）

IP地址是互联网世界中的主机设备的独一无二的由数字组成的标记，是用来唯一标识互联网上计算机的逻辑地址。 要将信息发到对应的服务器上时，我们需要知道对方的“地址”，即IP地址，形如aaa.bbb.ccc.ddd。但是IP地址不容易记忆，所以使用一个可读性更强的“域名”来方便记忆，域名与IP地址是一对一或者多对一。我们向服务器发送信息时，解析域名到指定的IP地址是通过**DNS域名服务器**来进行的。当发现一个不和谐网站，然后就把这个网站的域名给加到GFW劫持列表中。DNS客户端会盲目地相信第一个收到的答案，所以你去查询facebook.com的话，GFW只要在正确的答案被返回之前抢答了，然后伪装成你查询的DNS服务器向你发错误的答案就可以了。  

##### IP地址或传输层端口封锁（IP封锁）

通过将需要拦截的IP地址配置为空路由或者黑洞设备上，然后通过[动态路由协议](https://zh.wikipedia.org/wiki/动态路由协议)将相应配置路由扩散到公众互联网网络中，从将条件匹配拦截行为转为路由器的常规转发行为，从而提高拦截效率。多见于自主拥有大量IP地址段的需要审查的企业中。

#### 常用的“名词”

##### VPN

**虚拟专用网络**，Virtual Private Network，简称**VPN**。VPN 在很多人心目中就是用来翻墙的工具，其实不然，VPN 最主要的功能，并不是用来翻墙，只是它可以达到翻墙的目的。VPN的**功能**是：在公用网络上建立专用网络，进行加密通讯，在企业网络和高校的网络中应用很广泛。接入 VPN其实就是接入了一个专有网络，你的网络访问都从这个出口出去，你和 VPN 之间的通信是否加密，取决于你连接 VPN 的方式或者协议。

**VPN是为了保证通信的安全性、私密性，不是专门为“科学上网制定的技术。**

##### VPS

**虚拟专用服务器**，Virtual private server，简称**VPS**。不妨想一个问题：我们平时上网浏览网页，我们访问的那些网页都是哪来的？答案很简单，从另一台电脑上下载下来的。无论是用户平时所使用的个人PC电脑，还是用于搭载网站的服务器，本质上都是电脑。但与个人PC不同，被用作服务器的电脑必须做到24小时开机在线，以确保能在任何时候回应用户的请求。而VPS，就是不会关机的电脑。

VPS虚拟专用服务器是由VPS提供商维护，租用给站长使用的“不会关机的电脑”，也是服务器的一种。VPS不是一台台独立的电脑，而是将一台巨型服务器通过虚拟化技术分割成若干台看似独立的服务器，这台巨型服务器不间断运行，被分割出来的小服务器也跟着不停的运作，站长租用其中一台小服务器，搭载上自己的站点，就可以等着用户访问了。

VPS提供商有：**[Vultr](http://www.vultr.com/?ref=7038906)、[Digital Ocean](https://m.do.co/c/ad240a3367a0)、[Linode](https://www.linode.com/)、[搬瓦工（bandwagonhost）](https://bwh1.net/)**

**实验就用的是[Vultr](http://www.vultr.com/?ref=7038906)，用得人多，特征会明显，容易被GFW识别并拦截，因此故障多一点，需要经常换IP。**Vultr性价比比较高，适合个人使用。

##### SS与SSR

![SS与SSR](https://pic.downk.cc/item/5e7cccbe504f4bcb04d92eac.png)

SS，全称ShadowSocks，SSR，全称ShadowSocks-R，如上图，SS/SSR的图标酷似飞机，又俗称机场，SS/SSR的账号俗称机票。SS/SSR是一种跨域网络加速的开源方案，说白了就是一种穿透GFW的的**代理工具**。

对SS/SSR来说穿透防火墙是第一位，抗干扰性强，而且对流量做了混淆，所有流量在通过防火墙的时候，基本上都被识别为普通流量，也就是说你翻墙了，但是GFW是检测不到你在翻墙的，SS/SSR更注重流量的混淆加密。

SSR作者声称SS不够隐匿，容易被防火墙检测到，SSR在改进了混淆和协议，更难被防火墙检测到。简单地说，SSR是SS的改进版。SS和SSR两者原理相同，都是基于socks5代理。客户端与服务端没有建立专有通道，客户端和实际要访问的服务端之间通过代理服务器进行通信，客户端发送请求和接受服务端返回的数据都要通过[代理服务器](https://www.idcbest.com/servernews/11000420.html)。SSR目的是为了能让流量通过防火墙。

SSR和SS有一些区别，如下：

1. SSR可以伪装成自定义的http开头，让监控着认为你是在访问百度，将实际的内容进行加密。SS是纯加密流量；
2. 如果SS速度用着很快，没什么问题，带宽基本都能跑满，那么就可以继续使用SS。如果SS总是跑着跑着速度就渐渐上不去，或者没有速度的时候，就可能是遇到了[QOS](https://zh.wikipedia.org/zh-hans/%E6%9C%8D%E5%8A%A1%E8%B4%A8%E9%87%8F)，需要使用SSR。当然，两者直接使用SSR也是没什么问题的；

客户端请求服务端数据流程（**SSR**)：

1、浏览器发送请求（基于socks5协议），通过SSR客户端将sock5协议通过协议插件和混淆插件进行转换加密，使得来自客户端的流量和基于HTTP协议的流量无差别；

2、SSR服务端（代理服务器）收到请求后，通过混淆插件、协议插件将数据解密并还原协议，最后转发到目标服务器。

顺便提一嘴SS和SSR代理工具的故事，大概是酱婶儿的。

SS作者是clowwindy，他自己为了翻墙写了shadowsocks，简称 ss 或者叫影梭，后来他觉得这个东西非常好用，速度快，而且不会被封锁，他就把源码共享在了 GitHub 上，然后就火了，但是后来作者被请去喝茶，删了代码，并且保证不再参与维护更新。

这个悲伤的故事还没有结束，在SS作者被喝茶之后，GitHub 上出现了一个叫 breakwa11(破娃)的帐号，声称SS容易被防火墙检测到，所以在混淆和协议方面做了改进，更加不容易被检测到，而且兼容SS，改进后的项目叫ShadowSocks-R，简称SSR，然后SS用户和SSR用户自然分成了两个派别，互撕，后来，破娃被人肉出来，无奈之下删除了SSR的代码，并且解散了所有相关群组……

朋友们，好东西就自己好好用吧……愿天堂没有争吵。

##### V2Ray

V2Ray 是 Project V 下的一个工具。Project V 是一个包含一系列构建特定网络环境工具的项目，而 V2Ray 属于最核心的一个。简单地说，V2Ray 是一个与 ShadowSocks 类似的代理软件，可以用来科学上网（翻墙）学习国外先进科学技术。

**那么V2Ray和ShadowSocks有什么区别呢？**

ShadowSocks 只是一个简单的代理工具，而 V2Ray 定位为一个平台，任何开发者都可以利用 V2Ray 提供的模块开发出新的代理软件。

ShadowSocks 是 clowwindy 开发的自用的软件，开发的初衷只是为了让自己能够简单高效地科学上网，自己使用了很长一段时间后觉得不错才共享出来的。V2Ray 是 clowwindy 被喝茶之后 V2Ray 项目组为表示抗议开发的，一开始就致力于让大家更好更快的科学上网。

由于出生时的历史背景不同，导致了它们性格特点的差异。

简单来说，ShadowSocks 功能单一，V2Ray 功能强大，但Shadowsocks 简单好上手，V2Ray 复杂配置多。

事物的优点和缺点总是相生相随的。相对来说，V2Ray 有以下优势：

- **更完善的协议**： V2Ray 使用了新的自行研发的 VMess 协议，改正了 ShadowSocks 一些已有的缺点，更难被GFW检测到；

- **更强大的性能**：网络性能更好，具体数据可以看 [V2Ray 官方博客](https://steemit.com/cn/@v2ray/3cjiux)

- 更丰富的功能：以下是部分 V2Ray 的功能

  - **mKCP**: KCP 协议在 V2Ray 上的实现，不必另行安装 kcptun
  - **动态端口**：动态改变通信的端口，对抗对长时间大流量端口的限速封锁
  - **路由功能**：可以随意设定指定数据包的流向，去广告、反跟踪都可以
  - 传出代理：看名字可能不太好理解，其实差不多可以称之为多重代理。类似于 Tor 的代理
  - **数据包伪装**：类似于 Shadowsocks-rss 的混淆，另外对于 mKCP 的数据包也可伪装，伪装常见流量，令识别更困难
  - **WebSocket 协议**：可以 PaaS 平台搭建V2Ray，通过 WebSocket 代理。也可以通过它使用 CDN 中转，抗封锁效果更好
  - **Mux**：多路复用，进一步提高科学上网的并发性能

  V2Ray虽然优势明显，但配置复杂，产业链不成熟，不适合新手入门。

### 方法

![翻墙原理](https://pic.downk.cc/item/5e7c9dc9504f4bcb04b98b01.png)

#### 基本原理

在上一节[简介](#简介)里，给大家介绍了科学上网的相关知识，这一节，介绍科学上网的一般方法。

在前面提过，SS和SSR的原理都是一样的，就是 socks5 代理。socks 代理只是简单的传递数据包，而不必关心是何种协议，所以 socks 代理比其他应用层代理要快的多。socks5 代理是把你的网络数据请求通过一条连接你和代理服务器之间的通道，由服务器转发到目的地，这个过程中你是没有通过一条专用通道的，只是数据包的发出，然后被代理服务器收到，整个过程并没有额外的处理。

**通俗的说，现在你有一个代理服务器（VPS）在香港，比如你现在想要访问google，你的电脑发出请求，流量通过 socks5 连接发到你在香港的服务器上，然后再由你在香港的服务器去访问 google，再把访问结果传回你的电脑，这样就实现了翻墙。**

#### 配置环境

##### 硬件环境

1. **VPS服务器**
- 免费VPS：不好意思，没有一直免费的，目前网上搜到的免费的VPS服务器，极不稳定，影响体验，适合偶尔使用，不适合长期使用；
   - 国外的云服务：[谷歌云](https://cloud.google.com/)、[亚马逊云](https://aws.amazon.com/cn/)、[微软云服务Azure](https://azure.microsoft.com/zh-cn/)等，这些云服务，基本上都会提供一年的免费体验期，可以挨个换着用，通过特殊的方法，有些云服务可以使用不止一次……其实国内的阿里云有[国际版](https://link.zhihu.com/?target=https%3A//www.alibabacloud.com/%3Fspm%3D5176.2020520130.1001.d1.1dd16bc4h2cx2M)，但是禁止大陆用户注册……
   - 学生优惠：包括上述的国外云服务厂商，还有[Digital Ocean](https://m.do.co/c/ad240a3367a0)，一定要记得自己的学校邮箱，可以申请到不少好东西，还可以申请学生优惠；
   
- VPS提供商有：**[Vultr](http://www.vultr.com/?ref=7038906)、[Linode](https://www.linode.com/)、[搬瓦工（bandwagonhost）](https://bwh1.net/)**，这些都是消费服务，但是有时候优惠很大，Vultr应该是国内用户用得最多的VPS，价格厚道，支持xx宝付款，但是无奈用户太多，且大部分用户都选择在日本的机房，特征明显压力大，最近频频被封号……[Linode](https://link.zhihu.com/?target=https%3A//www.linode.com/%3Fr%3D34ae657f093b6c1f90eb6cba7015e9bd852db128)强推；
  
2. 一台可以上网的电脑（个人PC）Windows、Mac系统都可以。

##### 软件环境

1. SSH登录终端：[**Xshell**](https://www.netsarang.com/zh/xshell-download/)、[**FinalShell**](http://www.hostbuf.com/t/988.html)等；
2. Chrome浏览器
3. 谷歌账号
4. SSR客户端

##### 其它环境

1. 实体信用卡或者[虚拟信用卡](https://www.payoneer.com/raf/zh/?rid=C9D47D6B-361F-47BA-B49B-0EE3C054786B)

#### 搭配方案

经过我的一番调研，在这个特殊时期，在**Windows10**系统下，使用**谷歌云 + BBRPlus加速 + SSR**的方案，可完美科学上网，谷歌搜索WiKi百科无压力，Youtube略有压力。

**完美支持Windows、Android、Mac。**

下表是我的配置环境

|   环境    |     版本号     |
| :-------: | :------------: |
|   系统    | Windows10 1909 |
| VPS服务商 |     谷歌云     |
|  浏览器   | Chrome/Edge等  |
| 终端登录  |     Xshell     |
| 代理工具  |      SSR       |

**VPS服务器**：**谷歌云**，新用户注册谷歌云，可获得300美刀的赠金。

#### Win10配置步骤

实验环境是Windows10，**谷歌云 + BBRPlus加速 + SSR**配置步骤大概分10步。

##### Step1 - 翻个小墙

没错，第一步就是翻墙，翻个小墙，偷摸摸的出去干点合理合法的小事情，一切为了学习！因为需要一个谷歌账号，如果已经有了谷歌账号，则需要在谷歌云网站注册为新用户，所以我们需要一些方法先翻出去，搞定这些，做好铺垫，再回来利用终端进行配置。

搜集了一些不太稳定，但是可以暂时翻出去的方法。

1. **网站类**：[https://ac.scmor.com/](https://ac.scmor.com/)，这是一个镜像网站，不稳定，但有时可以用；

![Google镜像网站](https://source.thankjava.com/view/XEX0gCk)

2. **插件类**：请移步[链接](https://pan.baidu.com/s/1Dz47RxD2im1QS_aOLod6uQ)进行下载，提取码是hd2q。里面有三款插件，推荐使用**VeePN**。插件的安装方法：

   - 打开Chrome浏览器：输入[chrome://extensions/](chrome://extensions/)，打开后，点击右上角的按钮，进入**开发者模式**；

   ![Chrome开发者模式](https://pic.downk.cc/item/5e7cdf1a504f4bcb04e33b74.png)

   插件的格式为crx，将其拖入上述界面，若出现错误，则将插件后缀名.crx更改为.zip后，重新拖入；

   - 若继续出现错误，将.zip文件进行解压，点击`加载已解压的扩展程序`，选中刚刚解压好的文件夹，点击确定；
   - **Edge浏览器用户**，进入[edge://extensions/](edge://extensions/)，点击`加载解压缩的扩展`，选择上一步解压好的文件夹，确定后进行尝试；

3. **免费SSR账号**：链接为https://free-ss.site/，使用方法参考[Step 10](#Step10：连接SSR)（截止2020.04.02，该网站还没有恢复运营）

如果上述方法都不可用，请联系管理员。

后续步骤都建立在**Step1**之上，即先翻个小墙，所以想成功完成配置的同学，还是想方设法翻一下墙。

#####  Step2 - 注册谷歌账号

这一步无需多言，在翻墙进来之后，打开[Google账户注册](https://support.google.com/accounts/answer/27441?hl=zh-Hans)页面，一步一步进行操作，手机号貌似是可选项，可以不填；整个过程，这一步应该是最好过的一步了。

#####  Step3 - 注册谷歌云服务

**新用户注册谷歌云，可获得300美刀的赠金。**这些小钱钱是打到你的谷歌云账户里的，不能提现，只能在使用谷歌云服务时，进行抵押。

在注册谷歌云，非常非常关键的一步是，需要一张**信用卡**。如果你的信用卡是实体卡，也就是在国内申请的，并没有问题，可以不看这段；但是大部分学生是没有信用卡的，如何薅羊毛呢？利用[Payoneer](https://www.payoneer.com/raf/zh/?rid=C9D47D6B-361F-47BA-B49B-0EE3C054786B)，全中文界面，如果你相信这家公司，就请注册，审核比较快，审核通过后，可以进行谷歌云账号的注册，Payoneer的注册教程请[移步](https://www.jianshu.com/p/5d918a6aec1c)。

拿到信用卡账号后，打开[谷歌云官网](https://cloud.google.com/)进行注册，整个过程不难，需要注意的是：

- 在**国家/地区**的选择列表里，并没有中国，这并不奇怪……隨便选择一个国家即可；
- 还有在输入你的**地址**时，有一个简单的办法，大家可以在[谷歌地图](https://www.google.com/maps/?hl=zh-CN)搜索你所选择国家的大学地址就好，比如搜索斯坦福大学，地址：450 Serra Mall, Stanford, CA 94305。另外，不需要1美元进行验证。

如果看到可选的项目，可以不填。

至此，你已经注册好了谷歌云，并进入了控制台界面。

![Google结算界面](https://pic.downk.cc/item/5e7ce99a504f4bcb04e9946d.png)

![Google结算界面](https://pic.downk.cc/item/5e7ceb8f504f4bcb04eac66d.png)

如上图，进入到**结算界面**后，我们在右侧可以看到赠送到你账户的300刀以及免费使用的天数365天。因为我之前注册过并使用了一些服务，所以服务的费用从300刀里面扣了一部分，需要注意的是，这部分赠金使用完后，**不自动**从绑定的信用卡里进行扣费。

#####  Step4 - 创建VPS服务器

在这一步，创建VPS服务器，在谷歌云里，叫**虚拟机实例**（VM实例）。

![虚拟机实例创建](https://pic.downk.cc/item/5e7cecdf504f4bcb04eb79c5.png)

进入**虚拟机实例**创建界面，点击**新建**或**创建实例**，进行VPS配置。

- **名称**：随便取，比如 scisurfing，只能是小写英文字母；
- **标签**：省略不填；
- **区域**：如果在国内，尽可能选择离我们近一点的城市，比如中国台湾、中国香港、东京、大坂、首尔、孟买、新加坡等，要根据自己的为位置和网速进行选择，配置完成后可以试一下哪个速度最快；

![地区选择](https://pic.downk.cc/item/5e7cee56504f4bcb04ec6109.png)

- **地区**：同上，随便选一个即可，到时候可以挨个尝试；

- **机器配置**：
  - **系列**：N1即可；
  - **机器类型**：列表里有各种配置，配置越高，收费越贵，根据实际情况来选择，这里我选择`n1-standard-1（1个vCPU，3.75GB内存）`；
- **启动磁盘**：根据自己的习惯来选择，这里我选择**CentOS系统**，版本号选择**7**。

![启动磁盘](https://pic.downk.cc/item/5e7cf1d5504f4bcb04ee2435.png)

- **防火墙**：将`允许HTTP流量`和`允许HTTPS流量`全部勾选上。

我的整体配置如下：

![](https://pic.downk.cc/item/5e7cf3c5504f4bcb04ef0e30.png)

配置完成后，右侧会显示每个月的具体花费，根据消费金额，可适当调整。紧接着点击`创建`按钮，稍等片刻，控制台再完成配置之后，会生成一个**外部IP地址**，如下图所示：

![](https://pic.downk.cc/item/5e7cf4d5504f4bcb04ef8281.png)

为了测试这个IP可以在国内使用，需要在[站长之家](http://ping.chinaz.com/)对这个IP（aaa.bbb.ccc.ddd）进行检测，ping得通，则认为这个IP还没有被墙，可以正常使用，进行下一步，如下图，一片“绿”就对了。

![Ping检测](https://pic.downk.cc/item/5e7cf5cd504f4bcb04eff81a.png)

如果这个IP在国内ping不通，则显示一片红，需要在控制台删除这个实例，再重新创建，直到一片绿油油的景象……

#####  Step5 - 修改谷歌云控制权限

这一步的目的是为了我们可以在本地计算机利用ssh管理这个VPS服务器，而不是每次都要登录到谷歌云控制台里面。

![VM-SSH登录](https://pic.downk.cc/item/5e7cf703504f4bcb04f07c4d.png)

点击上图里的`SSH`，加载一段时间会弹出一个终端界面，如下图所示，其服务器系统就是在创建实例的时候选择的系统，比如我选择是CentOS 7版本

![谷歌云终端界面](https://pic.downk.cc/item/5e7cf7f4504f4bcb04f0e614.png)

首先要获取服务器root权限，输入

```bas
sudo -i
```

获取root权限后，继续输入

```bash
# 允许 ssh 密码登录
sed -i 's/PermitRootLogin no/PermitRootLogin yes/g' /etc/ssh/sshd_config

sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
```

然后再终端重启sshd服务。

```bash
systemctl restart sshd.service
```

修改密码，在终端继续输入

```bash
passwd
```

按照提示更改登录密码，温馨提示，尽可能不要用小键盘输入数字，密码请使用数组加字母的组合形式。

执行完以上命令，接着就可以在 xShell 中使用账号密码来连接主机啦，**后续步骤都可以在xShell中进行**，也可以在网页端进行，网页里的ssh连接有点卡。

##### Step6 - 登录VPS服务器

在**个人电脑**上，打开xshell或者其它终端，具体使用方法请看我的[另一篇文章（Lab532服务器环境(CentOS 7.6)须知）](https://github.com/kangzhiheng/GitLocalDoc#ssh%E7%99%BB%E5%BD%95)，输入

```bash
ssh root@IPAddr -p 22
```

读者们仅仅需要将`IPAddr`替换成自己的VPS IP地址就好。第一次登录可能会出现以下界面，输入`yes`即可。

![SSH登录](https://pic.downk.cc/item/5e7cfd04504f4bcb04f2d5ae.png)

#####  Step7 - 安装BBR加速内核

[BBR](https://github.com/google/bbr) 是 Google 提出的一种新型拥塞控制算法，可以使 Linux 服务器显著地提高吞吐量和减少 TCP 连接的延迟。从 4.9 开始，Linux 内核已经用上了该算法。部署最新版内核，开启TCP BBR 加速的 VPS，**网速可以提升几个数量级**。

CentOS默认没有安装`wget`，需手动安装：

```bash
yum -y install wget
```

安装BBR加速，允许下列代码

```bash
wget --no-check-certificate https://raw.githubusercontent.com/cx9208/Linux-NetSpeed/master/tcp.sh && chmod +x tcp.sh && ./tcp.sh
```

出现的界面如下：

![BBR加速](https://pic.downk.cc/item/5e7d03ad504f4bcb04f4ec70.png)

输入数字`2`，等待安装`BBRplus版内核`

安装完成后，可能出现以下界面

![安装BBR](https://pic.downk.cc/item/5e7d0575504f4bcb04f570f9.png)

输入`Y`，如果出现了这样的界面

![BBR](https://pic.downk.cc/item/5e7d05b9504f4bcb04f5843b.png)

选择`No`即可。

此时本地客户端连接断开，谷歌云正在重启VPS服务器，回到**谷歌云控制台**里的虚拟机实例页面，点击`SSH`

![VM-SSH登录](https://pic.downk.cc/item/5e7cf703504f4bcb04f07c4d.png)

加载网页上的终端后，输入

```bas
sudo -i
```

获取root权限，运行

```bas
./tcp.sh
```

出现以下界面

![安装BBR加速](https://pic.downk.cc/item/5e7d0705504f4bcb04f5ef36.png)

现在安装BBR加速，选择`7`。

很快，出现界面

![BBR安装完成](https://pic.downk.cc/item/5e7d075c504f4bcb04f60b46.png)

出现了这个界面，表示BBR加速已经安装完成并启动成功了。

##### Step8 - 在VPS上安装SSR服务

继续在上一步的网页版的终端里运行代码

```bas
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

![服务器端SSR](https://pic.downk.cc/item/5e7d080b504f4bcb04f645fa.png)

输入`1`，进行SSR安装

安装过程中需要对**端口**、**密码**、**协议插件**、混淆插件等进行配置，全部默认即可（一直按Enter键），直至出现

![SSR安装](https://pic.downk.cc/item/5e7d0973504f4bcb04f6b0fb.png)

输入`y`，安装服务端SSR所需要的依赖服务，这可能需要个几分钟时间，**然后可能会出现一个啥都不显示的界面**，恭喜你，安装完成了，这时候需要用鼠标进行页面向上的滚动，就会看到

![BBR安装完成](https://pic.downk.cc/item/5e7d0a80504f4bcb04f81ead.png)

离成功越来越近了，继续操作。**可以将上述SSR配置信息保存到本地。**

#####  Step9 - 关闭谷歌云防火墙

![VPS防火墙](https://pic.downk.cc/item/5e7d0b3d504f4bcb04f86edf.png)

如上图，打开`VPC网络`里的`防火墙规则`

![VPS防火墙修改](https://pic.downk.cc/item/5e7d0c11504f4bcb04f8beff.png)

因为在之前的设置中，使用了不同的端口和协议，所以需要修改防火墙规则，上图里用红色框框圈出来的两个过滤表，**都需要进行相同的操作**，选择第一个为例进行说明。

点击第一个，进入下一个界面，如下图所示，点击修改，将页面滑到最下面。

![](https://pic.downk.cc/item/5e7d0cb4504f4bcb04f8f64b.png)

![](https://pic.downk.cc/item/5e7d0d23504f4bcb04f91c4b.png)

将`协议和端口`改为**全部允许**，`default-allow-https`也是一样的操作。

##### Step10 - 连接SSR

下载SSR客户端，链接为：[Windows](https://github.com/shadowsocksrr/shadowsocksr-csharp/releases)、[Android](https://github.com/shadowsocksrr/shadowsocksr-android/releases)、[Mac](https://github.com/qinyuhang/ShadowsocksX-NG-R/releases/)

以Win10系统为例，解压文件夹后，运行`ShadowSOcksR-dotnet4.0.exe`。

![SSR客户端](https://pic.downk.cc/item/5e7d0ebe504f4bcb04f9a5c9.png)

SSR正常运行时，桌面右下角出现一个粉色的飞机小图标。

返回到网页终端，或者打开保存好的SSR配置信息。

![BBR安装完成](https://pic.downk.cc/item/5e7d0a80504f4bcb04f81ead.png)

复制上图中的`SSR链接`。紧接着，右键SSR客户端

![](https://pic.downk.cc/item/5e7d1033504f4bcb04fa30ed.png)

选择`剪切板批量导入ssr://链接`，出现界面

![](https://pic.downk.cc/item/5e7d10a5504f4bcb04fa56dd.png)

可以进行备注和群组名填写，然后选择确定，配置完成。

右键SSR客户端，在

- `系统代理模式`里，选择`PAC模式`；
- `服务器`里，选择刚刚配置好的服务器IP；

一切准备就绪，测试能否[翻墙](https://google.com.hk)，若不能，删除该VPS服务器，重新配置即可。

### 结语

一般来说，刚刚配置的VPS，速度都不错，在[油管](https://www.youtube.com/)看4K基本无压力，速度可达24000Kbps。但是随着时间的推移，就会发现速度逐渐变慢，这个时候看超清视频，可能都有点吃力，但是对Google搜索影响不大。如果速度很慢，利用[站长工具](http://ping.chinaz.com/)进行IP检测，如果实在不能忍受，在谷歌云里删除该VPS服务器，重新配置即可。

另外，这种方法，也可以在手机（安卓）上进行配置，但是运营商会限速，只是用来搜索，基本没啥大问题。

时间仓促，水平有限，有错误，请随时联系我：<kangzhiheng@live.cn>。

希望大家可以正确利用技术。

希望这个方法带给大家帮助。

### 参考

[1] [互联网是如何工作的？VPN、SSR、V2ray翻墙是什么原理？](https://www.youtube.com/watch?v=iidqJ7p7ln4)

[2] http://www.ggfwzs.com/ 

[3] [浅谈vpn-vps-proxy以及shadowsocks之间的联系和区别](https://medium.com/@thomas_summon/浅谈vpn-vps-proxy以及shadowsocks之间的联系和区别-b0198f92db1b)

[4] [手把手教你搭建搬瓦工-ss-代理](https://github.com/frank-lam/vps-ss#手把手教你搭建搬瓦工-ss-代理)

[5] [SS和SSR的区别](https://www.idcbest.com/idcnews/11003957.html)

[6] [免费翻墙，谷歌云+SSR](https://www.youtube.com/watch?v=RxbGtkRVUWQ&t=291s)

[7] [V2Ray](https://toutyrater.github.io/)