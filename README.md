# 地球 Online · PWA 入口壳页

把工作台变成**手机和电脑上的独立图标应用**：安装后点图标直接进，图标永远是我们自己的地球图标（不会被 WorkBuddy 站点图标刷新替换）。

```
index.html    入口页（PWA 安装引导 / 微信引导 / 自动跳转）
manifest.json PWA 清单（名称、图标、standalone 独立窗口）
sw.js         Service Worker（有它浏览器才给「安装应用」选项，并支持离线壳页）
icons/        192 / 512 / maskable-512（安卓自适应）/ apple-touch-180 / favicon-64
```

## 文件在你电脑的哪里

```
C:\Users\perla\Documents\陈佩玲\个人工作台\build\gh-pages\
├─ index.html                     7.6 KB   入口页
├─ manifest.json                  0.9 KB   PWA 清单
├─ sw.js                          1.7 KB   Service Worker
├─ README.md                      3.3 KB   本文档（不影响运行，传不传都行）
└─ icons\                        共 816 KB
   ├─ icon-192.png                         安卓/电脑清单图标
   ├─ icon-512.png                         高清清单图标
   ├─ icon-maskable-512.png                安卓自适应（圆形/方形裁切都不伤主体）
   ├─ apple-touch-icon-180.png             iOS 主屏图标
   └─ favicon-64.png                       浏览器标签页图标
```

打开方式：按 `Win + R`，把上面那行路径粘进去回车，就是资源管理器里的这个文件夹。
（只上传 **这个文件夹里面的东西**，不要上传 `gh-pages` 这个文件夹本身——否则 GitHub 上会多一层 `gh-pages/` 子目录，网址就变成 `/gh-pages/`，容易 404。）

## 需要你配置 / 可能要改的项

**A. GitHub 上要设的（一次性，4 处）**

| 位置 | 设成什么 | 说明 |
|---|---|---|
| 新建仓库时的可见性 | **Public** | Private 仓库用 Pages 需要付费版，免费账号会直接不给开 |
| 仓库名 | 比如 `earth-online` | 决定最终网址 `https://<用户名>.github.io/<仓库名>/` |
| Settings → Pages → Source | **Deploy from a branch** | 不要选 GitHub Actions |
| Settings → Pages → Branch | **main** + **/(root)** | 老账号的默认分支可能叫 `master`，下拉里看到哪个选哪个 |
| Settings → Pages → Enforce HTTPS | 保持勾选（默认） | PWA 安装要求 https，不勾就装不了 |

**B. 代码里可能要改的（现在都已配好，一般不用动）**

| 文件 | 位置 | 作用 | 什么时候改 |
|---|---|---|---|
| `index.html` | `var TARGET = "..."` | 点击/自动跳转的目标地址 | 工作台发布链接变了才改 |
| `index.html` | `goSoon(200)` | 首次打开停留 200ms 再跳 | 想留更多时间看安装提示就调大 |
| `index.html` | `<title>` / `.sub` | 卡片上的标题和副标题 | 想改文案 |
| `manifest.json` | `name` / `short_name` | 安装后显示的应用名 | 改名时两处一起改，`short_name` 建议 ≤6 字 |
| `manifest.json` | `theme_color` / `background_color` | 独立窗口顶栏与启动底色 | 换配色时改（当前 `#d8edf6` 天空蓝） |
| `sw.js` | `var CACHE = "earth-online-v1"` | 缓存版本号 | **改过上面任何文件都要 +1**（v2、v3…），否则手机端继续用旧缓存 |
| `icons/*.png` | 同名替换 | 图标 | 换图标时 5 张一起换，文件名别改 |

## 部署到 GitHub Pages（约 5 分钟）

### Step 0 · 有 GitHub 账号

没有就去 https://github.com 注册（邮箱 + 密码，2 分钟）。已有账号直接下一步。

### Step 1 · 建仓库

右上角 `+` → **New repository**

| 字段 | 填什么 |
|---|---|
| Repository name | `earth-online`（小写、连字符，别用中文和空格） |
| Description | 随便，可留空 |
| 可见性 | 选 **Public** ← 必须 |
| Initialize this repository with | 全部**不勾**（README、.gitignore、license 都不勾） |

点 **Create repository**。

### Step 2 · 上传文件（最容易错的一步）

仓库页 → **Add file** → **Upload files** → 打开资源管理器，进到

```
C:\Users\perla\Documents\陈佩玲\个人工作台\build\gh-pages\
```

全选里面的 **4 个文件 + icons 文件夹**（`index.html`、`manifest.json`、`sw.js`、`README.md`、`icons`），拖到浏览器上传区 → 底部 **Commit changes**。

**目标是长成这样**（文件名直接挂在仓库根目录下）：

```
仓库根目录/
├─ index.html        ← 必须在根，不能在里面套一层
├─ manifest.json
├─ sw.js
├─ README.md
└─ icons/
   ├─ icon-192.png
   └─ …
```

**不要这样传**（把 `gh-pages` 文件夹本身拖进去 → 所有文件被套进 `gh-pages/` 子目录）：

```
仓库根目录/
└─ gh-pages/         ← ✗ 多了一层，网址会变 /gh-pages/，且 Pages 根路径打开是 404
   └─ index.html
```

传完刷新仓库首页，看到 `index.html` 和 `icons` 并排出现在文件列表里就对了。
如果多套了一层，删掉那个文件夹重传即可（点进 `gh-pages` → 右上角 `...` → Delete directory）。

