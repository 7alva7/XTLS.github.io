---
date: "2020-12-23T00:00:00.000Z"
description: Project X 的文档.
title: Trojan
weight: 7
---

[Trojan](https://trojan-gfw.github.io/trojan/protocol) 协议

{{% notice danger important %}}
Trojan 被设计工作在正确配置的加密 TLS 隧道
{{% /notice %}}

## InboundConfigurationObject

---

```json
{
  "clients": [
    {
      "password": "password",
      "email": "love@xray.com",
      "level": 0,
      "flow": "xtls-rprx-direct"
    }
  ],
  "fallbacks": [
    {
      "dest": 80
    }
  ]
}
```

{{% notice dark %}} `clients`: \[ [ClientObject](#clientobject) \]{{% /notice %}}

一个数组，代表一组服务端认可的用户.

其中每一项是一个用户 [ClientObject](#clientobject)。

{{% notice dark %}} `fallbacks`: \[ [FallbackObject](../../fallback) \]{{% /notice %}}

一个数组，包含一系列强大的回落分流配置（可选）。

{{% notice  %}}
**TIP**\
Xray 的 Trojan 有完整的 fallbacks 支持，配置方式完全一致。</br>
触发回落的条件也与VLESS类似：首包长度 < 58 或第 57 个字节不为 '\r'（因为 Trojan 没有协议版本）或身份认证失败。
{{% /notice %}}

<br />
### ClientObject
---

```json
{
  "password": "password",
  "email": "love@xray.com",
  "level": 0,
  "flow": "xtls-rprx-direct"
}
```

{{% notice dark %}} `password`: string{{% /notice %}}

必填，任意字符串。

{{% notice dark %}} `email`: string{{% /notice %}}

邮件地址，可选，用于标识用户

{{% notice dark %}} `level`: number{{% /notice %}}

用户等级，连接会使用这个用户等级对应的[本地策略](../../base/policy#levelpolicyobject)。

userLevel 的值, 对应 [policy](../../base/policy#policyobject) 中 level 的值. 如不指定, 默认为 0.

{{% notice dark %}} `flow`: string{{% /notice %}}

流控模式，用于选择 XTLS 的算法。

目前入站协议中有以下流控模式可选：

- `xtls-rprx-origin`：最初的流控模式，此时客户端仅可选择 `xtls-rprx-origin` 和 `xtls-rprx-origin-udp443` 这两种流控模式。该模式纪念价值大于实际使用价值
- `xtls-rprx-direct`：**推荐**，所有平台皆可使用的典型流控方式，此时客户端可选择任何流控模式

{{% notice warning %}}
**注意**

当 `flow` 被指定时，还需要将该入站协议的 `streamSettings.security` 一项指定为 `xtls`，`tlsSettings` 改为 `xtlsSettings`。详情请参考 [streamSettings](../../base/transport#streamsettingsobject)。

此外，目前 XTLS 仅支持 TCP、mKCP、DomainSocket 这三种传输方式。
{{% /notice %}}

<br />


