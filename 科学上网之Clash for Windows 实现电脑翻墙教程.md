## ClashForWindows是什么？

ClashForWindows是一款基于Clash的Windows GUI代理客户端。它有以下几点优势：

1. 隐私保护：作为一款代理客户端，ClashForWindows可以帮助用户在浏览网页时隐藏自己的IP地址，保护用户的在线隐私。
2. 加密通信：ClashForWindows支持多种加密方式，包括但不限于Shadowsocks、VMess、Trojan等，可以保护用户的网络通信不被窃听。
3. 规则分流：ClashForWindows具有强大的规则分流功能，用户可以根据自己的需求设置不同的代理规则，防止不必要的数据泄露。
4. 透明代理：通过ClashForWindows，用户可以实现全局或者按需代理，有效避免了部分网站或应用对IP地址的限制。
5. 开源：ClashForWindows是开源软件，其源代码可以公开查阅，这意味着任何人都可以查看其内部实现，从而确保没有隐藏的恶意代码。

## ClashForWindows下载

下载链接：[Index of /clash_for_windows/](https://dl.p6p.net/clash_for_windows/)

![image-20251114142425945](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/image-20251114142425945.png)

建议下载中文版的，Clash for Windows 下载和安装之后，可用通过 **Clash for Windows 使用教程**配置好Clash for Windows。

## 添加配置文件

添加配置文件一般有如下两种方式：

- 远程订阅地址
- 本地配置文件

一般情况下，远程订阅地址（URL 导入）及本地配置文件（本地文件拖拽导入）都由节点服务商提供。

### 远程订阅地址

远程订阅地址即通过 URL 链接导入，一般的VPN服务商都会直接提供 Clash 节点地址，**如果你不知道哪些VPN服务商靠谱的话，推荐使用【一支红杏】VPN服务商，我自己用了七八年了，主打一个安全、稳定！**

- 一支红杏支持 Windows、MAC OS X、cross platform、Android、IOS、openwrt等系统，是非常实用的网络代理VPN。
- 网速很快，奈飞、电报、Google、youtubu、Facebook啥的都能上。

🔗 官网地址：https://order.yizhihongxing.club/aff.php?aff=4818

#### 第一步：注册一支红杏账号并获取订阅地址

1. 打开官网：https://order.yizhihongxing.club/aff.php?aff=4818
2. 翻到下面会有VPN的注册方式，如果是个人用的话，选择**入门版**就可以了，一个月100G足够用了。
   [![img](https://picx.zhimg.com/80/v2-244e645586693c51228d64ac5d41ca9d_720w.png)](https://picx.zhimg.com/80/v2-244e645586693c51228d64ac5d41ca9d_720w.png)
3. 购买之后依次点击上方导航栏中的 **产品与服务->我产品与服务->查看详情**，就能看到你购买的节点信息。

[![img](https://pica.zhimg.com/80/v2-2d44e88baa237bfe08fe7f5818f28d97_720w.png)](https://pica.zhimg.com/80/v2-2d44e88baa237bfe08fe7f5818f28d97_720w.png)

![image-20251114144345762](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/image-20251114144345762.png)

#### 第二步：Clash for Windows远程导入clash订阅地址

点击上述图片红框右侧中的【获得地址】按钮，接着复制地址：

![image-20251114144750655](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/image-20251114144750655.png)

打开Clash for Windows软件，然后点击界面左侧菜单 `配置`，在顶部输入框填入刚才复制的 `URL` 连接地址并点击 `下载` 即可，如下图所示。

![远程订阅地址](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/3.DxpisIlY.avif)

### 本地配置文件

本地配置文件即通过本地文件拖拽导入，一般为无法通过远程订阅地址导入的情况下使用，可尝试在浏览器中下载配置文件后直接通过拖拽方式导入或点击 `Import` 导入，如下图所示。

![image-20251114145115645](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/image-20251114145115645.png)

![本地配置文件](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/4.C9ncx4ev.avif)

### 选择代理节点

在添加为订阅地址之后，需要选择一个代理节点使用，点击软件主界面左侧的 `代理` 选项卡，软件右上角代理规则处默认保持 `规则` 即可，代理模式主要有以下四种：

- 规则：所有请求根据配置文件规则进行分流
- 全局：所有请求直接发往代理服务器
- 直连：所有请求直接发往目的地，即不使用代理
- 脚本：所有请求根据脚本文件规则进行分流

全局模式可能会导致国内流量也走代理访问，除了网络会变慢外，还会消耗套餐流量。规则模式的好处就是区分国内国外的流量只有在规则内的国外网站才会走代理，这样即不影响国内访问速度，又节省套餐流量，所以如果没有什么特别的需求，一般选择 `规则` 即可。

然后在展开的节点组之中任意单击鼠标左键选择一个节点即可，如下图所示：

![选择代理节点](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/5.XoxBgcHi.avif)

## 启用代理

启用代理，需要点击界面左侧菜单 `主页` 选项卡，找到 `系统代理` 并开启开关即可，开启状态下按钮状态为绿色，如下图所示为开启状态。

![启用代理](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/6.DGvFxM4D.avif)

启动代理后系统托盘的图标会变色金色猫咪，以下是系统托盘图标颜色说明，代理启动后，可以打开浏览器，访问 Google / YouTube / ChatGPT 测试VPN是否正常。

![图标](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/7.BT-gQHan.webp)

## 设置开机自动启动

设置开机自启动，需要点击界面左侧菜单 `主页` 选项卡，找到 `开机自启动` 并开启开关即可，开启状态下按钮状态为绿色，如下图所示为开启状态。

![开机自启动](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/8.C8ZL1Kfz.avif)

## 更新配置文件

点击界面左侧菜单 `配置`，点击 `更新全部` 即可更新所有配置文件，如下图所示。

![更新配置文件](https://cdn.jsdelivr.net/gh/tqb4342/BlogPhoto/9.3MrXh7te.avif)

## 其它设备设置VPN

### iOS/MAC 端

**注册教程：**[iphone如何用外网/苹果手机搭梯子-翻墙VPN软件工具](https://tanqingbo.cn/iphone-open-internet/)

### Android 端

**注册教程：**[安卓/Android系统手机实现科学上网教程-翻墙VPN软件工具](https://tanqingbo.cn/Android-open-internet/)

