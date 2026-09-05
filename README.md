# vless-all-in-one

[![Version](https://img.shields.io/badge/version-v3.4.11-blue)](https://github.com/Felix666-ship-It/vless-all-in-one2/releases)
[![Platform](https://img.shields.io/badge/platform-Debian%20%7C%20Ubuntu%20%7C%20CentOS%20%7C%20Alpine-lightgrey)](#系统要求)

多协议代理服务端一键部署与管理脚本。菜单式操作，支持 15 种协议共存、多用户管理、流量统计、订阅服务、回落分流，协议实现版本安装时自动从各官方渠道获取最新版。

## 支持协议

| # | 协议 | 实现 | 说明 |
|---|------|------|------|
| 1 | VLESS + Reality | Xray-core | 推荐，抗封锁，无需域名 |
| 2 | VLESS + Reality + XHTTP | Xray-core | 多路复用 |
| 3 | VLESS + WS + TLS | Xray-core | CDN 友好，可作回落 |
| 4 | VMess + WS | Xray-core | 回落分流/免流 |
| 5 | VLESS-XTLS-Vision | Xray-core | 支持回落 |
| 6 | SOCKS5 | Xray-core | 本地/内网穿透场景 |
| 7 | Shadowsocks 2022 | Xray-core | 新版加密，需时间同步 |
| 8 | Hysteria2 | sing-box | UDP 高速 |
| 9 | Trojan | Xray-core | 支持回落 |
| 10 | Trojan + WS | Xray-core | WebSocket+TLS，支持 CDN 转发 |
| 11 | Snell v4 | Surge 官方二进制 | Surge 专属 |
| 12 | Snell v5 | Surge 官方二进制 | Surge 专属 |
| 13 | AnyTLS | anytls-go | 新协议 |
| 14 | TUIC v5 | sing-box | UDP 高速 |
| 15 | NaiveProxy | Caddy + forwardproxy | 浏览器指纹伪装 |

> 各协议安装/更新时均实时从其官方渠道查询最新版本（如 Snell 取自 Surge 官方
> release notes、AnyTLS 取自 anytls-go GitHub Releases），无需随脚本升级协议版本。

## 系统要求

- Root 权限
- Debian 11+ / Ubuntu 20.04+ / CentOS 8+ / Alpine（systemd 或 OpenRC）
- 建议内存 512MB 以上
- VLESS-WS / VMess-WS / Trojan / Trojan-WS / NaiveProxy 的订阅功能需要域名与证书；VLESS-Reality / Snell 等无需域名

## 快速开始

```bash
wget -O vless-server.sh https://raw.githubusercontent.com/Felix666-ship-It/vless-all-in-one2/main/vless-server.sh && chmod +x vless-server.sh && bash vless-server.sh
```

安装任一协议后，脚本会自动把自身复制为快捷命令 `vless`，之后在任意目录输入 `vless` 即可打开管理菜单。

## 主菜单功能

| 菜单 | 功能 |
|------|------|
| 1 | 安装新协议（多协议共存） |
| 2 | 核心版本管理（Xray / Sing-box，稳定版/测试版/指定版本） |
| 3 | 卸载指定协议 |
| 4 | 用户管理（多用户 / 流量 / 到期通知） |
| 5 | 查看协议配置 |
| 6 | 订阅服务管理 |
| 7 | 管理协议服务（启动/停止/重启/端口修改） |
| 8 | 分流管理（回落后备线路） |
| 9 | CF Tunnel (Argo) |
| 10 | BBR 网络优化 |
| 11 | 查看运行日志 |
| 12 | 检查脚本更新 |
| 13 | 完全卸载 |

## 更新脚本

- 菜单内选择「检查脚本更新」，会从本仓库拉取最新版本
- 或重新执行「快速开始」中的命令覆盖后重启菜单
- 版本发布以 tag 为准（如 `v3.4.11`）

## 命令行参数

```text
--sync-traffic        同步流量数据到数据库（用于定时任务）
--show-traffic        显示实时流量统计
--check-expire        检查并禁用过期用户（用于定时任务）
--setup-expire-cron   安装过期检查定时任务
--help, -h            显示帮助信息
```

## 文件位置

```text
/etc/vless-reality/                 配置与数据目录
├── config.json                     Xray/sing-box 运行配置
├── db.json                         协议与用户数据库
├── certs/                          证书目录
└── subscription/                   订阅文件
/usr/local/bin/xray                 Xray 核心
/usr/local/bin/sing-box             sing-box 核心（安装 UDP 协议后）
/usr/local/bin/vless                脚本快捷命令
vless-reality.service               代理服务（按协议命名）
vless-watchdog.service              看门狗服务
```

## 本 Fork 的改动

基于 [Chil30/vless-all-in-one](https://gitlab.com/chil30-group/vless-all-in-one) 维护：

- 兼容 Xray 26.x 的 `xray x25519` 输出格式变更（修复 Reality 密钥提取失败）
- 协议菜单扩充为 15 项，新增「Trojan + WS」独立入口
- 脚本自更新 / 版本检查指向本仓库

## 免责声明

本项目仅供学习交流与个人使用，请遵守所在地区法律法规。使用本项目产生的任何后果由使用者自行承担。
