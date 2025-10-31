# GitHub Pages 部署指南

## 🚀 自动部署(推荐)

### 方法一: 使用 GitHub Actions 自动部署

这是最稳定的方式,不受本地网络限制!

#### 1. 创建 GitHub Actions 配置文件

在博客根目录创建 `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: gh-pages
```

#### 2. 推送配置文件到 GitHub

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions workflow"
git push
```

#### 3. 在 GitHub 仓库设置中启用 GitHub Pages

1. 打开仓库: https://github.com/xuzhengqiang/my-hexo-blog
2. 点击 **Settings** → **Pages**
3. Source 选择 **gh-pages** 分支
4. 点击 **Save**

#### 4. 等待部署完成

- GitHub Actions 会自动构建和部署
- 在 Actions 标签页可以查看进度
- 完成后访问: https://xuzhengqiang.github.io/my-hexo-blog

---

## 📝 方法二: 手动部署(网络问题时使用)

如果 `hexo deploy` 连接 GitHub 失败,可以手动部署:

### 步骤:

#### 1. 生成静态文件

```bash
npx hexo clean
npx hexo generate
```

#### 2. 进入 public 目录

```bash
cd public
```

#### 3. 初始化 Git 并推送

```bash
# 初始化 Git
git init

# 添加所有文件
git add .

# 提交
git commit -m "Deploy blog"

# 添加远程仓库
git remote add origin https://github.com/xuzhengqiang/my-hexo-blog.git

# 推送到 gh-pages 分支
git push -f origin master:gh-pages
```

#### 4. 在 GitHub 设置 Pages

1. 打开: https://github.com/xuzhengqiang/my-hexo-blog/settings/pages
2. Source 选择 **gh-pages** 分支
3. 点击 **Save**

#### 5. 访问博客

等待 1-2 分钟后访问: https://xuzhengqiang.github.io/my-hexo-blog

---

## 🔧 解决网络连接问题

### 问题: `fatal: unable to access 'https://github.com'`

这是由于网络连接 GitHub 超时导致的。

### 解决方案:

#### 方案 1: 使用 SSH 连接(推荐)

1. **生成 SSH 密钥**

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 一路回车,使用默认设置
```

2. **复制公钥**

```bash
# Windows PowerShell
Get-Content $HOME/.ssh/id_rsa.pub
```

3. **添加到 GitHub**

- 访问: https://github.com/settings/keys
- 点击 **New SSH key**
- 粘贴公钥内容
- 点击 **Add SSH key**

4. **修改 `_config.yml` 使用 SSH**

```yaml
deploy:
  type: git
  repo: git@github.com:xuzhengqiang/my-hexo-blog.git
  branch: gh-pages
```

5. **重新部署**

```bash
npx hexo clean
npx hexo deploy
```

#### 方案 2: 配置 Git 代理(如果你有代理)

```bash
# 设置 HTTP 代理
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

#### 方案 3: 修改 hosts 文件

在 `C:\Windows\System32\drivers\etc\hosts` 添加:

```
140.82.113.3 github.com
199.232.69.194 github.global.ssl.fastly.net
```

#### 方案 4: 使用 GitHub Actions(最推荐!)

使用 GitHub Actions 自动部署,不受本地网络限制!

---

## ✅ 推荐部署流程

### 最佳方案: GitHub Actions 自动部署

1. **创建 `.github/workflows/deploy.yml`**(见上面)
2. **推送代码到 GitHub**
   ```bash
   git add .
   git commit -m "Add deploy workflow"
   git push
   ```
3. **在 GitHub 设置 Pages**
4. **完成!** 以后每次 push 都会自动部署

### 日常更新流程

```bash
# 1. 写文章
hexo new "文章标题"

# 2. 本地预览
hexo server

# 3. 提交并推送
git add .
git commit -m "新增文章"
git push

# 4. GitHub Actions 自动部署!
```

---

## 📊 验证部署

### 检查 GitHub Actions

1. 访问: https://github.com/xuzhengqiang/my-hexo-blog/actions
2. 查看最新的 workflow 运行状态
3. 如果失败,点击查看详细日志

### 检查 GitHub Pages

1. 访问: https://github.com/xuzhengqiang/my-hexo-blog/settings/pages
2. 确认 Source 设置为 **gh-pages** 分支
3. 查看部署状态

### 访问博客

访问: https://xuzhengqiang.github.io/my-hexo-blog

如果显示 404:
- 等待 2-5 分钟(GitHub Pages 需要时间生效)
- 检查 gh-pages 分支是否存在
- 检查文件是否正确推送

---

## 🎯 总结

| 方法 | 优点 | 缺点 |
|------|------|------|
| **GitHub Actions** | ✅ 自动部署<br>✅ 无网络问题<br>✅ 最稳定 | 需要配置 |
| **hexo deploy** | ✅ 一键部署 | ❌ 可能网络超时 |
| **手动部署** | ✅ 完全掌控 | ❌ 步骤繁琐 |

**推荐: 使用 GitHub Actions 自动部署!** 🚀

