# 👗 我的衣橱 — 手机端衣橱搭配网站

一个专为 iPhone 优化的个人衣橱管理网站，支持上传衣服照片、分类管理、搭配预览和保存。

## 功能

- 📸 **拍照上传**：支持拍照或从相册选择，自动压缩节省空间
- 🏷️ **分类管理**：上衣/下装/连衣裙/外套/鞋子/配饰，支持筛选
- ✨ **搭配组合**：按分类选择衣物，实时预览搭配效果
- 🎲 **随机搭配**：一键生成搭配灵感
- 💾 **收藏搭配**：保存喜欢的搭配方案，随时回顾
- 📱 **移动优化**：适配 iPhone 安全区域，支持添加到主屏幕
- ☁️ **云端同步**：可选连接 Supabase，多设备自动同步
- 📦 **数据导出/导入**：支持 JSON 格式备份恢复

## ☁️ 云端同步设置（让手机和电脑数据互通）

### 1. 创建 Supabase 项目（免费）

1. 打开 https://supabase.com → Sign Up 注册（可用 GitHub 登录）
2. 创建一个新项目，选免费的 Starter 方案
3. 进入项目 Settings → API → 复制 **Project URL** 和 **anon public key**

### 2. 创建数据库表

在 Supabase 项目里打开 **SQL Editor**，粘贴以下 SQL 并执行：

```sql
CREATE TABLE clothes (
  id BIGSERIAL PRIMARY KEY,
  name TEXT NOT NULL DEFAULT '',
  category TEXT NOT NULL,
  image TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE outfits (
  id BIGSERIAL PRIMARY KEY,
  name TEXT NOT NULL DEFAULT '我的搭配',
  item_ids BIGINT[] NOT NULL DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 允许匿名访问（个人使用没问题）
ALTER TABLE clothes ENABLE ROW LEVEL SECURITY;
ALTER TABLE outfits ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon_all" ON clothes FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "anon_all" ON outfits FOR ALL USING (true) WITH CHECK (true);
```

### 3. 连接网站

1. 打开你的衣橱网站 → 点击 ⚙️ 按钮
2. 粘贴 Supabase URL 和 Anon Key
3. 点击「连接云端」
4. 在手机和电脑上都这样配置一遍（填入相同的 URL 和 Key）

之后无论在哪台设备上传衣服，所有设备都会自动同步！

## 如何部署

### GitHub Pages（推荐，免费）

1. 在 GitHub 新建一个仓库（如 `wardrobe`）
2. 将 `index.html` 上传到仓库
3. 进入仓库 Settings → Pages → Source 选 `main` 分支 → Save
4. 等几分钟后，访问 `https://你的用户名.github.io/wardrobe`
5. 在 iPhone Safari 打开 → 点击分享按钮 → 「添加到主屏幕」

### Vercel / Netlify（免费，国内访问更快）

直接拖拽 `index.html` 到 [Netlify Drop](https://app.netlify.com/drop) 即可部署。

## 技术说明

- 纯前端静态网站，可部署到任何静态托管服务
- 本地存储使用 IndexedDB，云端同步使用 Supabase
- 图片上传时自动压缩至 600px 宽，节省存储空间
- 云端模式下数据存储在 Supabase PostgreSQL 数据库中
