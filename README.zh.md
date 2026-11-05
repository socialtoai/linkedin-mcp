# LinkedIn MCP：读公开帖子、公司主页和广告库

[English](README.md)

让 AI 读 LinkedIn 上公开可见的内容：某个话题下的帖子、公司主页发了什么、LinkedIn 广告库里的广告。**不用 LinkedIn Cookie，不用你的账号，不会牵连你的账号。只读公开内容，不发帖。**

SocialToAI 团队发布。服务是托管的，这里没有需要安装的东西；这个仓库放配置方法、用法和限制。

## 先看：它做不到什么

很多人搜 LinkedIn MCP 是想查人。这个服务**不提供**：

- 个人的档案、工作经历、联系方式；
- 任何人的人脉，帖子下面的评论；
- 你自己的首页、私信和数据后台；
- 发帖、点赞、加好友、发消息。

这里的 `user_profile` 和 `user_posts` 指的都是**公司主页**。如果要操作自己的账号，用 LinkedIn 官方的登录和发帖权限，或者用自己登录态的工具更合适，同时要承担相应的账号风险。

## 接入

地址：`https://mcp.socialtoai.com/mcp`

```bash
# Claude Code：运行后输入 /mcp，选 socialtoai，登录
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp

# Codex：浏览器会打开登录页
codex mcp add socialtoai --url https://mcp.socialtoai.com/mcp
```

其他客户端见[接入页](https://socialtoai.com/zh/mcp/linkedin/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106)。新账户送 100 积分，不用绑卡。

## 四个用法

**1. 一个话题大家怎么说。** “LinkedIn 上大家怎么看 AI SDR？把观点归类，每条带链接。” → `search`，传 `query`，可加 `published_within`。每页约 10 条，一页 2 积分。

**2. 竞品公司主页在说什么。** “这家公司的 LinkedIn 主页最近发了什么？总结主题。” → `user_posts`，传 handle（链接里 `/company/` 后面那段）。LinkedIn 公开最多显示最新 10 条，这个结果不带点赞和评论数，适合看主题，不适合看效果。

**3. 一家公司在投什么广告。** “在 LinkedIn 广告库里搜 AI 会议纪要相关的广告：谁在投，承诺了什么？” → `call` `linkedin.ad_search`，再用 `linkedin.ad_detail` 细看。这是 LinkedIn 为透明度公开的库，能看到一家公司在付费投放里承诺什么。

**4. 视频帖里说了什么。** “这个 LinkedIn 视频讲了什么？” → `detail` 带 transcript，前提是 LinkedIn 有这条视频的字幕。

## 一段真实录制

2026-10-30 在线上真实跑的，原文链接见 [LinkedIn MCP 页面](https://socialtoai.com/zh/mcp/linkedin/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106)。

> **问：** LinkedIn 上大家怎么看 AI SDR？
>
> **调用：** `search`，`query: "AI SDR"`，返回 10 条公开帖子。
>
> **答案要点：** 三种看法反复出现：第一波承诺过头（10 条里 3 条）、现在用在入站线索上有效（3 条）、要每天调才有用（2 条）。每种看法都能点回原帖。10 条是小样本，适合找论点，不适合算比例。
>
> **花费：** 2 积分，$0.0067。

## 费用

2026-11-05 查询 [`/v1/capabilities`](https://api.socialtoai.com/v1/capabilities)，价格版本 `pv_29229c6acba5`。

| 计费项 | 读什么 | 积分 | $10 档折合 |
| --- | --- | --- | --- |
| `linkedin.search` | 按关键词搜公开帖子 | 2 | $0.0067 |
| `linkedin.detail` | 按活动 ID 或链接读一条帖子 | 2 | $0.0067 |
| `linkedin.detail:transcript` | 视频帖里说了什么 | 2 | $0.0067 |
| `linkedin.user_profile` | 一个公司主页 | 2 | $0.0067 |
| `linkedin.user_posts` | 公司主页最新帖子 | 2 | $0.0067 |
| `linkedin.ad_search` | 搜广告库 | 2 | $0.0067 |
| `linkedin.ad_detail` | 一条广告 | 2 | $0.0067 |

赠送的 100 积分约 50 次调用。$10 = 3,000 积分。失败不扣；正常查询但没有结果也算一次。

## 限制

- LinkedIn 没有 `trending` 和 `comments`。调用会回答不支持，不收费。
- 帖子搜索给正文、作者、时间、点赞和评论数。浏览、分享、转发、收藏、认证、作者粉丝数都是 `null`，意思是“没有提供”，不是 0。
- 公司主页不返回粉丝数和发帖数。
- `published_within` 的时间按搜索引擎标注，搜索结果只是 LinkedIn 的样本，不是完整记录。
- 请在 LinkedIn 条款和适用的隐私法律范围内使用数据。没有合法依据时，不要用它整理要联系的人员名单。

## LinkedIn 自己的 API

LinkedIn 自助开通的 API 权限只覆盖登录和代成员发帖，不包括读取其他成员的帖子。详情和来源：[LinkedIn 有 API 吗？](https://socialtoai.com/zh/does-linkedin-have-an-api/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106)

## 链接

- [LinkedIn API 文档](https://socialtoai.com/zh/platforms/linkedin/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106)
- [全部 18 个平台](https://github.com/socialtoai/socialtoai-mcp)
- 支持：support@socialtoai.com

本仓库的文字和示例采用 MIT 许可。SocialToAI 团队发布。
