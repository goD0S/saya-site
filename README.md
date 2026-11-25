# HeartResonance 安装包站点

- 线上地址：https://saya.eris.studio
- 托管：GitHub Pages（`main` 分支根目录）
- 自定义域：根目录的 `CNAME` 文件，**不要删除**
- 产品名：**HeartResonance**（安装包名）
- 安装包托管：`dl.eris.studio`（Cloudflare R2 + 自定义域）

## 发新版本要改哪里

`index.html` 里只有两个按钮，各改两处：

| 按钮 | 要改的字段 |
|---|---|
| 完整版 | `href`、`.cta-meta` 里的版本号和大小 |
| 便携版 | `href`、`.cta-meta` 里的版本号和大小 |

```html
<a class="cta" id="download"
   href="https://dl.eris.studio/HeartResonance_v1.0.0.zip"
   data-key="full">
  <span class="cta-label"><span class="dot"></span>下载</span>
  <span class="cta-meta">完整版 · v1.0.0 · 822 MB</span>
</a>
```

- `href` 改回 `"#"` → 该按钮自动降级为「即将开放」，不可点
- `data-key` 决定计数记到哪个桶，不要随便改，否则历史数据会断层

改完提交到 `main`，大约 1 分钟后自动更新。

### 改简介文案

`index.html` 里的 `<p class="about">…</p>` 整段，改完即生效。
左侧那道竖线是 CSS 的 `border-left`，不要在文案里手写。

## 下载量计数

页面下方的「累计下载 N」是**两个按钮的点击总和**，需要自建一个计数服务（免费计数 API 基本都停服了）。

### 接口约定

- `GET /count` → `{ "full": 902, "portable": 382, "total": 1284 }`
- `GET /hit?key=full` 或 `?key=portable` → 计数 +1，返回同上结构
- 响应需带 `Access-Control-Allow-Origin: https://saya.eris.studio`

### 启用方式

在 `index.html` 脚本顶部填上地址即可，留空则整块隐藏：

```js
const COUNTER_API = "";   // 改成 "https://count.eris.studio"
```

### 参考实现（Cloudflare Worker + KV）

```js
export default {
  async fetch(req, env) {
    const url = new URL(req.url);
    const cors = {
      "Access-Control-Allow-Origin": "https://saya.eris.studio",
      "Cache-Control": "no-store",
    };
    const key = url.searchParams.get("key");
    if (url.pathname === "/hit" && (key === "full" || key === "portable")) {
      const cur = parseInt((await env.KV.get(key)) || "0", 10);
      await env.KV.put(key, String(cur + 1));
    }
    const full = parseInt((await env.KV.get("full")) || "0", 10);
    const portable = parseInt((await env.KV.get("portable")) || "0", 10);
    return new Response(JSON.stringify({ full, portable, total: full + portable }), {
      headers: { ...cors, "Content-Type": "application/json" },
    });
  },
};
```

部署：Workers & Pages → 创建 Worker → 粘贴 → 绑定一个名为 `KV` 的 KV 命名空间 → 发布。
拿到 `https://xxx.workers.dev` 后填进 `COUNTER_API`。国内访问慢的话，可以在 Cloudflare 里给 Worker 绑一条自定义路由。

### 注意

- 统计的是**点击**，不是服务端确认完成的下载。用户点取消也算一次。
- 计数请求用 `keepalive` 发送，下载跳转后仍能发出。
- 服务不可用时整块隐藏，不影响下载按钮。

## 问题反馈

页面底部有一个邮箱链接，直接发邮件到 `2932359175@qq.com`，主题已预填「HeartResonance 问题反馈」。

> 邮箱地址在 HTML 里拆成了 `data-user` / `data-domain` 两段，由脚本拼装。
> 这是为了绕开 Cloudflare 的「邮箱地址混淆」——否则 `mailto:` 会被替换成
> `/cdn-cgi/l/email-protection#...`，页面上只显示 `[email protected]`。

## 注意

- 不要删除根目录的 `CNAME` 文件，否则自定义域名会失效。
- 不要删除 `.nojekyll`，否则 Pages 会跑 Jekyll 处理。
- 不要把安装包放进这个仓库（Pages 单站 1GB、单文件 100MB 上限）。安装包一律放 `dl.eris.studio`。
- 页面本身没有任何外部依赖（无 web font、无图片、无 CDN），只有计数服务是外部请求且可关闭。
- 本地预览：直接用浏览器打开 `index.html`。
