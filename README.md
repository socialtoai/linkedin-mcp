# LinkedIn MCP server for public posts, company pages and the Ad Library

[中文](README.zh.md)

Let your AI read what is publicly visible on LinkedIn: posts on a topic, what a company page published, and ads in the LinkedIn Ad Library. **No LinkedIn cookies, no account of yours: your account stays out of it. It reads public content and does not post.**

Published by the SocialToAI team. The server is hosted, so there is nothing here to install; this repository is the setup, the recipes and the limits.

## Read this first: what it does not do

Plenty of people search for a LinkedIn MCP wanting person lookups. This server does **not** give you:

- a member's profile, work history or contact details,
- anyone's connections, or the comments under a post,
- your own feed, inbox or analytics,
- posting, liking, connecting or messaging.

`user_profile` and `user_posts` here mean **company pages**. If you need to act on your own account, LinkedIn's official sign-in and posting permissions, or a tool that uses your own session, are a better fit, with the account risk that comes with it.

## Connect

Address: `https://mcp.socialtoai.com/mcp`

```bash
# Claude Code: run this, then type /mcp, pick socialtoai and sign in
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp

# Codex: the browser opens for sign-in
codex mcp add socialtoai --url https://mcp.socialtoai.com/mcp
```

Every other client: [connect page](https://socialtoai.com/mcp/linkedin/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106). New accounts get 100 free credits, no card.

## Four recipes

**1. What people say about a topic.** "What are people on LinkedIn saying about AI SDRs? Group the views and link each one." → `search` with `query`, optionally `published_within`. About 10 posts a page, 2 credits a page.

**2. What a competitor's company page is saying.** "What has the Unesco company page posted lately? Summarise the themes." → `user_posts` with the handle (the part after `/company/`). LinkedIn shows at most the latest 10 publicly, and these results carry no like or comment counts, so judge themes, not performance.

**3. What a company is advertising.** "Find ads in the LinkedIn Ad Library about AI meeting notes: who runs them and what they promise?" → `call` `linkedin.ad_search`, then `linkedin.ad_detail` for a closer look. LinkedIn publishes this library for transparency, and it shows what a company is promising in paid campaigns.

**4. What was said in a video post.** "What does this LinkedIn video say?" → `detail` with the transcript include, where LinkedIn has captions for it.

## A recorded answer

Recorded on the live service on 2026-10-30. It is on [the LinkedIn MCP page](https://socialtoai.com/mcp/linkedin/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106) with its source links.

> **Asked:** What are people on LinkedIn saying about AI SDRs?
>
> **Call:** `search` with `query: "AI SDR"`: 10 public posts.
>
> **Answer, in short:** three views keep coming back. The first wave over-promised (3 of 10), it works now for inbound leads (3 of 10), and it only pays off with daily tuning (2 of 10). Each view links to its posts. Ten posts is a small sample: use it to find the arguments, not to measure them.
>
> **Spent:** 2 credits, $0.0067.

## Credits

Checked on 2026-11-05 against [`/v1/capabilities`](https://api.socialtoai.com/v1/capabilities), price version `pv_29229c6acba5`.

| Price key | What it reads | Credits | On the $10 pack |
| --- | --- | --- | --- |
| `linkedin.search` | Public posts by search terms | 2 | $0.0067 |
| `linkedin.detail` | One post by activity ID or link | 2 | $0.0067 |
| `linkedin.detail:transcript` | What is said in a video post | 2 | $0.0067 |
| `linkedin.user_profile` | One company page | 2 | $0.0067 |
| `linkedin.user_posts` | A company page's latest posts | 2 | $0.0067 |
| `linkedin.ad_search` | Ad Library search | 2 | $0.0067 |
| `linkedin.ad_detail` | One ad | 2 | $0.0067 |

100 free credits is about 50 calls. $10 buys 3,000 credits. Failed calls are free; a search that ran and found nothing is charged.

## Limits

- `trending` and `comments` are not offered on LinkedIn. Calling them answers "not supported" and costs nothing.
- Post search gives text, author, time, reaction and comment counts. Views, shares, reposts, saves, verification and author follower counts are `null`, which means "not given", not zero.
- Company pages do not return follower or post counts.
- Post dates in `published_within` are as a search engine dates them, so treat search as a sample of LinkedIn, not a complete record.
- Use the data within LinkedIn's terms and the privacy law that applies to you. Do not use it to build lists of people to contact without a lawful basis.

## LinkedIn's own API

LinkedIn's self-serve API permissions cover signing in and posting on a member's behalf, not reading other members' posts. Details and sources: [Does LinkedIn have an API?](https://socialtoai.com/does-linkedin-have-an-api/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106)

## Links

- [LinkedIn API reference](https://socialtoai.com/platforms/linkedin/?utm_source=github&utm_medium=repo&utm_campaign=linkedin-mcp-20261106)
- [All 18 platforms](https://github.com/socialtoai/socialtoai-mcp)
- Support: support@socialtoai.com

MIT licence for the text and examples in this repository. Published by the SocialToAI team.
