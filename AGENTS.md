# AGENTS.md — click-bug-demo

CCPocket 产品介绍落地页。单文件静态站：全部内容在 `index.html`（含一段内联 `<script>`）。

## ⚠️ 硬性要求：这是一个已发布到公网的线上网站

- 本站**已部署到 Cloudflare Pages**，生产地址：**https://click-bug-demo.pages.dev/**
- 它**不是内网 / 本地站**。任何人从外网都能访问，用户是在**线上**发现问题、提 issue 的。
- 因此：**任何代码改动完成后，必须重新部署到公网让线上生效**，绝不能只改本地文件就算完事。修了 bug 却没上线 = 没修。

## 部署命令（改完必须执行）

在项目根目录执行：

```bash
wrangler pages deploy . --project-name=click-bug-demo --branch=main --commit-dirty=true
```

- 认证走环境变量 `CLOUDFLARE_API_TOKEN`（运行环境已配好）。
- **严禁**把 token 写进任何文件或提交到 git；只从环境变量读取。
- 部署成功后，wrangler 会输出线上地址；确认 https://click-bug-demo.pages.dev/ 已是修复后的版本。

## 修 bug 标准流程

1. 结合 issue 描述复现并定位问题。
2. 修改 `index.html`（本站只有这一个页面 + 一段内联脚本）。
3. 读一遍改动，确认逻辑正确。
4. `git add -A && git commit`（Conventional Commits，如 `fix: ...`）。
5. **执行上面的部署命令，把修复推到公网。**
6. 简述：改了什么、为什么、线上是否已生效。

## 项目背景（帮助定位）

- 页面主体是 CCPocket 介绍（hero + 功能网格 + 反馈区）。
- 交互只有一处：底部反馈区的「👍 有帮助」点赞按钮，点击应让赞数 +1。
- 点赞逻辑在页面底部内联 `<script>` 的 `likePage()` 函数里。
