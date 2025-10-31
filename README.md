# 我的 Hexo 博客

基于 Hexo + Stun 主题的个人技术博客

## 本地开发

```bash
# 安装依赖
npm install

# 启动本地服务器
npm run server

# 生成静态文件
npm run build

# 清除缓存
npm run clean
```

## 部署到 Vercel

### 方法一: 通过 Vercel 网站部署(推荐)

1. 将代码推送到 GitHub
2. 访问 [Vercel](https://vercel.com)
3. 使用 GitHub 账号登录
4. 点击 "Import Project"
5. 选择你的 GitHub 仓库
6. Vercel 会自动识别 Hexo 项目
7. 点击 "Deploy" 即可

### 方法二: 通过 Vercel CLI 部署

```bash
# 安装 Vercel CLI
npm install -g vercel

# 登录
vercel login

# 部署
vercel
```

## 目录结构

```
.
├── _config.yml          # 站点配置文件
├── _config.stun.yml     # Stun 主题配置文件
├── package.json         # 依赖包配置
├── scaffolds/          # 文章模板
├── source/             # 源文件
│   ├── _posts/        # 博客文章
│   ├── about/         # 关于页面
│   ├── tags/          # 标签页面
│   └── categories/    # 分类页面
├── themes/            # 主题文件
│   └── stun/         # Stun 主题
└── vercel.json        # Vercel 配置文件
```

## 写作

```bash
# 创建新文章
hexo new "文章标题"

# 创建草稿
hexo new draft "草稿标题"

# 发布草稿
hexo publish draft "草稿标题"
```

## 主题

当前使用 [Stun](https://github.com/liuyib/hexo-theme-stun) 主题

主题配置文件: `_config.stun.yml`

## License

MIT

