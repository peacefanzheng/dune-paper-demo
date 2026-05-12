# Dune Paper Demo 部署指南

> 把这个 demo 部署到 `dune.uspouches.store`，给朋友 / 合伙人 / 潜在合作方看。
>
> **不影响**主域名 `uspouches.store`（ZSY 站继续保留）。
>
> 预计耗时：15-30 分钟（DNS 生效要等几分钟到几小时）。

---

## 📦 这个文件夹里有什么

```
dune-paper-site/
├── index.html       ← 主页面
├── favicon.svg      ← 浏览器 tab 图标
├── vercel.json      ← Vercel 部署配置
├── robots.txt       ← 阻止搜索引擎索引（这是 demo，不希望被搜到）
└── DEPLOY.md        ← 本文件
```

---

## 🚀 部署步骤（最简方式 · 全程在浏览器）

### Step 1：登录 Vercel

打开 https://vercel.com/dashboard ，用你现有账号登录（就是部署 uspouches.store 的那个账号）。

### Step 2：创建新项目

1. 点右上角 **"Add New..."** → **"Project"**
2. 你会看到两个选项：
   - **Import Git Repository**（从 GitHub 导入）
   - **Or, deploy from a template / Browse all templates** 旁边有个小入口可以**直接拖拽文件**

**推荐方式 A：拖拽部署（最简单，30 秒搞定）**

1. 把整个 `dune-paper-site` 文件夹**压缩成 zip**
2. 在 Vercel 项目创建页面，找到 **"Deploy without Git"** 或者用 **Vercel CLI**（见下方）
3. 实际上 Vercel 网页端推荐用 Git，所以下面是**方式 B**

**推荐方式 B：通过 GitHub 部署（更标准）**

1. 在 GitHub 创建一个新仓库，比如叫 `dune-paper-demo`
2. 把 `dune-paper-site/` 里的 4 个文件传上去（直接在 GitHub 网页上 "Add file" → "Upload files" 拖进去即可）
3. 回到 Vercel：**"Add New..."** → **"Project"** → 选择刚才创建的 GitHub 仓库 → **"Import"**
4. 配置页面：
   - **Project Name**: `dune-paper`（随便起，影响默认 URL）
   - **Framework Preset**: 选 **"Other"**（这是纯静态 HTML，不需要框架）
   - **Root Directory**: 保持默认 `./`
   - 其他全部默认
5. 点 **"Deploy"**

等 30 秒左右，Vercel 会给你一个临时 URL，类似：
`https://dune-paper-xyz.vercel.app`

**👉 打开这个 URL 验证：能看到 Dune Paper 首页 = 部署成功。**

---

### Step 3：绑定子域名 `dune.uspouches.store`

1. 在 Vercel 进入你刚部署的 `dune-paper` 项目
2. 点顶部 **Settings** → 左侧 **Domains**
3. 在输入框输入：`dune.uspouches.store`
4. 点 **Add**

Vercel 会立刻告诉你需要在 DNS 服务商那边加一条记录，通常是这样：

```
Type: CNAME
Name: dune
Value: cname.vercel-dns.com.
```

（具体值以 Vercel 页面显示的为准）

---

### Step 4：在 DNS 服务商加 CNAME 记录

你的域名 `uspouches.store` 当初是在哪里买的？

**最可能的几种情况：**

#### A. 在 Vercel 买的 / 域名 nameserver 已经指向 Vercel

很简单：在 Vercel Dashboard → 顶部 **"Domains"**（不是项目里的，是账号顶级的）找到 `uspouches.store` → **Edit DNS records** → 添加：

```
Type: CNAME
Name: dune
Value: cname.vercel-dns.com
```

#### B. 在 GoDaddy / Namecheap / Cloudflare / 阿里云 等地方买的

1. 登录到该平台
2. 找到 `uspouches.store` 的 **DNS 管理** / **DNS Records** / **域名解析**
3. 添加新记录：
   - **Type**: `CNAME`
   - **Name / Host**: `dune`（**不要**写 `dune.uspouches.store`，只写 `dune`）
   - **Value / Target / Points to**: `cname.vercel-dns.com`（注意末尾的点要不要看平台要求）
   - **TTL**: `Auto` 或 `300`
