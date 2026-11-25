# HeartResonance 安装包站点

- 线上地址：https://saya.eris.studio
- 托管：GitHub Pages（`main` 分支根目录）
- 自定义域：根目录的 `CNAME` 文件，**不要删除**
- 产品名：**HeartResonance**（安装包名）

## 怎么改

直接编辑 `index.html`（GitHub 网页上点铅笔图标即可），提交到 `main`，大约 1 分钟后自动更新。

### 上线下载链接（只改一处）

找到这一行：

```html
<a class="cta" id="download" href="#" aria-label="下载 HeartResonance">
```

把 `href="#"` 换成真实下载地址即可。页面自带状态检测：

- `href="#"` → 按钮自动变成灰色描边「即将开放」，不可点
- 有真实链接 → 按钮变成实心浅紫「下载」，可点

### 改版本号和大小

按钮第二行，把 `即将开放` 换成实际信息：

```html
<span class="cta-meta">v1.0.0 · 480 MB · Windows</span>
```

### 其他

- 平台说明：改 `<p class="hint">Windows 10 / 11 · 64 位</p>`
- 镜像链接：`index.html` 里有一整段被注释掉的 `<p class="mirror">`，取消注释并填链接即可
- 改配色：`--accent` 是主色，`--bg` 是背景色，`--accent-hi` / `--accent-lo` 是按钮渐变的起止色
- 按钮主文案：`<span class="cta-label">` 里的「下载」两个字
- 反馈邮箱：改 `.feedback` 里的 `mailto:` 和显示的地址

## 下载量计数

页面下方的「累计下载 N」是真实数据，取自 GitHub Releases 所有资产的 `download_count` 之和：

- 数据源：`https://api.github.com/repos/goD0S/saya-site/releases`
- 还没发 Release、或数值为 0 时，**整块隐藏**，不会出现「0 次下载」
- 成功后缓存 10 分钟（localStorage），避开未登录 API 的 60 次/小时限流
- 0 不写入缓存，所以**发版的第一次访问就会看到数字**
- 想在别处托管安装包（如 R2/OSS）时，这个数字不会统计到，需要另建计数服务
- 换仓库：改脚本里的 `const REPO = "goD0S/saya-site"`

## 问题反馈

页面底部有一个 `mailto:` 链接，直接发邮件到 `2932359175@qq.com`，主题已预填「HeartResonance 问题反馈」。

## 注意

- 不要删除根目录的 `CNAME` 文件，否则自定义域名会失效。
- 不要删除 `.nojekyll`，否则 Pages 会跑 Jekyll 处理。
- 不要在这个仓库里放大的安装包（GitHub Pages 单站 1GB、月流量约 100GB，单个文件 100MB 上限）。
  安装包建议放 GitHub Releases，用 `.../releases/latest/download/文件名` 作为页面链接。
- 页面没有任何外部依赖（无 web font、无图片、无 CDN、JS 不到 1KB），改动时请保持这一点。
- 本地预览：直接用浏览器打开 `index.html`。
