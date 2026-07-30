# 2025 Clash 机场各种协议差别介绍与选择指南（Vmess / Vless / Trojan / Reality / Hysteria）

## Clash 介绍

Clash 是一款跨平台的规则化网络代理客户端，主要用于科学上网、网络调试、流量转发等目的。它采用 YAML 格式进行配置，支持多种代理协议，并通过灵活的规则系统实现自动选择最佳节点。Clash 的设计理念强调高可扩展性和稳定性，深受高级用户与开发者欢迎。

### Clash 的应用场景

- 科学上网突破网络封锁
- 网站加速与内容解锁
- 国际数据传输优化
- 多节点选择与负载均衡
- 网络请求调试

## Clash 支持的协议概览（总览图先看这里）

Clash 支持众多主流代理协议，使其可以适配不同类型的机场服务与传输需求。常见协议包括：

- Vmess（V2Ray 核心协议）
- Vless（更轻量的无状态版本）
- Shadowsocks（SS）
- ShadowsocksR（SSR）
- Trojan
- SOCKS5
- HTTP(S) 代理
- Hysteria / TUIC（基于 UDP 的高性能协议）
- Reality（新一代伪装式抗审查协议）

### 协议对比简表（基础）

| 协议       | 加密机制   | 伪装能力 | UDP支持 | 隐蔽性 | 安全性 |
|------------|------------|----------|---------|--------|--------|
| Vmess      | 有         | 中       | 支持    | 中     | 高     |
| Vless      | 无（需TLS）| 强       | 支持    | 高     | 高     |
| Shadowsocks| 有         | 弱       | 支持    | 中     | 中     |
| Trojan     | TLS        | 强       | 支持    | 高     | 高     |
| Hysteria   | QUIC+UDP   | 中       | 强      | 中     | 高     |
| Reality    | TLS+X25519 | 极强     | 支持    | 极高   | 极高   |

## Vmess

Vmess 是 V2Ray 项目的核心协议之一，具备加密传输、身份验证等功能，广泛应用于中高端机场服务中。

### 工作原理

- 客户端与服务端建立连接后，通过动态生成的加密信息进行身份验证。
- 支持多种传输方式：TCP、mKCP、WebSocket、HTTP/2、QUIC 等。
- 默认启用 AES-128-GCM 或 ChaCha20 加密算法。

### 安全性分析

Vmess 具备中等至高等级的安全性，避免明文传输并且内置时间戳机制以防止重放攻击。但相较于后起的 Vless 与 Reality，在伪装能力上略逊一筹。

### 优缺点

**优点：**
- 加密算法内建，配置简单。
- 与 V2Ray 完美兼容，部署广泛。
- 支持多种传输方式与混淆手段。

**缺点：**
- 易被主动探测识别（在无 TLS 伪装时）。
- 配置更新缓慢，依赖 V2Ray 核心升级。

## Vless

Vless 是 V2Ray 项目对 Vmess 协议的轻量替代版本，采用无状态设计，并不包含加密层，必须结合 TLS 或 XTLS 实现安全传输。

### 特性与改进

- 移除了加密逻辑，完全依赖外部加密（如 TLS）。
- 减少客户端和服务器之间的数据交互负担。
- 支持 XTLS，为绕过防火墙提供更高隐蔽性。

### Vless 与 Vmess 的差异

| 特性             | Vmess      | Vless      |
|------------------|------------|------------|
| 是否加密         | 是         | 否（需TLS）|
| 状态机制         | 有状态     | 无状态     |
| 伪装能力         | 中         | 强         |
| TLS/XTLS支持     | 支持       | 强制       |
| 性能优化         | 一般       | 更优       |

### TLS 与 XTLS 的优势

- **TLS**：通用加密传输标准，可结合 CDN 进行伪装，适合 Web 环境中传输。
- **XTLS**：专为代理协议设计的扩展传输层，具备低延迟、高隐蔽、低功耗等优势，适合高性能节点使用。

---

## Shadowsocks (SS)

Shadowsocks 是一款轻量级、高性能的加密代理协议，最早由中国开发者 clowwindy 创建，旨在突破网络审查和屏蔽。

### 基本原理

- 使用 SOCKS5 代理协议接口。
- 客户端将请求通过加密算法发送至服务端，服务端解密并转发请求。
- 双向通信均加密，避免明文泄露。

### 加密算法选择

Shadowsocks 支持多种对称加密算法，包括：

- AES-256-GCM
- ChaCha20-IETF-Poly1305
- AES-128-GCM

