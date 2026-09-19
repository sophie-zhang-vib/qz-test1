# 🚀 个人网站部署到 GitHub Pages 新手教程（Windows 版）

## 整体思路（先理解，再动手）

```
你电脑里的代码  →① git 打包存档→  ② 上传到 GitHub 仓库  →③ 开启 Pages 托管→  全世界可访问
                  (本地 commit)       (push)                  (网页上点一下)
```

全程**免费**，不需要买服务器。GitHub Pages 是 GitHub 官方提供的静态网站托管服务，本网站是纯 HTML/CSS/JS，正好适用。

> 💡 本项目**不需要安装 Node.js、npm 或任何依赖**。想在本地预览网站，直接双击 `index.html` 用浏览器打开即可。

---

## 第 0 步：准备工作（只需做一次）

### 0.1 注册 GitHub 账号
打开 https://github.com 注册一个账号（免费），注册后记得去邮箱点验证链接。

### 0.2 安装 Git
1. 打开 https://git-scm.com/download/win ，会自动下载 **Git for Windows**
2. 双击安装，**一路点 "Next" 用默认选项即可**（默认会勾选 Git Credential Manager，登录很方便）
3. 安装完成后，打开 **PowerShell**（开始菜单搜 "PowerShell"），输入：

```powershell
git --version
```

看到类似 `git version 2.46.0` 就说明安装成功。

### 0.3 告诉 Git 你是谁（提交代码要署名）

```powershell
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub邮箱"
```

> 只用当前这台电脑的话用 `--global` 最省事。以后提交记录里就会显示这个名字。

---

## 第 1 步：在 GitHub 上创建一个空仓库

1. 登录 GitHub，右上角点 **➕ → New repository**
2. 填写：
   - **Repository name**：`personal-website`（建议全小写，用短横线）
   - **Public**（必须选公开，免费版只有公开仓库能用 Pages）
   - **不要勾选** "Add a README file"、.gitignore、license（保持空仓库，避免首次推送冲突）
3. 点 **Create repository**
4. 创建后页面会显示一个仓库地址，复制它，形如：

```
https://github.com/你的用户名/personal-website.git
```

---

## 第 2 步：检查项目里的"安全清单"

本项目是纯静态网站，没有依赖文件夹、也没有密钥文件，需要上传的只有：

- 四个页面：`index.html`、`resume.html`、`projects.html`、`contact.html`
- `assets/` 文件夹（`style.css` 和 `main.js`）
- 辅助文件：`.gitignore`、`supabase-todos-setup.sql`、`DEPLOY.md`

项目根目录的 `.gitignore` 会自动排除系统垃圾文件和编辑器配置目录。

> ⚠️ 通用安全原则：以后如果网站用到了密钥文件（如 `.env`），**绝不能**推送到公开仓库，要先把它写进 `.gitignore`。

---

## 第 3 步：把本地代码变成 Git 仓库

在 PowerShell 里进入项目文件夹：

```powershell
cd C:\Users\Lenovo\Desktop\personal-website
```

然后依次执行（每行一条，等它执行完再下一条）：

```powershell
# 1. 初始化本地仓库（只需一次）
git init

# 2. 选中所有要上传的文件（.gitignore 里的会自动排除）
git add .

# 3. 存档一次，引号里是这次提交的说明
git commit -m "Initial commit: my personal website"

# 4. 把主分支命名为 main
git branch -M main
```

> 如果第 3 步报 `Author identity unknown`，说明第 0.3 步的 user.name/email 没配，回去补上再重新 commit。

---

## 第 4 步：连接 GitHub 仓库并上传

```powershell
# 把下面的地址换成第 1 步复制的你自己的地址
git remote add origin https://github.com/你的用户名/personal-website.git

# 首次推送
git push -u origin main
```

**关于登录验证（重点）：**
- 如果安装的是 Git for Windows，这时**通常会自动弹出一个浏览器窗口**让你登录 GitHub 授权，点同意即可，以后不用再登录。
- 如果在命令行里要求输入用户名密码：用户名填 GitHub 用户名，**密码处不能填 GitHub 登录密码**（GitHub 早已停用），要填 **Personal Access Token (PAT)**：
  1. GitHub 网页 → 头像 → **Settings** → 最下面 **Developer settings** → **Personal access tokens** → **Tokens (classic)**
  2. **Generate new token (classic)** → 勾选 `repo` → 生成 → **复制保存**（只显示一次）
  3. 把这个 token 粘贴到密码处

看到类似 `* [new branch] main -> main` 就表示上传成功了。刷新 GitHub 仓库页面，能看到你的代码。

> 🔐 国内网络偶尔连接 GitHub 超时，重试几次即可；如果公司/校园网络受限，换手机热点试一下。

---

## 第 5 步：开启 GitHub Pages（让网站上线）

1. 打开你的仓库页面，点顶部 **Settings**
2. 左侧菜单找到 **Pages**
3. 在 **Build and deployment** 下：
   - **Source**：选 `Deploy from a branch`
   - **Branch**：选 `main`
   - **文件夹**：选 `/ (root)`（⚠️ 本项目的网站文件就在仓库根目录）
4. 点 **Save**

等待 **1～2 分钟**（首次可能要 3-5 分钟），刷新页面顶部会出现：

```
Your site is live at https://你的用户名.github.io/personal-website/
```

点这个链接，网站就上线了 🎉

---

## 第 6 步：以后更新网站内容

每次改完代码，只需三行：

```powershell
git add .
git commit -m "写清楚这次改了什么"
git push
```

推送后 Pages 会**自动重新部署**，1-2 分钟后刷新网页即可（看到旧内容是浏览器缓存，按 `Ctrl + F5` 强制刷新）。

---

## 🩺 常见问题

| 问题 | 解决方法 |
|---|---|
| `Authentication failed` | 密码处要填 PAT，不是登录密码（见第 4 步） |
| 推送报连接超时/443 错误 | 网络问题，多重试几次或换网络；确认浏览器能正常打开 github.com |
| 网站打开是 404 | 刚开启要等几分钟；确认 Branch 选了 `main`、文件夹选了 `/ (root)` |
| 页面能开但没样式/排版乱 | HTML 里必须用相对路径（`assets/style.css` 而非 `/assets/style.css`）——本项目已经处理好了 ✅ |
| 修改推送后网页没变 | 等 1-2 分钟，然后 `Ctrl + F5` 强制刷新 |
| 不小心把密钥传上去了 | 立即去对应平台**作废/轮换密钥**；仅删除文件再提交不能抹掉历史记录 |

---

## ✅ 部署完成检查清单

- [ ] 四个页面都能打开（首页 / Resume / Projects / Contact）
- [ ] 页面之间导航链接正常
- [ ] 样式和字体正常显示
- [ ] Projects 页的 To-Do List 能添加、勾选、删除任务（数据走 Supabase，不依赖服务器）
- [ ] 手机上打开布局正常（可以把网址发给自己手机测试）

---

## 一句话总结

```
安装 Git → 创建仓库 → git init/add/commit → git push → Settings → Pages → 选 main + / (root) → 访问网址
```

部署完成后，把网站地址分享给朋友和老师吧！🚀
