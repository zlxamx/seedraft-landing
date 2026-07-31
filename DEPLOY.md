# 文芽 Seedraft 落地页部署指南

## 方案 A：Vercel 部署（推荐）

### 1. 推送到 GitHub

```bash
# 在 GitHub 创建新仓库：seedraft-landing（可以是私有的）
cd /Users/zhangluxi/.proma/agent-workspaces/seedraft/workspace-files
git remote add origin https://github.com/你的用户名/seedraft-landing.git
git add vercel.json
git commit -m "Add Vercel config"
git push -u origin main
```

### 2. 导入到 Vercel

1. 访问 https://vercel.com
2. 点击「New Project」
3. 导入你的 GitHub 仓库 `seedraft-landing`
4. 项目设置：
   - Framework Preset: **Other**
   - Root Directory: `./`
   - Build Command: (留空)
   - Output Directory: `./`
5. 点击「Deploy」

### 3. 配置自定义域名

部署完成后：

1. 进入项目设置 → Domains
2. 添加域名：`seedraft.xiluluke.com`
3. Vercel 会提供 DNS 记录

### 4. 在域名商配置 DNS

去你的域名管理面板（xiluluke.com 的 DNS 服务商），添加：

```
类型: CNAME
名称: seedraft
值: cname.vercel-dns.com
TTL: 自动
```

**或者用 A 记录**（如果 CNAME 不支持）：

```
类型: A
名称: seedraft
值: 76.76.21.21
TTL: 自动
```

等待 DNS 生效（5-30 分钟），访问 https://seedraft.xiluluke.com

---

## 方案 B：Cloudflare Pages

### 1. 推送到 GitHub（同上）

### 2. 部署到 Cloudflare Pages

1. 访问 https://dash.cloudflare.com
2. 进入「Workers & Pages」
3. 点击「Create application」→「Pages」→「Connect to Git」
4. 选择你的 GitHub 仓库
5. 构建设置：
   - Framework preset: **None**
   - Build command: (留空)
   - Build output directory: `/`
6. 点击「Save and Deploy」

### 3. 配置自定义域名

1. 部署完成后，进入项目 → Custom domains
2. 添加：`seedraft.xiluluke.com`
3. 如果 xiluluke.com 已经在 Cloudflare，会自动配置 DNS
4. 如果不在，需要手动添加 CNAME 记录（Cloudflare 会提供）

---

## 方案 C：直接上传到服务器（传统方式）

如果你有自己的服务器：

### 1. 上传文件

```bash
# 使用 scp
scp index.html 你的用户名@你的服务器IP:/var/www/seedraft/

# 或使用 FTP 客户端上传 index.html
```

### 2. 配置 Nginx

```nginx
server {
    listen 80;
    server_name seedraft.xiluluke.com;
    root /var/www/seedraft;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # SSL 配置（推荐使用 Let's Encrypt）
    # listen 443 ssl;
    # ssl_certificate /path/to/cert.pem;
    # ssl_certificate_key /path/to/key.pem;
}
```

### 3. 配置 DNS

添加 A 记录指向你的服务器 IP。

---

## 推荐选择

| 方案 | 优点 | 缺点 |
|------|------|------|
| **Vercel** | 免费、零配置、自动 HTTPS、全球 CDN、CI/CD | 需要 GitHub |
| **Cloudflare Pages** | 免费、快速、与 Cloudflare DNS 集成好 | 需要 GitHub |
| **自己的服务器** | 完全控制 | 需要维护、配置 SSL |

**我推荐 Vercel**，原因：
- 零配置，推送即部署
- 自动 HTTPS
- 全球 CDN 加速
- 每次 git push 自动更新
- 免费额度足够个人项目

---

## 更新网站

以后修改内容只需：

```bash
cd /Users/zhangluxi/.proma/agent-workspaces/seedraft/workspace-files
# 修改 index.html
git add .
git commit -m "更新内容"
git push
```

Vercel/Cloudflare 会自动重新部署。

---

## 需要帮助？

- Vercel 文档: https://vercel.com/docs
- Cloudflare Pages 文档: https://developers.cloudflare.com/pages
- 微信: zlxamx
