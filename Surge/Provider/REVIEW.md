# Provider 审核清单

这个文件记录没有自动合并进当前主规则集的内容，以及原因。

## 未自动合并

### Auto

`Auto/` 看起来是旧版 Surge 配置拆出来的片段，不是当前主 Provider 来源。
下面这些文件需要之后人工判断：

- `Auto/DIRECT.conf`：旧的直连/国内规则，里面混着国内服务、局域网、SMTP、
  BT 下载和行为规则，不能整文件并入 `China.list`。
- `Auto/PROXY.conf`：旧的代理大杂烩，可能把已经删掉或重新分类的服务又加回来。
- `Auto/REJECT.conf`：广告拦截规则，不动。Reject 行为必须保持独立。
- `Auto/Header Rewrite.conf`、`Auto/URL Rewrite.conf`、`Auto/Script.conf`、
  `Auto/Hostname.conf`：重写、脚本、MITM hostname，不是普通路由 Provider。
- `Auto/HOST.conf`：空文件。

`Auto/Apple.conf` 里明确属于 Apple 直连的内容已经合并进
`Apple Microsoft.list`。

### 2025/Home_IP_Only.list

这个文件的用途已经确认：和 `AI.list`、`Disney Hulu Netflix.list` 一样，未来
都准备走家里服务器路线。

已处理：

- AI / 开发工具相关内容已合并进 `AI.list`。
- 美区流媒体相关内容已合并进 `Disney Hulu Netflix.list`。
- 已存在的规则没有重复追加。

仍然需要人工确认的冲突项：

- Google AI / Gemini 相关域名：按之前约定，Google 服务保持在 `Proxy.list`，
  不并入 `AI.list`。
- YouTube TV 相关域名：属于 Google / YouTube 服务，保持在 `Proxy.list`，
  不并入 `Disney Hulu Netflix.list`。
- Apple TV+ 相关域名：属于 Apple 服务，保持在 `Apple Microsoft.list`，
  不并入 `Disney Hulu Netflix.list`。
- Microsoft Copilot 周边的广义 Microsoft 域名，例如 `bing.com`、`msn.com`、
  `edge.microsoft.com`、`microsoftonline.com`、`assets.msn.com`：这些可能和
  `Apple Microsoft.list` 的直连目标冲突，所以没有自动并入 `AI.list`。
- `api.github.com`：GitHub API 很宽，只有 GitHub Copilot 明确并入了
  `AI.list`，没有把整个 GitHub API 改成家里路线。
- Firebase 相关域名：属于 Google 生态，暂时不并入 `AI.list`。

### 2025/Global.list

这是一个很大的全球代理大杂烩，和当前 `Proxy.list`、`Apple Microsoft.list`、
`China.list` 都有重叠。没有整文件合并，因为会破坏现在按用途和线路拆分的
结构。

### 2025/block_udp443.list

内容是：

```text
AND,((PROTOCOL,UDP),(DEST-PORT,443)),REJECT-NO-DROP
```

这是阻止 QUIC / UDP 443 的行为规则，不是普通域名 Provider。之后整理主配置
或模块时再决定是否启用。
