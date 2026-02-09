# FAQ / 常见问题

## 1. 主流期刊都有反爬虫功能，会不会受影响？

基本不会。搜索走的是 Semantic Scholar、Unpaywall、arXiv 的**官方 API**，不是爬网页。获取付费论文走的是学校 EZproxy 代理，从出版商角度看就是一个正常用户在浏览器里打开论文，跟你自己点开没区别。代码里还做了 2-5 秒随机延迟限速，不会触发风控。

## 2. Deep Research 出来的大部分都是预印本和 OA，这个有什么不同？

Deep Research 只能搜公开内容，所以大部分结果是预印本和 OA，而且它是封闭的，没法接 MCP。

paper-fetcher 的思路不一样——它是跑在 Claude Code 里的 MCP Server，等于给 AI 加了个"图书馆借书证"。同样的论文 Deep Research 只能看到摘要，接了 paper-fetcher 的 Claude 可以直接拿全文来分析。两个其实可以互补：Deep Research 做广度调研，Claude + paper-fetcher 做深度精读。

## 3. Elsevier 网站经常提示人机验证，能顺利跳过吗？

Elsevier 的人机验证确实比较烦。不过 paper-fetcher 获取 Elsevier 论文时走的是学校 EZproxy 代理，请求从学校 IP 出去，一般不会触发验证。而且很多时候 Unpaywall 能直接找到 Elsevier 论文的 OA 版本，根本不需要访问 Elsevier 官网。实在碰到验证过不去的话，也不会卡死，会降级返回元数据信息。

## 4. Semantic Scholar 的 API 怎么申请的？

Semantic Scholar 的公开 API 不需要申请 key 就能用，paper-fetcher 用的就是这个免费的公开接口。它有速率限制（每秒大概 1-3 次请求），但正常用完全够了。需要审批的是他们的 Partner API / Dataset API，但这个项目用不到那个。

## 5. EZproxy session 会过期吗？人不在电脑前怎么办？

会过期，通常几小时到一天（取决于学校设置）。过期后需要重新登录。可以在出门前运行 `paper-fetcher login` 预先登录保存 cookies。如果 session 过期且人不在，获取付费论文会失败，但 Open Access 和 arXiv 的论文仍然可以正常获取。

## 6. 支持其他学校吗？

支持！EZproxy 是全球大学通用的系统，只需要修改配置文件 `~/.paper-fetcher/config.json` 中的 `proxy_base` 字段：

```json
{
  "proxy_base": "http://your-proxy.university.edu/login?url="
}
```

## 7. 需要安装什么环境？

- Python >= 3.10
- Chrome 浏览器（EZproxy 登录时需要）
- 安装命令：`pip install -e .`
- 注册到 Claude Code：`claude mcp add paper-fetcher -- paper-fetcher-mcp`