推荐使用 GCM 或 Poly1305 类别算法，以获得更高安全性和性能。

### SS 与 Vmess/Vless 对比

| 特性         | Shadowsocks   | Vmess       | Vless       |
|--------------|---------------|-------------|-------------|
| 加密机制     | 客户端自选    | 内建        | 外部依赖    |
| 隐蔽性       | 较弱          | 中          | 强          |
| 协议扩展性   | 差            | 强          | 强          |
| 性能         | 高            | 中          | 高          |

**优点：**
- 简洁、轻量、易于部署。
- 广泛兼容客户端、平台和路由器。

**缺点：**
- 缺乏高级伪装能力，容易被审查识别。
- 不支持多路复用和动态节点切换。

## ShadowsocksR (SSR)

ShadowsocksR 是 Shadowsocks 的非官方增强版本，由 BreakWa11 开发，加入了多项混淆机制和协议扩展，目的是增强抗封锁能力。

### SSR 的特点

- 支持多种协议混淆（如 `auth_sha1_v4`, `http_simple`）。
- 支持混淆插件与流量分流。
- 对不同流量特征进行模拟，降低被探测风险。

### 应用场景

- 高封锁环境（如中国大陆）中绕过 GFW。
- 与 CDN、TLS 伪装结合效果更佳。
- 多为私有协议，不建议用于公共环境。

**注意**：由于 SSR 属于非主流协议，已基本停止更新，新机场和客户端逐步弃用。

## Trojan

Trojan 是一种基于 TLS 加密层的代理协议，设计理念是“不可识别”，即让代理流量看起来像普通 HTTPS 流量，最大程度避免被 DPI（深度包检测）识别。

### 工作机制

- Trojan 基于 TLS 1.3 建立连接。
- 使用域名证书伪装为普通 HTTPS 网站（如 www.google.com）。
- 流量高度混淆，适合 CDN 中转与前置伪装。

### 安全性分析

- 完整依赖 TLS 加密，具备高安全等级。
- 可集成 Let's Encrypt 免费证书，易于部署。
- 几乎无法通过特征识别进行封锁（只要配置合理）。

### Trojan 与 Vless 的差异

| 特性       | Trojan      | Vless       |
|------------|-------------|-------------|
| TLS伪装     | 必须        | 可选        |
| 支持CDN     | 是          | 是          |
| 加密层      | 原生TLS     | 依赖外部    |
| 性能        | 高          | 更高（XTLS）|
| 易部署性    | 中          | 一般        |

**优点：**
- 安全性极高，天然伪装 HTTPS。
- 可与主流 Web 服务器共存（如 Nginx）。

**缺点：**
- 对服务器 TLS 配置要求较高。
- 初学者配置门槛较大。

## HTTP/HTTPS Proxy

传统的 HTTP/HTTPS 代理是一种较为古老的代理协议形式，主要用于 Web 请求代理传输。

### HTTP 代理介绍

- 仅支持 TCP 连接（主要为 Web 请求）。
- 不支持 UDP、流媒体、游戏等需求。
- 安全性较低，容易被监控与识别。

### HTTPS 代理介绍

- 增加 TLS 加密层，数据安全性稍强。
- 部分客户端支持 CONNECT 方法穿透 TLS。

### 与现代代理协议的对比

| 特性         | HTTP/HTTPS Proxy | Vmess/Vless/Trojan |
|--------------|------------------|--------------------|
| 加密支持     | 仅 HTTPS         | 原生或外接支持     |
| UDP支持      | 不支持           | 支持               |
| 伪装能力     | 弱               | 强                 |
| 易识别性     | 高               | 低                 |

**优点：**
- 配置简易，客户端原生支持。
- 在企业、校园网中可短期应急使用。

**缺点：**
- 缺乏安全与隐蔽性。
- 不适合长期科学上网使用。

---

## SOCKS5

SOCKS5 是一种通用型代理协议，最初由 NEC 开发，用于客户端与服务器之间转发任意协议的网络请求。

### 工作机制

- 支持 TCP 和 UDP 流量。
- 不对流量进行加密，仅做数据转发。
- 可实现基于用户名与密码的简单认证。

### 应用场景与限制

- **优点：**
    - 协议简单、兼容性高。
    - 原生支持浏览器与多种客户端。
    - 支持 UDP（如 DNS、游戏）转发。

- **缺点：**
    - 无加密与伪装机制，易被识别。
    - 不适合公共网络环境中使用。
    - 安全性完全取决于传输链路。

适用于本地转发、内网代理、中继链构建等需求，但不建议单独用于科学上网主通道。

