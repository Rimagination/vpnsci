# vpnsci

学术论文全文获取工具，支持 100+ 中国高校。自动通过学校 WebVPN 获取付费论文，优先使用免费 Open Access 来源。

提供 [MCP](https://modelcontextprotocol.io/) Server，可接入 Claude Code、Cursor、Windsurf 等 AI Agent。

## 安装

```bash
pip install git+<repo-url>
```

## 快速开始

```bash
# 设置学校和邮箱
vpnsci config-cmd --school 你的学校
vpnsci config-cmd --email your@email.com

# 首次获取论文时会弹出浏览器，完成校园网登录即可
vpnsci fetch "10.1038/s41566-024-01234-5"
```

## 使用方式

### CLI

```bash
# 按 DOI 或 URL 获取论文
vpnsci fetch "10.1038/s41566-024-01234-5"
vpnsci fetch "https://www.nature.com/articles/s41566-024-01234-5"

# 搜索论文
vpnsci search "perovskite solar cells" --limit 10

# 搜索并获取全文
vpnsci search "organic photovoltaics" --fetch

# 批量获取（每行一个 DOI）
vpnsci batch dois.txt --output ./papers

# 查看支持的学校
vpnsci schools
vpnsci schools 北京
```

### MCP（AI Agent 集成）

```bash
# Claude Code
claude mcp add vpnsci -- vpnsci-mcp

# 其他 MCP 工具（Cursor、Windsurf 等）
# 在 MCP 配置中添加：
# { "mcpServers": { "vpnsci": { "command": "vpnsci-mcp" } } }
```

配置后重启 Agent，首次使用时告诉 Agent 你的学校即可：

> **你**: 帮我搜几篇钙钛矿太阳能电池的论文
> **Agent**: 你还没配置学校，请告诉我你的学校名称
> **你**: 兰州大学
> **Agent**: 已配置，现在帮你搜索...

### 需要 VPN 的学校

大部分学校直接使用即可。少数学校（浙大、南大、海大等）需要额外配置 VPN 代理，详见 [VPN 配置指南](#vpn-配置)。

## 支持的学校

内置 100+ 高校配置，包括清华、北大、复旦、浙大、上海交大等。搜索你的学校：

```bash
vpnsci schools 学校名称
```

## VPN 配置

部分学校需要通过 VPN 代理访问，配置方式：

```bash
# 启动 docker-easyconnect（需先安装 Docker）
docker run --rm -d --name easyconnect --privileged \
  -p 127.0.0.1:1080:1080 -p 127.0.0.1:8888:8888 \
  -e EC_VER=7.6.3 -e VPN_ADDR=你的学校VPN地址 \
  hagb/docker-easyconnect

# 浏览器打开 http://127.0.0.1:8888 完成登录

# 配置代理
vpnsci config-cmd --proxy-url socks5://127.0.0.1:1080
```

> 将 `你的学校VPN地址` 替换为学校 VPN 门户地址（如 `vpn.ouc.edu.cn`）。

## 环境要求

- Python >= 3.10
- Chrome 浏览器（首次登录需要）

## 免责声明

本项目是学术论文获取工具，帮助高校师生合法访问机构已订阅的学术资源。不包含任何 VPN 协议实现，不提供 VPN 连接功能。使用者应遵守相关法律法规和学校网络使用规范。

## 致谢

- [webvpn-converter](https://github.com/lcandy2/webvpn-converter) — 学校配置数据
- [Tuna-Erha-Bot](https://github.com/Konano/Tuna-Erha-Bot) — WebVPN 加密算法
- [ZJUWebVPN](https://github.com/eWloYW8/ZJUWebVPN) — 动态密钥方案
- [CASPaperTunneling](https://github.com/qiyang-ustc/CASPaperTunneling) — CAS 认证流程

## License

[MIT](LICENSE)
