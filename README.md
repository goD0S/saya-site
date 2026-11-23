# Saya 的小程序站点

- 线上地址：https://saya.eris.studio
- 托管：GitHub Pages（`main` 分支根目录）
- 自定义域：根目录的 `CNAME` 文件，**不要删除**

## 怎么改

直接编辑 `index.html`（GitHub 网页上点铅笔图标即可），提交到 `main`，大约 1 分钟后自动更新。

- 改标题：找到 `<h1>小程序正在制作中</h1>`
- 换文案：找到 `<p>做好之后，下载入口会出现在这里。</p>`
- 加下载按钮：把 `<span class="soon">...</span>` 整段替换成

  ```html
  <a class="soon" href="你的下载链接"><span class="dot"></span>下载小程序</a>
  ```

- 改配色：`--accent` 是主色，`--bg` 是背景色

## 注意

- 不要删除根目录的 `CNAME` 文件，否则自定义域名会失效。
- 不要在这个仓库里放大的安装包（GitHub Pages 单站 1GB、月流量约 100GB）。
  安装包建议放 GitHub Releases，页面里链接过去即可。
- 本地预览：直接用浏览器打开 `index.html`。