4. 保存

#### C. 用了 Cloudflare 做 DNS

注意：Cloudflare 的"小橙云"（橙色云朵 proxy）会跟 Vercel 冲突，**必须关闭**：
- 添加 CNAME 记录时，点击橙色云朵把它变成灰色（DNS only 模式）

---

### Step 5：等 DNS 生效

通常 5-15 分钟，最长可能 2 小时。

验证方式：
- 在 Vercel Domains 页面，`dune.uspouches.store` 旁边的状态从 ⚠️ 变成 ✅
- 浏览器打开 `https://dune.uspouches.store/` 能看到网站
- Vercel 会自动签发 HTTPS 证书（不用自己搞）

---

## ✅ 验证清单

部署完成后，请逐项确认：

- [ ] `https://dune.uspouches.store/` 能打开，显示 Dune Paper 首页
- [ ] 主域 `https://www.uspouches.store/` **没有变化**，还是原来的 ZSY 站
- [ ] 在手机上打开 `https://dune.uspouches.store/`，移动端正常显示
- [ ] 把链接发到自己的 WhatsApp / 微信，能看到带描述的预览卡片
- [ ] 浏览器 tab 标签页显示 "d." 图标

---

## 🖼️ （可选）加 OG 预览图

发链接到 WhatsApp / 微信 / 邮件时，如果想要带图片的预览（不只是文字描述），需要加一张 1200×630 的预览图：

1. 把 `dune.uspouches.store` 首屏截一张图
2. 用图片编辑工具（Figma / Photoshop / Canva）做成 1200×630 像素
3. 保存为 `og-image.jpg`
4. 上传到 GitHub 仓库的根目录（跟 `index.html` 平级）
5. Vercel 会自动重新部署

HTML 里已经预留了 `og:image` 指向 `/og-image.jpg`，你上传后立刻生效。

测试预览效果：https://www.opengraph.xyz/url/https%3A%2F%2Fdune.uspouches.store

---

## 🛟 常见问题

**Q: 部署后访问报 404？**
A: 检查 `index.html` 是不是在仓库**根目录**（不是放在子文件夹里）。

**Q: DNS 配了但访问不到？**
A: 用 `https://www.whatsmydns.net/` 查 `dune.uspouches.store` 的 CNAME 是否全球生效。等 1 小时再试。

**Q: Cloudflare 报 "Too many redirects" / 521 错误？**
A: 关闭 Cloudflare 的橙云代理（改成灰云 DNS only）。

**Q: HTTPS 证书报错？**
A: 等 5 分钟让 Vercel 自动签发 Let's Encrypt 证书。如果 1 小时后还报错，在 Vercel Domains 页面点 **Refresh** 按钮重试。

**Q: 想改内容（产品名、文案、价格）？**
A: 直接编辑 `index.html`，提交到 GitHub，Vercel 自动重新部署（约 30 秒）。

**Q: 想下掉这个 demo？**
A: 两种方式：
   1. Vercel Dashboard → 项目 → Settings → Advanced → **Delete Project**
   2. 或只删除子域名绑定：项目 → Settings → Domains → `dune.uspouches.store` → Remove

---

## 🔒 安全注意

这个 demo 是**纯静态 HTML**，没有任何数据库、密钥、用户数据。它独立于 ZSY 主站和 Supabase。

但请注意：**ZSY 主站那边的密钥泄露问题还没修**（HANDOVER.md 里的 service role key + GitHub token）。建议先处理那边的安全问题，再发布任何对外链接。

---

## 📞 部署遇到问题

把以下信息发给我，我帮你诊断：
1. 卡在哪一步（Step 1-5）
2. Vercel 上的错误截图
3. DNS 服务商是哪家
4. `https://www.whatsmydns.net/#CNAME/dune.uspouches.store` 的查询结果
