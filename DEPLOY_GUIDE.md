# Hexo 博客部署到 Vercel 完整指南

## 📋 前提条件

- ✅ 已安装 Git
- ✅ 拥有 GitHub 账号
- ✅ 代码已提交到本地 Git 仓库

## 🚀 部署步骤

### 第一步: 创建 GitHub 仓库

1. **访问 GitHub**
   - 打开 https://github.com
   - 登录你的账号

2. **创建新仓库**
   - 点击右上角 `+` 号
   - 选择 `New repository`

3. **填写仓库信息**
   - Repository name: `my-hexo-blog` (或任意名称)
   - Description: `My personal blog built with Hexo`
   - 选择 `Public` (公开)
   - **不要**勾选 "Add a README file"
   - 点击 `Create repository`

4. **复制仓库地址**
   - 复制仓库的 HTTPS 地址
   - 格式: `https://github.com/你的用户名/my-hexo-blog.git`

### 第二步: 推送代码到 GitHub

在博客目录下打开终端(PowerShell 或 CMD),执行以下命令:

```bash
# 1. 添加远程仓库(替换成你的仓库地址)
git remote add origin https://github.com/你的用户名/my-hexo-blog.git

# 2. 重命名分支为 main
git branch -M main

# 3. 推送代码
git push -u origin main
```

**如果遇到身份验证问题:**

- 使用 Personal Access Token (推荐)
  1. GitHub → Settings → Developer settings → Personal access tokens → Generate new token
  2. 勾选 `repo` 权限
  3. 生成 token 并复制
  4. 推送时使用 token 作为密码

- 或使用 GitHub Desktop (更简单)
  1. 下载并安装 GitHub Desktop
  2. 登录 GitHub 账号
  3. 添加本地仓库并推送

### 第三步: 部署到 Vercel

#### 方法一: 通过 Vercel 网站(推荐,最简单)

1. **访问 Vercel**
   - 打开 https://vercel.com
   - 点击 `Sign Up` 或 `Log In`

2. **使用 GitHub 登录**
   - 选择 `Continue with GitHub`
   - 授权 Vercel 访问你的 GitHub 账号

3. **导入项目**
   - 点击 `Add New...` → `Project`
   - 找到你的 `my-hexo-blog` 仓库
   - 点击 `Import`

4. **配置项目(通常自动识别,无需修改)**
   - Framework Preset: 选择 `Other` (Vercel 会自动检测)
   - Build Command: `npm run build` (自动填充)
   - Output Directory: `public` (自动填充)
   - Install Command: `npm install` (自动填充)

5. **部署**
   - 点击 `Deploy` 按钮
   - 等待 2-3 分钟,Vercel 会自动构建和部署

6. **获取访问链接**
   - 部署完成后,会显示你的博客地址
   - 格式: `https://你的项目名.vercel.app`
   - 点击链接访问你的博客! 🎉

#### 方法二: 通过 Vercel CLI

```bash
# 1. 安装 Vercel CLI
npm install -g vercel

# 2. 登录 Vercel
vercel login

# 3. 部署项目
vercel

# 4. 首次部署会询问:
#    - Set up and deploy? Y
#    - Which scope? 选择你的账号
#    - Link to existing project? N
#    - Project name? my-hexo-blog
#    - In which directory? ./
#    - Want to modify settings? N

# 5. 部署成功后会显示访问链接
```

### 第四步: 自动部署(可选)

Vercel 已自动配置好自动部署! 

每次你推送代码到 GitHub:

```bash
git add .
git commit -m "更新博客内容"
git push
```

Vercel 会自动检测并重新部署博客! 🚀

## 🎯 绑定自定义域名(可选)

如果你有自己的域名:

1. **在 Vercel 中添加域名**
   - 进入项目 → Settings → Domains
   - 输入你的域名(如 `blog.example.com`)
   - 点击 `Add`

2. **配置 DNS**
   - 在域名服务商处添加 DNS 记录
   - 类型: `CNAME`
   - 名称: `blog` (或 `@` 用于根域名)
   - 值: `cname.vercel-dns.com`

3. **等待生效**
   - DNS 生效通常需要 10 分钟 - 24 小时
   - Vercel 会自动配置 HTTPS 证书

## 📝 日常使用

### 写新文章

```bash
# 1. 创建新文章
hexo new "文章标题"

# 2. 编辑文章
# 在 source/_posts/ 找到新创建的 .md 文件并编辑

# 3. 本地预览
hexo clean && hexo server

# 4. 提交并推送
git add .
git commit -m "新增文章: 文章标题"
git push

# 5. Vercel 自动部署(2-3 分钟)
```

### 更新主题配置

```bash
# 1. 修改 _config.stun.yml

# 2. 本地测试
hexo clean && hexo server

# 3. 推送更新
git add .
git commit -m "更新主题配置"
git push
```

## 🔧 常见问题

### 1. Vercel 构建失败

**检查构建日志:**
- Vercel 项目页面 → Deployments → 点击失败的部署 → 查看 Build Logs

**常见原因:**
- Node.js 版本不兼容
- 依赖包安装失败
- 主题文件缺失

**解决方案:**
```bash
# 本地测试构建
npm run build

# 如果本地能成功,则是 Vercel 配置问题
# 在 Vercel 项目设置中指定 Node.js 版本
# Settings → General → Node.js Version → 选择 18.x
```

### 2. 推送到 GitHub 失败

**身份验证错误:**
```bash
# 使用 Personal Access Token
# GitHub → Settings → Developer settings → Personal access tokens
# 创建 token 并使用它作为密码
```

**推送被拒绝:**
```bash
# 先拉取远程更新
git pull origin main --allow-unrelated-histories

# 然后再推送
git push -u origin main
```

### 3. 访问 Vercel 链接显示 404

**原因:** 
- 构建成功但输出目录配置错误

**解决方案:**
- 检查 `vercel.json` 中 `distDir` 是否为 `public`
- 检查本地 `npm run build` 是否生成 `public` 目录

### 4. 主题样式不显示

**原因:**
- CSS/JS 文件路径错误

**解决方案:**
```yaml
# 在 _config.yml 中设置正确的 URL
url: https://你的项目名.vercel.app
root: /
```

## 📈 性能优化

### 启用 CDN 加速

Vercel 自动提供全球 CDN,无需额外配置! ✅

### 图片优化

```bash
# 使用图床服务
# - 七牛云
# - 阿里云 OSS
# - GitHub + jsDelivr CDN
```

## 🎉 完成!

恭喜! 你的博客已经成功部署到 Vercel!

- ✅ 免费托管
- ✅ 自动部署
- ✅ 全球 CDN
- ✅ HTTPS 支持
- ✅ 超快访问速度

现在可以:
1. 分享你的博客链接给朋友
2. 开始写作你的第一篇技术文章
3. 在简历和社交媒体上展示你的博客

---

**需要帮助?**
- Vercel 文档: https://vercel.com/docs
- Hexo 文档: https://hexo.io/zh-cn/docs/
- Stun 主题文档: https://github.com/liuyib/hexo-theme-stun

**祝你写作愉快! 🚀**