### Step 3 · 打开 Pages

仓库页 → 顶部 **Settings** → 左侧栏 **Pages**

| 项 | 设成 |
|---|---|
| Source | **Deploy from a branch** |
| Branch | **main**（若只有 `master` 就选 `master`） |
| 目录 | **/ (root)** |
| Custom domain | 留空 |

点 **Save**。

### Step 4 · 拿网址并验证

回到 **Settings → Pages**，顶部会出现绿底提示「Your site is live at …」，网址是：

```
https://<你的GitHub用户名>.github.io/earth-online/
```

（首次发布要等 1~2 分钟，期间刷新页面看状态从 `building` 变成 `live`。）

验证三件事：
1. 电脑浏览器打开这个网址 → 看到「地球 Online」卡片 = 成功
2. 访问 `网址 + manifest.json` → 能看到一段 JSON = 清单没丢
3. F12 → Application → Manifest，能看到图标和「Install」按钮 = PWA 可安装

### Step 5 · 装成独立图标

见下一节。**先在这一步用 Chrome 打开工作台并登录一次**，之后装出来的图标点开就是登录态。

## 装成独立图标

**安卓（Chrome）**
1. Chrome（不要微信）打开上面的网址 → 页面会给出「安装为独立图标」按钮，点它 → 确认安装
   - 若没看到按钮：Chrome 菜单 ⋮ → **安装应用 / 添加到主屏幕**
2. 主屏出现地球图标；之后点它 → 直接进工作台

**电脑（Chrome / Edge）**
1. 浏览器打开该网址 → 地址栏右侧出现 **⊕ 安装**（或菜单 → 安装此站点为应用）
2. 安装后会自动创建**桌面快捷方式和开始菜单项**，图标 = 地球图标
3. 打开是**无地址栏的独立窗口**，点图标直接进

**iPhone（Safari）**
Safari 打开 → 分享 → **添加到主屏幕**（用 `apple-touch-icon-180.png`），图标同样是地球。

## 关于「登录」

- **跳转这一跳去不掉**：工作台的数据读写依赖 WorkBuddy 容器注入的运行时，页面只有放在它的容器里才能访问云表；把页面 iframe 嵌到我们自己的站点会被它的 CSP（`frame-ancestors` 只允许自家域）拦成白屏。所以只能是「我们的图标 → 跳进工作台」。
- **但二次登录可以避免**：登录态是浏览器的 cookie。在 Chrome/Edge 里登录过一次后，PWA 打开会直接进内容，不会每次都让你登录。
  - 首次使用：让壳页跳到工作台 → 登录一次 → 之后长期保持
  - 别清理该站点的 Cookie / 网站数据，否则要重新登录
- **微信是例外**：微信内置浏览器是独立容器，进不去工作台。壳页在微信里会自动改成引导页（右上角 ··· → 在浏览器打开 / 复制链接），不会盲目跳转。
- 桌面 PWA 跳到外域时，Chrome 会在窗口顶部显示一行小地址栏，这是正常行为，不影响使用。

## 常见坑（对号入座）

| 现象 | 原因 | 解决 |
|---|---|---|
| 打开网址是 **404** | 文件被套进了 `gh-pages/` 子目录，或 Pages 分支/目录选错 | 仓库根目录必须直接躺着 `index.html`；Pages 选 `main` + `/ (root)` |
| 打开是**白屏 / 只有文字没有图标** | 图标路径大小写不符（Linux 构建机区分大小写，Windows 不区分） | `icons/` 全部小写，引用也全小写 |
| 页面正常但**没有「安装应用」**选项 | ① 不是 https ② `sw.js` 没注册成功 ③ 用微信/QQ 内置浏览器打开 | 用 Chrome；地址栏有锁图标才算 https；F12 → Application → Service Workers 看有没有报错 |
| Pages 设置里**找不到 Branch 下拉** | Source 还停在 `GitHub Actions` | 先把 Source 改回 `Deploy from a branch` |
| 改了图标/文案，**手机还是旧的** | Service Worker 缓存 | `sw.js` 里 `earth-online-v1` 改成 `v2` 重新上传，手机上卸载重装 |
| 仓库是 Private，Pages 开不起来 | 免费账号不支持私有仓库发布 | 改 Public：Settings → 最下方 Danger Zone → Change visibility |
| `github.io` 打不开或很慢 | 国内网络对 GitHub Pages 时好时坏 | 见下节「换个托管」 |

## 换个托管（github.io 不通时）

这套文件是纯静态的，**放到任何静态托管都能跑**，只改上传位置、不用改代码：

- **腾讯云 CloudStudio / EdgeOne Pages**：国内访问快，支持直接传文件夹，也支持绑定自己的域名
- **Vercel / Netlify**：拖文件夹即可，自动 https，国内速度一般
- **自己的服务器 / 对象存储**：静态网站托管 + 强制 https

换托管后唯一要注意的：**必须 https**（否则 Chrome 不给安装 PWA），并且 `manifest.json`、`sw.js` 要能通过 `https://域名/manifest.json`、`/sw.js` 直接访问。

## 更换跳转目标 / 图标

- 跳转地址：`index.html` 里 `var TARGET = "..."`
- 图标：替换 `icons/` 下的 PNG，然后把 `sw.js` 里 `CACHE = "earth-online-v1"` 版本号 +1（否则手机端会拿到旧缓存）

## 本地预览

`python -m http.server 8080`，访问 `http://localhost:8080`（本地判定为预览模式，不自动跳转）。