## Reality (Xray协议)

Reality 是由 V2Ray 项目的衍生版本 Xray-core 所推出的新一代抗审查代理协议，结合 TLS 1.3 与曲线加密，实现极致伪装与匿名性。

### 协议简介

- 基于 TLS 1.3 + X25519 + Vision 混淆技术。
- 模拟真实的 HTTPS 流量（如模拟 Cloudflare 的访问）。
- 使用 Service Name Indication（SNI）与 Server Name 分离实现最大隐蔽性。

### 对抗审查能力

- Reality 通过抓包也难以与真实 HTTPS 区分。
- 已成为中国大陆等强审查环境中最具前景的协议之一。
- 可动态使用公网真实服务器进行“透明代理”。

### 优点：

- 极高隐蔽性与抗干扰性。
- 支持 TLS Vision 特征变形。
- 可绕过主动探测、深度包检测。

**缺点：**
- 配置复杂，对服务器要求高。
- 客户端需使用兼容 Reality 的 Xray 核心。

## Hysteria

Hysteria 是一款基于 UDP、专为高延迟网络优化的代理协议。尤其适用于游戏加速、海外远程访问等场景。

### 协议特性

- 基于 QUIC 协议，内建 UDP 加密支持。
- 自动丢包重传与拥塞控制机制。
- 默认使用 TLS 进行传输加密。

### 性能优势

- 极低延迟、抗丢包能力强。
- 可自适应流量质量，保持稳定连接。
- 在网络波动较大的环境（如移动网络）中表现优越。

### 使用场景

- 游戏加速（如 Steam、战网等）。
- 跨境视频通话、直播推流。
- 对 UDP 需求高的 App（如 WhatsApp、Telegram）。

**注意**：需支持 UDP 的服务器端口，否则无法正常运行。

## TUIC

TUIC 是近年来新兴的代理协议，主打基于 UDP 的高性能加密传输，被视为 Hysteria 的“竞品”。

### 协议概述

- 采用 QUIC 协议栈，内建 TLS 加密。
- 具备多路复用、0-RTT 建连等先进特性。
- 更注重稳定性与安全性。

### Hysteria vs TUIC

| 特性             | Hysteria       | TUIC           |
|------------------|----------------|----------------|
| 协议基础         | UDP + TLS      | QUIC + TLS     |
| 多路复用         | 支持           | 更强           |
| 抗丢包机制       | 自动           | 更智能         |
| 性能稳定性       | 高             | 极高           |
| 隐蔽性           | 中             | 中             |

### 使用建议

- TUIC 更适合严苛环境下的长时间稳定连接。
- 若对传输质量要求极高，建议优先选择 TUIC。
- 若需求为低延迟响应，则 Hysteria 依然有优势。

---

## Clash 配置各类协议

Clash 使用 YAML 配置文件支持多种协议组合。以下为各类型协议的配置示例与注意事项：

### 配置示例：Vmess 节点

```yaml
proxies:
  - name: "Vmess 节点"
    type: vmess
    server: example.com
    port: 443
    uuid: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    alterId: 0
    cipher: auto
    tls: true
    network: ws
    ws-opts:
      path: "/ws"
      headers:
        Host: example.com
```

### 配置示例：Vless + XTLS

```yaml
  - name: "Vless XTLS"
    type: vless
    server: example.com
    port: 443
    uuid: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    flow: xtls-rprx-vision
    tls: true
    network: tcp
    servername: example.com
```

### 配置示例：Trojan

```yaml
  - name: "Trojan TLS"
    type: trojan
    server: example.com
    port: 443
    password: "your_password"
    sni: example.com
    skip-cert-verify: false
    network: tcp
```

### 常见问题

- Clash Premium 版本才能支持 TUIC、Reality、Hysteria 等新协议。
- 某些协议（如 XTLS）不兼容特定客户端，如 Clash Meta。
- 注意填写正确的 ServerName 与路径字段，避免 handshake 失败。

## 对比总结

对各协议的综合特性进行整理，便于用户依据自身需求做出选择。

