# 龙岗区幼师面试备考资料 — 项目文档

## 🎯 项目概述

本项目是一份交互式 HTML 备考资料，专为龙岗区幼师面试设计，包含：

- **40 道核心面试题**（10 道理论题 + 10 道时政题 + 7 道应急应变题 + 7 道家长沟通题 + 6 道补充观点题），点击题目展开答案
- **绘画备考方案**，含 5 大必练主体物和考场 10 分钟流程
- **3 大结构化答题框架** → 扩展为 **4 大框架 + 答题模板句子 + 速查表**（是·为·怎 / 停·做·说·防 / 听·懂·说·做 / 时·地·人·程·结）
- **每日学习计划**，附带限时绘画计时器
- **学习进度追踪**，标记已掌握题目，自动保存到浏览器

全程无需安装任何 App，在微信里直接打开浏览器就能用。

---

## 📡 部署原理

### 整体架构

```
用户手机/电脑                        GitHub Pages               GitHub API
    │                                    │                          │
    ├── 浏览器打开 ─────────────────►  zh1020102833.github.io     │
    │                                    │                          │
    │                                    │  (静态文件托管)          │
    │                                    │                          │
    │                              ┌─────┴──────┐                  │
    │                              │ index.html  │                  │
    │                              │ (97KB HTML) │                  │
    │                              └─────────────┘                  │
    │                                                              │
    └─────────────── 部署/更新 ────────────────────►  GitHub API ──┘
```

### 为什么选择 GitHub Pages

| 方案 | 免费 | 国内访问 | 无需代理 | 持久稳定 |
|------|:----:|:--------:|:--------:|:--------:|
| **GitHub Pages** ✅ | ✅ | 基本可用 | ✅ (直接打开) | ✅ |
| Gitee Pages | ✅ | ✅ 完美 | ✅ | 需账号安全等级 |
| Localtunnel | ✅ | ❌ 有中间页 | ✅ | ❌ 需电脑开机 |
| Serveo SSH 隧道 | ✅ | ✅ 无中间页 | ✅ | ❌ 需电脑开机 |
| 云服务器(ECS/COS) | ❌ 付费 | ✅ | ✅ | ✅ |

### GitHub Pages 的工作流程

```
1. 在 GitHub 上创建一个公开仓库（ys-kaoshi）
2. 将 index.html 上传到仓库的 main 分支
3. 开启仓库的 Pages 功能（设置 → Pages → 选择 main 分支）
4. GitHub 自动将文件部署到 zh1020102833.github.io/ys-kaoshi/
5. 用户通过该 URL 访问，GitHub 直接返回静态 HTML 文件
6. 所有交互功能（展开/折叠/进度/计时器）在用户浏览器本地运行
```

### 部署步骤（一次性的）

**前提：** 有一个 GitHub 账号，并创建了一个 Personal Access Token（classic）

```
1. 创建仓库
   POST /user/repos → {"name":"ys-kaoshi","private":false}

2. 上传文件
   PUT /repos/{owner}/{repo}/contents/index.html
   → 将 HTML 文件 base64 编码后上传到 main 分支

3. 开启 Pages
   POST /repos/{owner}/{repo}/pages
   → 设置 source.branch = "main"，source.path = "/"

4. 等待几分钟，访问 https://{username}.github.io/ys-kaoshi/
```

---

## 🔄 如何更新内容

当你修改了 HTML 文件（增删题目、修改答案等），只需重新部署即可。

### 方式一：运行 deploy.bat（推荐，1 分钟）

双击 `deploy.bat`，按提示输入 GitHub Token，自动完成更新。

### 方式二：手动操作（无需脚本）

1. 登录 [github.com](https://github.com)
2. 进入仓库 `zh1020102833/ys-kaoshi`
3. 点击 `index.html` → 点右上角"编辑"（铅笔图标）
4. 把新的 HTML 内容粘贴进去 → 点"Commit changes"
5. 等 1-2 分钟，刷新页面即可看到更新

### 方式三：API 命令（适合熟悉命令行的用户）

```bash
# 1. 将 HTML 文件 base64 编码
python -c "import base64; print(base64.b64encode(open('index.html','rb').read()).decode())"

# 2. 上传到 GitHub（替换 YOUR_TOKEN）
curl -X PUT "https://api.github.com/repos/zh1020102833/ys-kaoshi/contents/index.html" \
  -H "Authorization: token YOUR_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  -d "{\"message\":\"update: 备考资料\",\"content\":\"<base64编码内容>\",\"branch\":\"main\"}"

# 3. 几分钟后刷新页面
```

---

## 📁 文件结构

```
ys-kaoshi/
├── index.html          ← 主文件（备考资料，可直接在浏览器打开）
├── deploy.bat          ← 一键部署脚本（适用于更新后重新部署）
├── README.md           ← 本说明文档
└── images/             ← （预留）图片资源目录
```

---

## 📝 使用说明

### 在手机上使用

1. 打开微信 → 把链接发给文件传输助手 → 点击打开
2. 或复制链接到手机浏览器打开
3. **建议添加标签/收藏**，方便下次快速找到

### 备考建议

| 时间段 | 内容 | 时长 |
|--------|------|:----:|
| 🌅 早晨 | 读时政关键词 | 10 分钟 |
| ☀️ 上午 | 练 1 道理论题 + 1 道时政题（录音回听） | 20 分钟 |
| 🌤 下午 | 画 1 幅简笔画（限时 10 分钟） | 10 分钟 |
| 🌙 睡前 | 回顾当天题目，看关键词能否想起来 | 5 分钟 |
| 📆 周末 | 模拟完整面试（抽 3 题，8 分钟答完） | 30 分钟 |

### 学习技巧

- **先看题，自己说一遍，再点开答案对照**
- 掌握了的题目点圆点标记变绿，复习时优先看未掌握的
- 答题时用"是什么→为什么→怎么做"框架组织语言
- 结合自己的工作经验举例（叠衣服、洗手、摆椅子等）
- 每天开口练习，不要只看不练

---

## ⚙️ 技术细节

### 交互功能实现

所有交互功能都是用纯前端技术实现的，无需后端服务器：

| 功能 | 实现方式 |
|------|---------|
| 点击展开答案 | CSS + JavaScript 切换 class |
| 进度圆点 | localStorage 保存状态 |
| 进度统计 | JavaScript 计算并更新 DOM |
| 限时计时器 | setInterval 倒计时 |
| 分类筛选 | 显示/隐藏对应 section |
| 展开/收起全部 | 批量操作 card 的 open class |
| 数据持久化 | localStorage（关闭页面再打开不丢失） |

### 浏览器兼容性

支持所有现代浏览器：Chrome、Safari、Edge、微信内置浏览器、QQ 浏览器等。

---

## 🔑 安全说明

- **GitHub Token** 是访问你仓库的"钥匙"，使用后请及时删除
- 在 GitHub 设置 → Developer settings → Personal access tokens 中可以管理和撤销 token
- 本项目所有文件均在浏览器本地运行，不上传任何用户数据
- 学习进度存储在手机/电脑的浏览器本地，不会泄露

---

*最后更新：2026 年 5 月 11 日*
*祝面试顺利，成功上岸！🍀*