| 协议      | 加密 | UDP | 多路复用 | 隐蔽性 | 安全性 | 适用场景             |
|-----------|------|-----|----------|--------|--------|----------------------|
| Vmess     | 有   | 是  | 支持     | 中     | 高     | 通用网络代理         |
| Vless     | 无   | 是  | 支持     | 高     | 高     | 需要伪装、低延迟场景 |
| Trojan    | TLS  | 是  | 支持     | 高     | 高     | 高安全HTTPS伪装需求 |
| SS        | 有   | 是  | 无       | 低     | 中     | 简洁代理、移动设备   |
| SSR       | 有   | 是  | 无       | 中     | 中     | 高封锁环境绕墙       |
| Hysteria  | TLS  | 是  | 支持     | 中     | 高     | 游戏、实时通信       |
| TUIC      | TLS  | 是  | 强       | 中     | 高     | 高稳定性传输         |
| Reality   | TLS  | 是  | 支持     | 极高   | 极高   | 抗审查、极限伪装     |
| SOCKS5    | 无   | 是  | 无       | 低     | 低     | 内网、本地中转       |
| HTTP(S)   | TLS  | 否  | 无       | 低     | 低     | 临时Web代理           |

## 结论

### 协议选择建议

- **新手用户**：推荐使用 Trojan + TLS 或 Vless + TLS，兼顾安全与隐蔽。
- **老用户/折腾党**：建议尝试 Reality（需 Clash Premium）或 TUIC，拥有最前沿性能。
- **游戏加速**：优先选择 Hysteria 或 TUIC，优化 UDP 丢包问题。
- **轻量配置**：Shadowsocks 仍具价值，适用于移动设备或路由器。

### 后续发展趋势

- Reality 正在快速普及，或将成为主流抗审查协议。
- QUIC/UDP 类协议（如 TUIC、Hysteria）将在流媒体与高性能网络中占据优势。
- Clash 核心与 Meta、Premium 版本的生态逐渐分化，需关注官方兼容性更新。

---

## FAQs

**1. Clash Premium 与 Clash Meta 有什么区别？**  
Clash Premium 支持更多新协议如 TUIC、Reality、Hysteria，Meta 更侧重快速开发更新，二者配置兼容性略有不同。

**2. 为什么我的 Vless 节点连接失败？**  
检查 servername、UUID、端口是否正确，同时确认 TLS 是否启用，并确保服务器端配置无误。

**3. 哪些协议适合稳定科学上网抗审查？**  
Reality、Trojan、Vless + TLS 都具备较强伪装能力，是目前科学上网主流选择。

**4. Shadowsocks 还能用吗？**  
可以，但在高级审查环境中容易被识别，适合轻度使用或配合插件增强。

**5. Clash 是否支持移动端？**  
是，Clash 有 Clash for Android、Stash (iOS)、Shadowrocket (iOS) 等多款兼容客户端。

---

## 推荐阅读

- [机场推荐榜单 | 2026科学上网指南 ](https://tanqingbo.cn/Win-OpenInternet/)
- [Clash机场常用名称解释](https://github.com/OpenNetCN/freego/blob/main/mingci.md)


---

## 第二部分：Clash 机场术语字典（节点 / 模式 / 规则 / DNS / 线路 / 倍率 / 解锁）


## 1. 引言

### Clash 机场与科学上网概述

在数字化时代，全球信息高度互联，但由于网络审查或内容限制，许多互联网用户无法访问某些网站或服务。为了解决这一问题，Clash 机场应运而生。Clash 是一款高性能代理客户端，可通过配置远程代理节点，实现**科学上网**，绕过防火墙和地域限制。

所谓「机场」，是用户常见的对**付费代理服务提供商**的俗称，用户可以通过订阅这些机场提供的节点，实现高速、稳定、安全的翻墙体验。

### 为什么了解术语很重要

Clash 及其相关配置较为专业，涉及大量技术术语，如「Vmess」「Vless」「Reality」「Rule」「Fake-IP」等。理解这些术语不仅有助于正确配置软件，也能帮助用户选择最合适的机场服务，提高上网体验并避免安全隐患。

---

## 2. Clash软件简介

### Clash是什么？

Clash 是一个基于 Go 语言编写的规则驱动代理客户端，支持多种协议（如 Vmess、Vless、Trojan、Shadowsocks）。它允许用户通过配置文件，自定义规则进行流量分流、广告过滤、协议转换等操作，是目前科学上网领域最强大的工具之一。

### Clash与Clash for Windows、Clash Verge、Clash Meta的区别

| 软件名称 | 平台 | 特点 |
|----------|------|------|
| **Clash core** | 多平台 | 基础核心，命令行运行 |
| **Clash for Windows (CFW)** | Windows | 图形化界面，适合新手 |
| **Clash Verge** | Windows/macOS/Linux | 界面美观、支持Meta内核 |
| **Clash Meta** | 多平台 | 新一代核心，支持更多协议如Reality、Hysteria2 |

用户可根据自身需求选择合适的前端界面与核心版本。

---

## 3. 节点（Node）

### 节点的定义

节点是指一条代理连接的配置数据，通过该连接，用户可将流量转发到境外服务器，实现科学上网。一个节点包含了连接的服务器地址、端口、协议、UUID等参数信息。

### 常见节点类型

- **Vmess**：由V2Ray开发，常见于许多机场，兼容性好。
- **Vless**：Vmess的轻量升级版本，去除了加密，传输更灵活。
- **Trojan**：基于HTTPS协议，伪装性强，抗封锁能力好。
- **Shadowsocks (SS)**：较早期的加密代理协议，速度快但安全性稍弱。

---

## 4. 节点参数详解

### 常见字段说明

| 字段名 | 含义 |
|--------|------|
| `server` | 节点服务器地址 |
| `port` | 服务器端口号 |
| `uuid` | 用户唯一识别码（用于Vmess/Vless） |
| `alterId` | 额外ID（旧版Vmess专属） |
| `cipher` | 加密方式（如chacha20-ietf-poly1305） |
| `tls` | 是否启用TLS加密（true/false） |

### 节点订阅链接（Subscription URL）

机场通常提供一个订阅链接，用户将该链接粘贴至Clash配置界面即可获取全部节点信息，并可自动更新。链接格式通常以 `.yaml` 或 `.txt` 结尾。

---

## 5. 代理模式（Mode）

### Global、Rule、Direct 模式解释

Clash支持三种核心代理模式：

- **Global（全局模式）**：所有流量都通过代理节点转发。适合完全绕过防火墙，但访问国内网站时可能变慢。
- **Rule（规则模式）**：依据规则集自动判断哪些流量走代理，哪些不走。最推荐的模式。
- **Direct（直连模式）**：所有流量不经过代理，常用于测试连接或访问本地网络。

### 何时使用哪种模式？

| 使用场景 | 推荐模式 |
|----------|----------|
| 看Netflix、YouTube | Global |
| 中英文网站同时访问 | Rule |
| 使用银行/本地服务 | Direct |

---

## 6. 规则集（Rule Set）

### Rule的概念

规则（Rule）是Clash的核心之一，用于判断特定流量应通过哪个节点或是否走直连。规则可以是域名、IP范围、进程名称等形式。借助规则集，Clash可以做到“智能分流”，即国内流量直连，国外流量自动代理。

### 常见规则类型

| 类型 | 描述 | 示例 |
|------|------|------|
| DOMAIN | 匹配域名 | `DOMAIN,google.com` |
| DOMAIN-SUFFIX | 匹配域名后缀 | `DOMAIN-SUFFIX,youtube.com` |
| DOMAIN-KEYWORD | 包含关键字 | `DOMAIN-KEYWORD,google` |
| IP-CIDR | 匹配IP段 | `IP-CIDR,8.8.8.8/32` |
| GEOIP | 匹配国家或地区 | `GEOIP,CN` |
| PROCESS-NAME | 匹配进程 | `PROCESS-NAME,chrome.exe` |
| MATCH | 匹配所有流量（默认规则） | `MATCH` |

建议用户使用带有规则集订阅功能的机场，以确保规则长期维护和更新。

---

## 7. DNS配置

### DNS在Clash中的作用

Clash中的DNS模块决定域名如何被解析，并直接影响“域名规则”能否正确应用。错误的DNS解析可能导致无法命中规则或连接失败。

### Fake-IP与真实DNS的区别

- **Fake-IP 模式**：Clash将未匹配到真实DNS记录的域名生成一个伪造IP，从而确保规则命中率更高。推荐用于国内外流量混合场景。
- **真实DNS模式**：通过真实DNS服务器解析域名，但对流量分析要求较高，命中率可能不如Fake-IP。

```yaml
dns:
  enable: true
  listen: 0.0.0.0:53
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 114.114.114.114
    - tls://dns.rubyfish.cn:853
  enhanced-mode: fake-ip
```

---

## 8. 负载均衡与自动切换

### 负载均衡类型

Clash支持三种策略实现节点自动化管理：

- **URL-Test**：定期测试指定网址（如https://www.gstatic.com/generate_204）的响应速度，选择最快节点。
- **Fallback**：主节点失效后自动切换至备用节点。
- **Load-Balance**：将流量平均分配至多个节点（适合高频率分流需求）。

```yaml
proxies:
  - name: node1
    type: vmess
    server: 1.1.1.1
  - name: node2
    type: vmess
    server: 2.2.2.2

proxy-groups:
  - name: AutoGroup
    type: url-test
    proxies:
      - node1
      - node2
    url: 'http://www.gstatic.com/generate_204'
    interval: 300
```

### 节点测速与健康检查

测速逻辑基于响应时间、丢包率和可用性判断。Clash本身无法显示详细测速图表，但借助控制面板如 Yacd 可以实时查看延迟。

---

## 9. Tun模式与系统代理

### Tun模式的用途与配置

Tun模式可将系统层级所有应用的流量“透明接管”至Clash，实现比传统HTTP/HTTPS代理更强的全局穿透能力。尤其适用于不支持手动设置代理的游戏、软件或系统级服务。

#### Clash Meta启用Tun示例：

```yaml
tun:
  enable: true
  stack: system
  dns-hijack:
    - 198.18.0.2:53
```

### 系统代理设置方式

- **Windows**：Clash for Windows 启动后自动设置系统代理。
- **macOS/Linux**：需手动设置系统网络偏好中的HTTP和Socks代理。
- **Android**：Magisk模块（如Clash for Android内核）或VPN模式实现流量接管。

---

## 10. 外部控制面板

### Clash Dashboard控制面板

Clash运行时会暴露本地控制接口（REST API + Web GUI）。常见的Dashboard有两种：

1. **CFW自带面板**（默认本地127.0.0.1:9090）
2. **Yacd 面板**：简洁易用的第三方前端，可直接接入Clash配置。

### 使用Yacd、Meta-Yacd、ClashN等工具

- **Yacd**：最轻量的Web控制面板，支持实时切换节点、查看规则。
- **Meta-Yacd**：适配Clash.Meta的Yacd改版，支持Reality协议。
- **ClashN**：Windows下的Clash客户端集成界面，适合进阶用户。

---

---

## 11. 网络协议说明

### TCP、UDP的区别

- **TCP（Transmission Control Protocol）**：面向连接、传输稳定。适合网页浏览、视频播放等对传输完整性要求高的服务。
- **UDP（User Datagram Protocol）**：无连接、传输速度快但稳定性较差。适合游戏、实时语音等低延迟需求场景。

### TLS、XTLS、gRPC、H2等传输协议解析

| 协议 | 特点 | 常用于 |
|------|------|--------|
| **TLS** | 标准加密传输层协议，支持HTTPS伪装 | Trojan, Vmess |
| **XTLS** | 特殊加密技术，提升传输性能 | Vless (Reality协议) |
| **gRPC** | 谷歌提出的高性能协议，基于HTTP/2 | Trojan/Vless中继传输 |
| **H2 (HTTP/2)** | 低延迟、高并发连接 | Trojan, Vless, Reality |
| **Reality** | 新型XTLS替代方案，具备防探测、伪装能力 | Clash Meta支持 |

---

## 12. 加密方式（Cipher）

### 常见加密算法介绍

| 加密方式 | 描述 |
|----------|------|
| `aes-128-gcm` | 高安全性，对硬件要求较低，广泛兼容 |
| `chacha20-ietf-poly1305` | 适合移动设备的高性能加密方式 |
| `none` | 无加密，适用于Vless裸协议（通过TLS保障） |

### 如何选择合适的加密方式

- 对于低功耗设备：推荐 `chacha20-ietf-poly1305`
- 对于通用设备：`aes-128-gcm` 或 `aes-256-gcm` 是稳定之选
- 使用Vless/Reality等协议时，可选择 `none`，由TLS层加密保障

---

## 13. IP类型详解

### 住宅IP（家宽IP）与数据中心IP的区别

- **住宅IP（家宽）**：来自普通家庭网络，通常不易被识别为代理，适合访问敏感服务。
- **数据中心IP**：来自IDC机房，速度稳定但容易被服务商识别、限制。

### 原生IP、非原生IP的概念

- **原生IP**：由本国ISP分配的IP，具备完整网络权限（如原生美区IP可访问美国专属服务）。
- **非原生IP**：转接或托管方式分配，部分服务可能限制访问或出现内容缺失。

### 静态IP与动态IP区别

- **静态IP**：固定不变的IP地址，适合做端口转发、自建服务。
- **动态IP**：每次重连可能改变地址，适合普通用户但不利于持久映射。

---

## 14.  线路类型与传输路径说明

不同线路决定了机场速度、稳定性、抗封锁能力。常见线路说明如下：

###  常见线路类型对比

| 类型         | 路径结构                                  | 特点                           |
|--------------|-------------------------------------------|--------------------------------|
| **直连**     | 用户 → 境外节点                           | 延迟低，易被墙封，稳定差       |
| **中转**     | 用户 → 中转节点 → 境外节点                | 性价比高，抗封锁中等           |
| **BGP中转**  | 用户 → BGP中转 → 境外节点                 | 晚高峰稳定，适配三网           |
| **IEPL专线** | 用户 → 专线内网 → 境外节点                | 延迟低，稳定高，流媒体友好     |
| **IPLC专线** | 用户 → 物理专线 → 境外节点                | 企业级稳定，不易被墙           |
| **多跳隧道** | 用户 → Socks → gRPC/WS → 境外节点         | 抗封锁强，速度一般             |
| **自建VPS**  | 用户 → 自建代理 → 网站                    | 灵活可控，需运维               |

### 🧠 常用术语简析

- **三网优化**：自动选择电信/联通/移动最优路径
- **回程优化**：提升海外访问国内资源速度
- **原生IP**：用于解锁 Netflix/ChatGPT/TikTok 等服务
- **抗封锁性**：专线 > BGP中转 > 普通中转 > 直连

### ✅ 推荐选型

- 新手：BGP中转 / 普通中转
- 重度使用：IEPL / IPLC
- 自由可控：自建VPS
- 解锁流媒体/AI工具：优先原生IP + 专线



---

## 15. 倍率（Ratio）说明

### 什么是倍率？

倍率指的是机场服务中，**一个节点所消耗流量与实际使用流量的比值**。例如1:5倍率表示用户使用1GB，计费为5GB。

### 倍率对流量计算的影响

- **倍率高**：流量消耗快但价格相对便宜
- **倍率低或1:1**：计费公平但服务更昂贵

### 常见倍率设置示例

| 区域/节点 | 倍率 | 特点 |
|-----------|------|------|
| 日本/韩国IEPL | 1:1 | 速度快，适合游戏/视频 |
| 美国/香港中转 | 1:3 ~ 1:5 | 解锁强，但流量消耗快 |
| ChatGPT/Netflix专线 | 1:10 或更高 | 高级内容节点，精准优化 |

用户应根据使用习惯和流量预算合理选择使用不同倍率的节点。

---
---

## 16. 解锁能力解析

### 什么是解锁？

“解锁”指的是使用代理节点访问**本地受限或仅限某地区可用的服务内容**。例如：Netflix美区、TikTok国际版、ChatGPT、YouTube Music、Midjourney 等。

### 支持解锁的服务类型

- **流媒体平台**：Netflix、Disney+、YouTube、HBO Max、BBC iPlayer
- **社交与视频平台**：TikTok、Instagram、Facebook
- **AI与开发平台**：OpenAI（ChatGPT）、Midjourney、Claude、GitHub Copilot、Bing AI
- **金融与电商服务**：PayPal、Amazon、Stripe、eBay（取决于节点IP归属地）

### 如何判断节点是否支持解锁？

- 使用 Clash 控制面板的“测速检测”功能，或外部工具如：
    - [Netflix Checker](https://github.com/sjlleo/netflix-verify)
    - [TikTok Region Test](https://test.ipw.cn)
    - Speedtest + IP查询看是否为原生IP/住宅IP
- 节点备注中常有标注，如 `HK(解锁TikTok)`, `US(Netflix 全解)`

### 解锁失败的常见原因与对策

| 原因 | 解决办法 |
|------|----------|
| IP非原生/被污染 | 更换节点或联系机场反馈 |
| DNS泄漏导致识别为本地 | 使用Fake-IP或DoH DNS |
| 缓存地区信息 | 清除浏览器缓存/更改语言偏好 |
| 服务已封禁该段IP | 尽量使用住宅或IEPL/IPLC线路 |

---

## 17. 常见错误与诊断

### 节点无法连接的常见原因

1. 节点配置错误（端口/UUID/cipher等参数）
2. 节点失效或被墙（国内网络封锁）
3. Clash配置文件加载异常
4. 代理规则冲突或DNS解析失败

### 日志查看与调试技巧

- Clash GUI面板通常提供日志窗口，查看节点连接状态。
- `Clash.Meta` 和 `CFW` 都支持 `verbose` 日志级别，用于排查连接细节。
- 使用 `ping`、`tracert`、`curl` 等命令测试网络连通性。
- 可查看 `/log` 或终端返回的握手信息定位问题源头。

---

## 18. 高级功能

### GeoIP与GeoSite

- **GeoIP**：基于IP地址归属国家的流量识别，例如 `GEOIP,CN` 用于识别中国大陆流量。
- **GeoSite**：基于域名规则的预设站点分类集，如 `geosite:netflix` 用于一键识别所有Netflix相关流量。

```yaml
rules:
  - GEOIP,CN,DIRECT
  - GEOSITE,netflix,US-NODE
```

### 自定义配置文件（config.yaml）技巧

- 将常用规则集和分组预设进配置文件中，无需每次手动切换
- 可定义多个 proxy-group 实现一键切换场景（如ChatGPT专用组、解锁组、游戏组）
- 结合 `rule-providers` 动态拉取远程规则集，便于维护更新

---

## 19. 安全性与隐私保护

### 如何防止DNS泄漏

DNS泄漏会暴露你的真实访问目的地，即便你使用代理，也可能让ISP知道你访问了某些网站。避免DNS泄漏的方法包括：

- 启用 Fake-IP 模式
- 配置可信的 DNS-over-HTTPS（DoH）服务器
- 设置系统 DNS 与 Clash DNS 一致，避免“操作系统直通”

### 隐私增强配置建议

- 不使用无加密或明文协议（如原始SOCKS/HTTP代理）
- 使用支持TLS/XTLS的协议（Trojan/Vless+Reality）
- 禁用日志记录功能（如配置中 `log-level: silent`）
- 选择支持“混淆”或“伪装”功能的节点（如gRPC伪装）

---

## 20. Clash机场相关术语汇总表

### 简明术语表（按字母或功能分类）

| 术语 | 含义 | 分类 |
|------|------|------|
| 节点 | 一个可用的代理连接配置 | 核心组件 |
| 订阅链接 | 获取节点信息的远程地址 | 管理机制 |
| Rule | 流量分流规则 | 配置功能 |
| TLS | 安全加密协议 | 传输层 |
| Fake-IP | Clash伪造DNS解析的机制 | 网络控制 |
| 原生IP | 被目标平台识别为真实用户IP | 解锁机制 |
| 倍率 | 节点流量计费比率 | 商业计费 |
| 中转 | 多跳传输优化机制 | 线路优化 |

### 推荐使用场景与提示

- 需要看视频推荐使用：香港、台湾、美国节点，倍率不高优先
- AI工具解锁推荐：美国原生IP+Reality协议节点
- 追求稳定可用推荐：IEPL、CN2 GIA、IPLC等中高端线路

---
---

## 21. 结语

### 持续学习Clash进阶知识

Clash不仅仅是一个翻墙工具，它是一整套高度灵活、功能强大的网络流量控制平台。随着协议发展与网络监管变化，Clash社区不断更新核心和配置方式。建议用户持续关注以下内容：

- Clash 核心更新（如 Meta、Premium 支持 Reality、Hysteria2）
- 新兴协议与加密算法（如 uTLS、gQUIC）
- 社区维护规则集和配置模板（如lhie1、ACL4SSR、DivineEngine）

### 推荐资源与社区

| 名称 | 链接 | 说明 |
|------|------|------|
| Clash GitHub | https://github.com/Dreamacro/clash | 官方核心项目 |
| Clash.Meta | https://github.com/MetaCubeX | Meta 版本维护 |
| Yacd 控制面板 | https://github.com/haishanh/yacd | Web控制前端 |
| ACL4SSR 规则集 | https://github.com/ACL4SSR/ACL4SSR | 分流规则集合 |
| Telegram 频道 | 搜索：Clash 中文频道 | 社区交流与机场评测 |
| YouTube 频道 | 搜索：科学上网 教程 Clash | 视频教学资源丰富 |

借助社区力量，保持配置更新与技术前沿，是长期稳定使用Clash机场的关键。

---

## FAQs

### 1. Clash机场和VPN有什么区别？
Clash机场基于代理协议分流流量，功能更强大、可定制，而VPN通常是全局加密隧道，适合一键使用但可控性较弱。

### 2. 为什么我的节点看不了Netflix？
可能使用的是非原生IP或被识别为代理IP，建议更换带有“解锁Netflix”标识的节点。

### 3. Clash中Fake-IP有什么风险？
Fake-IP是伪造DNS机制，不会带来安全风险，反而可避免DNS泄漏，提高分流命中率。

### 4. 什么是Reality协议？为什么它越来越流行？
Reality是基于VLESS协议的新型加密与伪装机制，具备更强抗封锁性和隐蔽性，已被大量机场采用。

### 5. 倍率越高是不是越好？
不一定。高倍率节点价格低但流量消耗快，适合轻度使用；低倍率节点适合追求体验和稳定性的用户。


## 推荐阅读

- [机场推荐榜单 | 2026科学上网指南 ](https://tanqingbo.cn/Win-OpenInternet/)
